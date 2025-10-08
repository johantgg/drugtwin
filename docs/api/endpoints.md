# API Endpoints Documentation

## 🔗 Base URL
```
Development: http://localhost:8000/api/v1
Production: https://api.drugtwin.com/v1
```

## 🔑 Authentication
Drug Twin menggunakan Laravel Sanctum untuk authentication dengan Bearer Token.

```http
Authorization: Bearer {token}
Content-Type: application/json
Accept: application/json
```

## 📋 Response Format

### Success Response
```json
{
    "success": true,
    "message": "Success message",
    "data": {},
    "meta": {
        "pagination": {},
        "timestamp": "2025-10-08T10:30:00Z"
    }
}
```

### Error Response
```json
{
    "success": false,
    "message": "Error message",
    "errors": {
        "field": ["Error details"]
    },
    "error_code": "VALIDATION_ERROR",
    "meta": {
        "timestamp": "2025-10-08T10:30:00Z"
    }
}
```

## 🔐 Authentication Endpoints

### POST /auth/login
Login untuk semua jenis user.

**Request:**
```json
{
    "email": "doctor@example.com",
    "password": "password123",
    "device_name": "Web Browser"
}
```

**Response:**
```json
{
    "success": true,
    "message": "Login successful",
    "data": {
        "user": {
            "id": 1,
            "name": "Dr. John Doe",
            "email": "doctor@example.com",
            "role": "doctor",
            "qr_code": null
        },
        "token": "1|xyz123abc456def789",
        "expires_at": "2025-10-15T10:30:00Z"
    }
}
```

### POST /auth/logout
Logout dan revoke token.

**Headers:** `Authorization: Bearer {token}`

**Response:**
```json
{
    "success": true,
    "message": "Logged out successfully"
}
```

### POST /auth/qr-login
Login menggunakan QR Code (khusus pasien).

**Request:**
```json
{
    "qr_code": "QR123456789",
    "device_name": "Mobile App"
}
```

### GET /auth/me
Get current user information.

**Headers:** `Authorization: Bearer {token}`

**Response:**
```json
{
    "success": true,
    "data": {
        "id": 1,
        "name": "Dr. John Doe",
        "email": "doctor@example.com",
        "role": "doctor",
        "specialization": "Internal Medicine",
        "license_number": "DOC123456"
    }
}
```

## 👥 User Management Endpoints

### GET /users
Get list of users (Admin only).

**Query Parameters:**
- `role`: filter by role (admin, doctor, pharmacist, patient)
- `search`: search by name or email
- `page`: pagination page number
- `per_page`: items per page (default: 15)

**Response:**
```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "name": "Dr. John Doe",
            "email": "doctor@example.com",
            "role": "doctor",
            "specialization": "Internal Medicine",
            "is_active": true,
            "last_login_at": "2025-10-08T09:00:00Z"
        }
    ],
    "meta": {
        "pagination": {
            "current_page": 1,
            "per_page": 15,
            "total": 50,
            "last_page": 4
        }
    }
}
```

### POST /users
Create new user (Admin only).

**Request:**
```json
{
    "name": "Dr. Jane Smith",
    "email": "jane.smith@example.com",
    "password": "password123",
    "role": "doctor",
    "phone": "+62812345678",
    "license_number": "DOC789123",
    "specialization": "Cardiology"
}
```

### PUT /users/{id}
Update user information.

**Request:**
```json
{
    "name": "Dr. Jane Smith Updated",
    "phone": "+62812345679",
    "specialization": "Interventional Cardiology"
}
```

### DELETE /users/{id}
Soft delete user (Admin only).

## 🏥 Patient Management Endpoints

### GET /patients
Get list of patients.

**Query Parameters:**
- `search`: search by name, MRN, or phone
- `admission_type`: inpatient/outpatient
- `assigned_doctor_id`: filter by assigned doctor
- `page`, `per_page`: pagination

**Response:**
```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "user": {
                "id": 10,
                "name": "John Patient",
                "email": "patient@example.com"
            },
            "mrn": "MRN2025001",
            "birth_date": "1985-05-15",
            "age": 40,
            "gender": "male",
            "admission_type": "inpatient",
            "assigned_doctor": {
                "id": 2,
                "name": "Dr. Jane Smith"
            },
            "latest_lab_results": [
                {
                    "test_name": "GFR",
                    "test_value": 85.5,
                    "unit": "mL/min/1.73m²",
                    "status": "normal"
                }
            ]
        }
    ]
}
```

### POST /patients
Create new patient.

**Request:**
```json
{
    "user": {
        "name": "John Patient",
        "email": "patient@example.com",
        "password": "password123",
        "phone": "+62812345678"
    },
    "mrn": "MRN2025001",
    "birth_date": "1985-05-15",
    "gender": "male",
    "weight": 70.5,
    "height": 175.0,
    "address": "Jakarta, Indonesia",
    "allergies": ["Penicillin", "Aspirin"],
    "medical_history": ["Hypertension", "Diabetes Type 2"],
    "admission_type": "inpatient",
    "assigned_doctor_id": 2
}
```

### GET /patients/{id}
Get specific patient details.

**Response:**
```json
{
    "success": true,
    "data": {
        "id": 1,
        "user": {
            "id": 10,
            "name": "John Patient",
            "email": "patient@example.com",
            "qr_code": "QR123456789"
        },
        "mrn": "MRN2025001",
        "birth_date": "1985-05-15",
        "age": 40,
        "gender": "male",
        "weight": 70.5,
        "height": 175.0,
        "bmi": 23.0,
        "allergies": ["Penicillin", "Aspirin"],
        "medical_history": ["Hypertension", "Diabetes Type 2"],
        "admission_type": "inpatient",
        "admission_date": "2025-10-08T08:00:00Z",
        "assigned_doctor": {
            "id": 2,
            "name": "Dr. Jane Smith",
            "specialization": "Internal Medicine"
        },
        "recent_lab_results": [
            {
                "test_name": "GFR",
                "test_value": 85.5,
                "unit": "mL/min/1.73m²",
                "status": "normal",
                "test_date": "2025-10-07T10:00:00Z"
            },
            {
                "test_name": "HbA1c",
                "test_value": 7.2,
                "unit": "%",
                "status": "high",
                "test_date": "2025-10-07T10:00:00Z"
            }
        ],
        "active_prescriptions": [
            {
                "id": 1,
                "prescription_number": "RX2025001",
                "status": "approved",
                "total_medications": 3,
                "prescribed_at": "2025-10-08T09:00:00Z"
            }
        ]
    }
}
```

### PUT /patients/{id}
Update patient information.

### POST /patients/{id}/generate-qr
Generate QR code for patient (Admin only).

**Response:**
```json
{
    "success": true,
    "data": {
        "qr_code": "QR123456789",
        "qr_image_url": "/storage/qr-codes/QR123456789.png"
    }
}
```

## 🧪 Lab Results Endpoints

### GET /patients/{patient_id}/lab-results
Get lab results for specific patient.

**Query Parameters:**
- `test_name`: filter by test name
- `from_date`, `to_date`: date range
- `status`: normal/low/high/critical

### POST /patients/{patient_id}/lab-results
Add new lab result.

**Request:**
```json
{
    "test_name": "GFR",
    "test_value": 85.5,
    "unit": "mL/min/1.73m²",
    "normal_range_min": 60.0,
    "normal_range_max": 120.0,
    "test_date": "2025-10-08T10:00:00Z",
    "lab_technician": "Lab Tech 1",
    "notes": "Patient was fasting"
}
```

## 💊 Medication Management Endpoints

### GET /medications
Get list of medications.

**Query Parameters:**
- `search`: search by name or generic name
- `drug_class`: filter by drug class
- `dosage_form`: filter by dosage form
- `is_active`: filter active medications

**Response:**
```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "name": "Metformin HCl 500mg",
            "generic_name": "Metformin Hydrochloride",
            "brand_name": "Glucophage",
            "drug_class": "Antidiabetic",
            "dosage_form": "tablet",
            "strength": "500",
            "unit": "mg",
            "route_of_administration": "oral",
            "contraindications": ["Kidney disease", "Liver disease"],
            "side_effects": ["Nausea", "Diarrhea"],
            "is_active": true
        }
    ]
}
```

### POST /medications
Add new medication (Admin/Pharmacist only).

### GET /medications/{id}/dosages
Get dosage recommendations for specific medication.

**Response:**
```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "indication": "Type 2 Diabetes",
            "age_min": 18,
            "age_max": 65,
            "gfr_min": 60,
            "min_dose": 500,
            "max_dose": 2000,
            "frequency": "twice daily",
            "duration_days": 30,
            "special_instructions": "Take with meals"
        }
    ]
}
```

## 📋 Prescription Management Endpoints

### GET /prescriptions
Get list of prescriptions based on user role.

**Query Parameters:**
- `status`: filter by status
- `patient_id`: filter by patient (for doctors/pharmacists)
- `date_from`, `date_to`: date range

**Response:**
```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "prescription_number": "RX2025001",
            "patient": {
                "id": 1,
                "name": "John Patient",
                "mrn": "MRN2025001"
            },
            "doctor": {
                "id": 2,
                "name": "Dr. Jane Smith"
            },
            "pharmacist": {
                "id": 3,
                "name": "Pharmacist John"
            },
            "status": "approved",
            "ai_recommendation": {
                "confidence_score": 0.92,
                "warnings": ["Check kidney function"]
            },
            "total_medications": 3,
            "prescribed_at": "2025-10-08T09:00:00Z",
            "items": [
                {
                    "id": 1,
                    "medication": {
                        "id": 1,
                        "name": "Metformin HCl 500mg"
                    },
                    "dosage": "500mg",
                    "frequency": "twice daily",
                    "duration_days": 30,
                    "quantity": 60,
                    "instructions": "Take with meals"
                }
            ]
        }
    ]
}
```

### POST /prescriptions
Create new prescription (Admin only - triggers AI analysis).

**Request:**
```json
{
    "patient_id": 1,
    "doctor_id": 2,
    "medications": [
        {
            "medication_id": 1,
            "dosage": "500mg",
            "frequency": "twice daily",
            "duration_days": 30,
            "quantity": 60,
            "instructions": "Take with meals"
        }
    ],
    "notes": "Initial prescription for newly diagnosed diabetes"
}
```

### GET /prescriptions/{id}
Get specific prescription details.

### PUT /prescriptions/{id}/doctor-review
Doctor review of AI recommendation.

**Request:**
```json
{
    "action": "approve", // approve, edit, reject
    "medications": [
        {
            "prescription_item_id": 1,
            "dosage": "250mg", // if editing
            "frequency": "once daily",
            "duration_days": 30,
            "quantity": 30,
            "instructions": "Take with breakfast"
        }
    ],
    "doctor_notes": "Reduced dose due to kidney function"
}
```

### PUT /prescriptions/{id}/pharmacist-review
Pharmacist validation of prescription.

**Request:**
```json
{
    "action": "approve", // approve, request_changes
    "medication_substitutions": [
        {
            "prescription_item_id": 1,
            "substituted_medication_id": 5,
            "substitution_reason": "Original medication not available"
        }
    ],
    "pharmacist_notes": "Substituted with generic equivalent",
    "availability_check": true
}
```

## 🤖 AI Recommendation Endpoints

### POST /ai/analyze-prescription
Get AI recommendation for prescription (internal use).

**Request:**
```json
{
    "patient_id": 1,
    "medications": [
        {
            "medication_id": 1,
            "dosage": "500mg",
            "frequency": "twice daily",
            "duration_days": 30
        }
    ]
}
```

**Response:**
```json
{
    "success": true,
    "data": {
        "confidence_score": 0.92,
        "recommendations": [
            {
                "medication_id": 1,
                "recommended_dosage": "500mg",
                "recommended_frequency": "twice daily",
                "adjustments": [],
                "reasoning": "Appropriate dose for patient age and kidney function"
            }
        ],
        "warnings": [
            {
                "type": "kidney_function",
                "severity": "medium",
                "message": "Monitor kidney function regularly"
            }
        ],
        "drug_interactions": [],
        "contraindications": []
    }
}
```

### POST /ai/chat
AI Chat for patients and healthcare providers.

**Request:**
```json
{
    "message": "What are the side effects of Metformin?",
    "context": {
        "user_role": "patient",
        "patient_id": 1,
        "conversation_id": 5
    }
}
```

**Response:**
```json
{
    "success": true,
    "data": {
        "response": "Metformin commonly causes gastrointestinal side effects including nausea, diarrhea, and stomach upset. These usually improve over time...",
        "confidence": 0.95,
        "sources": ["medical_literature", "drug_database"],
        "follow_up_questions": [
            "Are you experiencing any of these side effects?",
            "Would you like tips on how to minimize side effects?"
        ]
    }
}
```

## 📱 Medication Adherence Endpoints

### GET /patients/{patient_id}/adherence
Get medication adherence for patient.

**Query Parameters:**
- `prescription_id`: filter by prescription
- `from_date`, `to_date`: date range

**Response:**
```json
{
    "success": true,
    "data": {
        "patient_id": 1,
        "overall_adherence": 85.5,
        "prescriptions": [
            {
                "prescription_id": 1,
                "medication_name": "Metformin HCl 500mg",
                "adherence_percentage": 90.0,
                "total_doses": 60,
                "taken_doses": 54,
                "missed_doses": 6,
                "upcoming_doses": [
                    {
                        "id": 100,
                        "scheduled_time": "2025-10-08T08:00:00Z",
                        "status": "scheduled"
                    }
                ]
            }
        ]
    }
}
```

### POST /adherence/{adherence_id}/mark-taken
Mark medication as taken by patient.

**Request:**
```json
{
    "taken_time": "2025-10-08T08:15:00Z",
    "notes": "Taken with breakfast"
}
```

### GET /patients/{patient_id}/adherence/schedule
Get medication schedule for patient.

**Response:**
```json
{
    "success": true,
    "data": {
        "today": [
            {
                "id": 100,
                "medication_name": "Metformin HCl 500mg",
                "dosage": "500mg",
                "scheduled_time": "08:00",
                "status": "scheduled",
                "instructions": "Take with breakfast"
            },
            {
                "id": 101,
                "medication_name": "Metformin HCl 500mg",
                "dosage": "500mg",
                "scheduled_time": "20:00",
                "status": "scheduled",
                "instructions": "Take with dinner"
            }
        ],
        "upcoming_week": [
            // ... upcoming doses
        ]
    }
}
```

## 💬 Chat System Endpoints

### GET /conversations
Get list of conversations for current user.

**Response:**
```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "type": "doctor_patient",
            "title": "Consultation with Dr. Jane Smith",
            "participants": [
                {
                    "id": 1,
                    "name": "John Patient",
                    "role": "patient"
                },
                {
                    "id": 2,
                    "name": "Dr. Jane Smith",
                    "role": "doctor"
                }
            ],
            "last_message": {
                "message": "Please take your medication as prescribed",
                "created_at": "2025-10-08T10:00:00Z"
            },
            "unread_count": 2,
            "status": "active"
        }
    ]
}
```

### POST /conversations
Create new conversation.

**Request:**
```json
{
    "type": "doctor_patient",
    "participant_ids": [1, 2],
    "title": "Medication Consultation"
}
```

### GET /conversations/{id}/messages
Get messages in conversation.

**Response:**
```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "sender": {
                "id": 1,
                "name": "John Patient",
                "role": "patient"
            },
            "message": "I'm experiencing some side effects",
            "message_type": "text",
            "is_read": true,
            "created_at": "2025-10-08T09:30:00Z"
        },
        {
            "id": 2,
            "sender": {
                "id": 2,
                "name": "Dr. Jane Smith",
                "role": "doctor"
            },
            "message": "Can you describe the side effects?",
            "message_type": "text",
            "is_read": false,
            "created_at": "2025-10-08T09:35:00Z"
        }
    ]
}
```

### POST /conversations/{id}/messages
Send message in conversation.

**Request:**
```json
{
    "message": "I feel nauseous after taking the medication",
    "message_type": "text"
}
```

### PUT /messages/{id}/read
Mark message as read.

## 📚 Educational Content Endpoints

### GET /educational-content
Get educational content for patients.

**Query Parameters:**
- `category`: filter by category
- `content_type`: article/video/infographic/external_link
- `search`: search in title and content

**Response:**
```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "title": "Understanding Diabetes Medication",
            "content": "Diabetes medications help control blood sugar levels...",
            "content_type": "article",
            "category": "medication_safety",
            "thumbnail_path": "/storage/thumbnails/diabetes-med.jpg",
            "duration_minutes": null,
            "author": "Dr. Health Expert",
            "source": "Kementerian Kesehatan RI",
            "view_count": 1250,
            "created_at": "2025-10-01T00:00:00Z"
        }
    ]
}
```

### GET /educational-content/{id}
Get specific educational content.

### POST /educational-content/{id}/view
Record content view (analytics).

## 📅 Appointment Reminder Endpoints

### GET /patients/{patient_id}/appointments
Get appointments for patient.

**Response:**
```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "appointment_date": "2025-10-15T10:00:00Z",
            "appointment_type": "follow_up",
            "doctor": {
                "id": 2,
                "name": "Dr. Jane Smith"
            },
            "status": "scheduled",
            "qr_code_used": false,
            "notes": "3-month diabetes follow-up"
        }
    ]
}
```

### POST /appointments
Create new appointment reminder.

### PUT /appointments/{id}/checkin
Patient check-in using QR code.

**Request:**
```json
{
    "qr_code": "QR123456789"
}
```

## 📊 Dashboard & Analytics Endpoints

### GET /dashboard/admin
Admin dashboard data.

**Response:**
```json
{
    "success": true,
    "data": {
        "stats": {
            "total_patients": 1250,
            "active_prescriptions": 850,
            "pending_validations": 25,
            "ai_recommendations_today": 45
        },
        "recent_activities": [
            {
                "type": "new_prescription",
                "patient_name": "John Patient",
                "doctor_name": "Dr. Jane Smith",
                "timestamp": "2025-10-08T10:00:00Z"
            }
        ],
        "ai_performance": {
            "accuracy_rate": 94.5,
            "confidence_average": 0.87,
            "interventions_prevented": 12
        }
    }
}
```

### GET /dashboard/doctor
Doctor dashboard data.

### GET /dashboard/pharmacist
Pharmacist dashboard data.

### GET /dashboard/patient
Patient dashboard data.

## 🔍 Search Endpoints

### GET /search
Global search across entities.

**Query Parameters:**
- `q`: search query
- `type`: patients/medications/prescriptions
- `limit`: result limit

**Response:**
```json
{
    "success": true,
    "data": {
        "patients": [
            {
                "id": 1,
                "name": "John Patient",
                "mrn": "MRN2025001",
                "match_type": "name"
            }
        ],
        "medications": [
            {
                "id": 1,
                "name": "Metformin HCl 500mg",
                "generic_name": "Metformin Hydrochloride",
                "match_type": "name"
            }
        ],
        "prescriptions": []
    }
}
```

## 🚨 Error Codes

| Code | Description | HTTP Status |
|------|-------------|-------------|
| `VALIDATION_ERROR` | Request validation failed | 422 |
| `AUTHENTICATION_FAILED` | Invalid credentials | 401 |
| `UNAUTHORIZED` | Insufficient permissions | 403 |
| `RESOURCE_NOT_FOUND` | Resource not found | 404 |
| `AI_SERVICE_ERROR` | AI service unavailable | 503 |
| `DATABASE_ERROR` | Database connection error | 500 |
| `RATE_LIMIT_EXCEEDED` | Too many requests | 429 |

## 🔄 Webhook Endpoints (Future)

### POST /webhooks/ai-analysis-complete
Notification when AI analysis is complete.

### POST /webhooks/prescription-status-change
Notification when prescription status changes.

---
**Next Steps**: [Frontend Components Documentation](../frontend/components.md)