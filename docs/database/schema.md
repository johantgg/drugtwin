# Database Schema Documentation

## 📊 Database Overview

Drug Twin menggunakan MySQL 8.0+ sebagai database utama dengan design yang mengikuti prinsip normalisasi untuk memastikan integritas data dan performa optimal.

## 🗄️ Entity Relationship Diagram

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│    Users    │    │  Patients   │    │Medications  │
│             │    │             │    │             │
├─────────────┤    ├─────────────┤    ├─────────────┤
│ id (PK)     │    │ id (PK)     │    │ id (PK)     │
│ name        │◄──►│ user_id(FK) │    │ name        │
│ email       │    │ mrn         │    │ generic_name│
│ role        │    │ birth_date  │    │ brand_name  │
│ password    │    │ gender      │    │ dosage_form │
│ qr_code     │    │ phone       │    │ strength    │
│ is_active   │    │ address     │    │ unit        │
│ created_at  │    │ created_at  │    │ description │
│ updated_at  │    │ updated_at  │    │ created_at  │
└─────────────┘    └─────────────┘    │ updated_at  │
                                      └─────────────┘
       │                   │                  │
       │                   │                  │
       ▼                   ▼                  ▼
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│Prescriptions│    │Lab_Results  │    │Drug_Dosages │
│             │    │             │    │             │
├─────────────┤    ├─────────────┤    ├─────────────┤
│ id (PK)     │    │ id (PK)     │    │ id (PK)     │
│ patient_id  │    │ patient_id  │    │medication_id│
│ doctor_id   │    │ test_name   │    │ min_dose    │
│ pharmacist_id│   │ test_value  │    │ max_dose    │
│ status      │    │ unit        │    │ age_min     │
│ ai_rec      │    │ normal_min  │    │ age_max     │
│ doctor_notes│    │ normal_max  │    │ gfr_min     │
│ pharmacy_not│    │ test_date   │    │ gfr_max     │
│ created_at  │    │ created_at  │    │ created_at  │
│ updated_at  │    │ updated_at  │    │ updated_at  │
└─────────────┘    └─────────────┘    └─────────────┘
```

## 📋 Detailed Table Schemas

### 1. Users Table
**Purpose**: Menyimpan data semua pengguna sistem (Admin, Dokter, Apoteker, Pasien)

```sql
CREATE TABLE users (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    email_verified_at TIMESTAMP NULL,
    password VARCHAR(255) NOT NULL,
    role ENUM('admin', 'doctor', 'pharmacist', 'patient') NOT NULL,
    phone VARCHAR(20) NULL,
    license_number VARCHAR(100) NULL, -- untuk dokter & apoteker
    specialization VARCHAR(255) NULL, -- untuk dokter
    qr_code VARCHAR(255) NULL, -- untuk pasien
    is_active BOOLEAN DEFAULT true,
    last_login_at TIMESTAMP NULL,
    remember_token VARCHAR(100) NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    INDEX idx_role (role),
    INDEX idx_email (email),
    INDEX idx_qr_code (qr_code)
);
```

### 2. Patients Table
**Purpose**: Menyimpan data klinis pasien

```sql
CREATE TABLE patients (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT UNSIGNED NOT NULL,
    mrn VARCHAR(50) UNIQUE NOT NULL, -- Medical Record Number
    birth_date DATE NOT NULL,
    gender ENUM('male', 'female') NOT NULL,
    weight DECIMAL(5,2) NULL, -- kg
    height DECIMAL(5,2) NULL, -- cm
    address TEXT NULL,
    emergency_contact_name VARCHAR(255) NULL,
    emergency_contact_phone VARCHAR(20) NULL,
    insurance_number VARCHAR(100) NULL,
    allergies TEXT NULL, -- JSON format
    medical_history TEXT NULL, -- JSON format
    admission_date TIMESTAMP NULL,
    discharge_date TIMESTAMP NULL,
    admission_type ENUM('inpatient', 'outpatient') DEFAULT 'outpatient',
    assigned_doctor_id BIGINT UNSIGNED NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (assigned_doctor_id) REFERENCES users(id) ON DELETE SET NULL,
    INDEX idx_mrn (mrn),
    INDEX idx_assigned_doctor (assigned_doctor_id),
    INDEX idx_admission_type (admission_type)
);
```

### 3. Medications Table
**Purpose**: Master data obat-obatan

```sql
CREATE TABLE medications (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    generic_name VARCHAR(255) NOT NULL,
    brand_name VARCHAR(255) NULL,
    drug_class VARCHAR(255) NOT NULL,
    dosage_form ENUM('tablet', 'capsule', 'syrup', 'injection', 'cream', 'inhaler') NOT NULL,
    strength VARCHAR(100) NOT NULL, -- e.g., "500mg", "250mg/5ml"
    unit VARCHAR(50) NOT NULL, -- mg, ml, g, etc.
    route_of_administration VARCHAR(100) NOT NULL, -- oral, IV, IM, topical, etc.
    contraindications TEXT NULL, -- JSON format
    side_effects TEXT NULL, -- JSON format
    drug_interactions TEXT NULL, -- JSON format
    pregnancy_category VARCHAR(10) NULL,
    storage_requirements TEXT NULL,
    manufacturer VARCHAR(255) NULL,
    is_active BOOLEAN DEFAULT true,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    INDEX idx_generic_name (generic_name),
    INDEX idx_drug_class (drug_class),
    INDEX idx_active (is_active)
);
```

### 4. Drug_Dosages Table
**Purpose**: Aturan dosis berdasarkan kondisi pasien

```sql
CREATE TABLE drug_dosages (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    medication_id BIGINT UNSIGNED NOT NULL,
    indication VARCHAR(255) NOT NULL, -- diagnosis/kondisi
    age_min INT NULL, -- tahun
    age_max INT NULL, -- tahun
    weight_min DECIMAL(5,2) NULL, -- kg
    weight_max DECIMAL(5,2) NULL, -- kg
    gfr_min INT NULL, -- kidney function
    gfr_max INT NULL,
    min_dose DECIMAL(8,2) NOT NULL,
    max_dose DECIMAL(8,2) NOT NULL,
    frequency VARCHAR(50) NOT NULL, -- "once daily", "twice daily", etc.
    duration_days INT NULL,
    special_instructions TEXT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    FOREIGN KEY (medication_id) REFERENCES medications(id) ON DELETE CASCADE,
    INDEX idx_indication (indication),
    INDEX idx_age_range (age_min, age_max),
    INDEX idx_gfr_range (gfr_min, gfr_max)
);
```

### 5. Lab_Results Table
**Purpose**: Hasil laboratorium pasien

```sql
CREATE TABLE lab_results (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    patient_id BIGINT UNSIGNED NOT NULL,
    test_name VARCHAR(255) NOT NULL,
    test_value DECIMAL(10,3) NOT NULL,
    unit VARCHAR(50) NOT NULL,
    normal_range_min DECIMAL(10,3) NULL,
    normal_range_max DECIMAL(10,3) NULL,
    status ENUM('normal', 'low', 'high', 'critical') DEFAULT 'normal',
    test_date TIMESTAMP NOT NULL,
    lab_technician VARCHAR(255) NULL,
    notes TEXT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    FOREIGN KEY (patient_id) REFERENCES patients(id) ON DELETE CASCADE,
    INDEX idx_patient_test (patient_id, test_name),
    INDEX idx_test_date (test_date),
    INDEX idx_status (status)
);
```

### 6. Prescriptions Table
**Purpose**: Resep obat dan workflow persetujuan

```sql
CREATE TABLE prescriptions (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    patient_id BIGINT UNSIGNED NOT NULL,
    doctor_id BIGINT UNSIGNED NOT NULL,
    pharmacist_id BIGINT UNSIGNED NULL,
    prescription_number VARCHAR(100) UNIQUE NOT NULL,
    status ENUM('pending_ai', 'pending_doctor', 'pending_pharmacist', 'approved', 'rejected', 'dispensed') DEFAULT 'pending_ai',
    ai_recommendation TEXT NULL, -- JSON format
    ai_confidence_score DECIMAL(3,2) NULL,
    ai_warnings TEXT NULL, -- JSON format
    doctor_notes TEXT NULL,
    pharmacist_notes TEXT NULL,
    total_medications INT DEFAULT 0,
    prescribed_at TIMESTAMP NULL,
    validated_at TIMESTAMP NULL,
    dispensed_at TIMESTAMP NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    FOREIGN KEY (patient_id) REFERENCES patients(id) ON DELETE CASCADE,
    FOREIGN KEY (doctor_id) REFERENCES users(id) ON DELETE RESTRICT,
    FOREIGN KEY (pharmacist_id) REFERENCES users(id) ON DELETE SET NULL,
    INDEX idx_status (status),
    INDEX idx_prescription_number (prescription_number),
    INDEX idx_patient_doctor (patient_id, doctor_id)
);
```

### 7. Prescription_Items Table
**Purpose**: Detail obat dalam resep

```sql
CREATE TABLE prescription_items (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    prescription_id BIGINT UNSIGNED NOT NULL,
    medication_id BIGINT UNSIGNED NOT NULL,
    dosage VARCHAR(100) NOT NULL, -- "500mg"
    frequency VARCHAR(100) NOT NULL, -- "twice daily"
    duration_days INT NOT NULL,
    quantity INT NOT NULL,
    instructions TEXT NULL,
    substituted_medication_id BIGINT UNSIGNED NULL, -- jika ada substitusi
    substitution_reason TEXT NULL,
    status ENUM('active', 'discontinued', 'completed') DEFAULT 'active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    FOREIGN KEY (prescription_id) REFERENCES prescriptions(id) ON DELETE CASCADE,
    FOREIGN KEY (medication_id) REFERENCES medications(id) ON DELETE RESTRICT,
    FOREIGN KEY (substituted_medication_id) REFERENCES medications(id) ON DELETE SET NULL,
    INDEX idx_prescription (prescription_id),
    INDEX idx_status (status)
);
```

### 8. Medication_Adherence Table
**Purpose**: Tracking kepatuhan minum obat pasien

```sql
CREATE TABLE medication_adherence (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    prescription_item_id BIGINT UNSIGNED NOT NULL,
    patient_id BIGINT UNSIGNED NOT NULL,
    scheduled_time TIMESTAMP NOT NULL,
    taken_time TIMESTAMP NULL,
    status ENUM('scheduled', 'taken', 'missed', 'skipped') DEFAULT 'scheduled',
    notes TEXT NULL,
    reminder_sent BOOLEAN DEFAULT false,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    FOREIGN KEY (prescription_item_id) REFERENCES prescription_items(id) ON DELETE CASCADE,
    FOREIGN KEY (patient_id) REFERENCES patients(id) ON DELETE CASCADE,
    INDEX idx_patient_schedule (patient_id, scheduled_time),
    INDEX idx_status (status)
);
```

### 9. Chat_Conversations Table
**Purpose**: Percakapan antar pengguna

```sql
CREATE TABLE chat_conversations (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    type ENUM('doctor_patient', 'doctor_pharmacist', 'pharmacist_patient', 'ai_chat') NOT NULL,
    patient_id BIGINT UNSIGNED NULL,
    doctor_id BIGINT UNSIGNED NULL,
    pharmacist_id BIGINT UNSIGNED NULL,
    title VARCHAR(255) NULL,
    status ENUM('active', 'closed', 'archived') DEFAULT 'active',
    last_message_at TIMESTAMP NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    FOREIGN KEY (patient_id) REFERENCES patients(id) ON DELETE CASCADE,
    FOREIGN KEY (doctor_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (pharmacist_id) REFERENCES users(id) ON DELETE CASCADE,
    INDEX idx_type (type),
    INDEX idx_participants (patient_id, doctor_id, pharmacist_id)
);
```

### 10. Chat_Messages Table
**Purpose**: Pesan dalam percakapan

```sql
CREATE TABLE chat_messages (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    conversation_id BIGINT UNSIGNED NOT NULL,
    sender_id BIGINT UNSIGNED NULL, -- NULL untuk AI messages
    sender_type ENUM('user', 'ai') DEFAULT 'user',
    message TEXT NOT NULL,
    message_type ENUM('text', 'image', 'file', 'prescription') DEFAULT 'text',
    file_path VARCHAR(500) NULL,
    is_read BOOLEAN DEFAULT false,
    read_at TIMESTAMP NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    FOREIGN KEY (conversation_id) REFERENCES chat_conversations(id) ON DELETE CASCADE,
    FOREIGN KEY (sender_id) REFERENCES users(id) ON DELETE SET NULL,
    INDEX idx_conversation (conversation_id),
    INDEX idx_created_at (created_at)
);
```

### 11. Educational_Content Table
**Purpose**: Konten edukasi untuk pasien

```sql
CREATE TABLE educational_content (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255) NOT NULL,
    content TEXT NOT NULL,
    content_type ENUM('article', 'video', 'infographic', 'external_link') NOT NULL,
    category VARCHAR(100) NOT NULL, -- "medication_safety", "disease_info", etc.
    target_audience ENUM('all', 'patients', 'doctors', 'pharmacists') DEFAULT 'patients',
    external_url VARCHAR(500) NULL,
    thumbnail_path VARCHAR(500) NULL,
    duration_minutes INT NULL, -- untuk video
    author VARCHAR(255) NULL,
    source VARCHAR(255) NULL, -- Kementerian Kesehatan, WHO, etc.
    is_published BOOLEAN DEFAULT false,
    view_count INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    INDEX idx_category (category),
    INDEX idx_published (is_published),
    INDEX idx_target_audience (target_audience)
);
```

### 12. Appointment_Reminders Table
**Purpose**: Pengingat kunjungan pasien

```sql
CREATE TABLE appointment_reminders (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    patient_id BIGINT UNSIGNED NOT NULL,
    doctor_id BIGINT UNSIGNED NULL,
    appointment_date TIMESTAMP NOT NULL,
    appointment_type ENUM('follow_up', 'consultation', 'lab_check', 'prescription_refill') NOT NULL,
    reminder_date TIMESTAMP NOT NULL,
    status ENUM('scheduled', 'reminded', 'completed', 'cancelled', 'missed') DEFAULT 'scheduled',
    notes TEXT NULL,
    qr_code_used BOOLEAN DEFAULT false,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    FOREIGN KEY (patient_id) REFERENCES patients(id) ON DELETE CASCADE,
    FOREIGN KEY (doctor_id) REFERENCES users(id) ON DELETE SET NULL,
    INDEX idx_patient_date (patient_id, appointment_date),
    INDEX idx_reminder_date (reminder_date),
    INDEX idx_status (status)
);
```

### 13. Audit_Logs Table
**Purpose**: Log aktivitas sistem untuk keamanan

```sql
CREATE TABLE audit_logs (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT UNSIGNED NULL,
    action VARCHAR(255) NOT NULL,
    table_name VARCHAR(100) NOT NULL,
    record_id BIGINT UNSIGNED NULL,
    old_values JSON NULL,
    new_values JSON NULL,
    ip_address VARCHAR(45) NULL,
    user_agent TEXT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE SET NULL,
    INDEX idx_user_action (user_id, action),
    INDEX idx_table_record (table_name, record_id),
    INDEX idx_created_at (created_at)
);
```

### 14. System_Settings Table
**Purpose**: Konfigurasi sistem

```sql
CREATE TABLE system_settings (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    key_name VARCHAR(255) UNIQUE NOT NULL,
    key_value TEXT NULL,
    description TEXT NULL,
    data_type ENUM('string', 'integer', 'boolean', 'json') DEFAULT 'string',
    is_public BOOLEAN DEFAULT false, -- apakah bisa diakses frontend
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    INDEX idx_key_name (key_name),
    INDEX idx_public (is_public)
);
```

## 🔐 Database Security & Performance

### Indexes
```sql
-- Performance indexes
CREATE INDEX idx_users_role_active ON users(role, is_active);
CREATE INDEX idx_prescriptions_status_date ON prescriptions(status, created_at);
CREATE INDEX idx_lab_results_patient_date ON lab_results(patient_id, test_date DESC);
CREATE INDEX idx_adherence_patient_time ON medication_adherence(patient_id, scheduled_time);

-- Search indexes
CREATE FULLTEXT INDEX idx_medications_search ON medications(name, generic_name, brand_name);
CREATE FULLTEXT INDEX idx_educational_content_search ON educational_content(title, content);
```

### Views untuk Reporting
```sql
-- Patient Summary View
CREATE VIEW patient_summary AS
SELECT 
    p.id,
    p.mrn,
    u.name as patient_name,
    p.birth_date,
    TIMESTAMPDIFF(YEAR, p.birth_date, CURDATE()) as age,
    p.gender,
    p.admission_type,
    doc.name as assigned_doctor,
    COUNT(DISTINCT pr.id) as total_prescriptions,
    COUNT(DISTINCT lr.id) as total_lab_results
FROM patients p
JOIN users u ON p.user_id = u.id
LEFT JOIN users doc ON p.assigned_doctor_id = doc.id
LEFT JOIN prescriptions pr ON p.id = pr.patient_id
LEFT JOIN lab_results lr ON p.id = lr.patient_id
GROUP BY p.id;

-- Medication Adherence Summary View
CREATE VIEW adherence_summary AS
SELECT 
    patient_id,
    COUNT(*) as total_doses,
    SUM(CASE WHEN status = 'taken' THEN 1 ELSE 0 END) as taken_doses,
    ROUND((SUM(CASE WHEN status = 'taken' THEN 1 ELSE 0 END) / COUNT(*)) * 100, 2) as adherence_percentage
FROM medication_adherence
GROUP BY patient_id;
```

## 📊 Data Seeding Strategy

### 1. Default System Settings
```sql
INSERT INTO system_settings (key_name, key_value, description, data_type) VALUES
('app_name', 'Drug Twin', 'Application name', 'string'),
('ai_confidence_threshold', '0.75', 'Minimum AI confidence for auto-approval', 'string'),
('reminder_hours_before', '24,2,0.5', 'Hours before appointment to send reminders', 'string'),
('max_prescription_duration', '90', 'Maximum prescription duration in days', 'integer');
```

### 2. Sample Medications
```sql
INSERT INTO medications (name, generic_name, brand_name, drug_class, dosage_form, strength, unit, route_of_administration) VALUES
('Metformin HCl 500mg', 'Metformin Hydrochloride', 'Glucophage', 'Antidiabetic', 'tablet', '500', 'mg', 'oral'),
('Amlodipine 5mg', 'Amlodipine Besylate', 'Norvasc', 'Antihypertensive', 'tablet', '5', 'mg', 'oral'),
('Simvastatin 20mg', 'Simvastatin', 'Zocor', 'Statin', 'tablet', '20', 'mg', 'oral');
```

### 3. Sample Drug Dosages
```sql
INSERT INTO drug_dosages (medication_id, indication, age_min, age_max, gfr_min, min_dose, max_dose, frequency) VALUES
(1, 'Type 2 Diabetes', 18, 65, 60, 500, 2000, 'twice daily'),
(2, 'Hypertension', 18, 80, 30, 2.5, 10, 'once daily'),
(3, 'Hyperlipidemia', 18, 75, 30, 10, 40, 'once daily');
```

## 🚀 Migration Strategy

### Development Sequence
1. Create base tables (users, patients, medications)
2. Create prescription workflow tables
3. Create communication tables (chat)
4. Create tracking tables (adherence, appointments)
5. Create content and audit tables
6. Add indexes and views
7. Seed initial data

### Production Deployment
1. Backup existing data
2. Run migrations in transaction
3. Verify data integrity
4. Update application configuration
5. Test all endpoints
6. Monitor performance

---
**Next Steps**: [API Endpoints Documentation](../api/endpoints.md)