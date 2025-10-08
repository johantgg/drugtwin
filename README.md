# Drug Twin - Clinical Decision Support System

**Good Dosing • Safer Therapy • Integrated Clinical Decision Support**

## 📋 Deskripsi Umum

Drug Twin adalah aplikasi Clinical Decision Support System (CDSS) berbasis Artificial Intelligence (AI) yang mengintegrasikan perawat (admin), dokter, apoteker, dan pasien dalam satu ekosistem digital. Aplikasi ini dirancang untuk mengoptimalkan terapi obat, memperkuat koordinasi lintas profesi, dan memantau kepatuhan pasien secara digital.

## 🎯 Tujuan Aplikasi

- ✅ Menyediakan dukungan pengambilan keputusan klinis berbasis AI
- ✅ Meningkatkan kolaborasi antar tenaga kesehatan
- ✅ Memastikan akurasi dalam penyiapan dan pemberian obat
- ✅ Menyediakan sistem kepatuhan dan pemantauan pasien terintegrasi
- ✅ Mempermudah proses follow-up dengan QR Code login system

## 🛠️ Tech Stack

### Frontend
- **Framework**: React.js
- **CSS Framework**: Bootstrap 5
- **State Management**: Redux Toolkit
- **Routing**: React Router DOM
- **HTTP Client**: Axios
- **UI Components**: React Bootstrap

### Backend
- **Framework**: Laravel 10
- **Authentication**: Laravel Sanctum
- **API**: RESTful API
- **Queue**: Laravel Queue (Redis)
- **File Storage**: Laravel Storage

### Database
- **Primary Database**: MySQL 8.0
- **Cache**: Redis
- **Search Engine**: Elasticsearch (optional)

### AI Integration
- **AI Provider**: Google Gemini API
- **Machine Learning**: Custom model for drug recommendation
- **Real-time Communication**: WebSocket (Laravel Broadcasting)

### Infrastructure
- **Web Server**: Nginx
- **PHP**: PHP 8.2+
- **Node.js**: Node.js 18+
- **Package Manager**: Composer (PHP), npm (Node.js)

## 👥 User Roles & Permissions

### 🏥 Admin/Perawat
- Input data pasien baru
- Assign dokter untuk pasien
- Monitor status terapi
- Generate QR Code untuk pasien rawat jalan

### 👨‍⚕️ Dokter
- Review rekomendasi AI
- Edit/approve/reject terapi
- Komunikasi dengan apoteker
- Monitor patient progress

### 💊 Apoteker
- Validasi resep dari dokter
- Check ketersediaan obat
- Recommend substitusi obat
- Final approval untuk resep

### 🤒 Pasien
- View jadwal minum obat
- Track medication adherence
- Chat dengan healthcare providers
- Access educational content
- QR Code login untuk kunjungan

## 🏗️ Arsitektur Sistem

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   React.js      │    │   Laravel API   │    │     MySQL       │
│   Frontend      │◄──►│    Backend      │◄──►│    Database     │
│                 │    │                 │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Bootstrap     │    │  Google Gemini  │    │     Redis       │
│   CSS Framework │    │    AI API       │    │     Cache       │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## 📁 Struktur Project

```
drugtwin/
├── frontend/                 # React.js Application
│   ├── public/
│   ├── src/
│   │   ├── components/       # Reusable Components
│   │   ├── pages/           # Page Components
│   │   ├── hooks/           # Custom React Hooks
│   │   ├── services/        # API Services
│   │   ├── store/           # Redux Store
│   │   ├── utils/           # Utility Functions
│   │   └── styles/          # Custom CSS
│   ├── package.json
│   └── README.md
├── backend/                  # Laravel Application
│   ├── app/
│   │   ├── Http/Controllers/
│   │   ├── Models/
│   │   ├── Services/
│   │   └── Jobs/
│   ├── database/
│   │   ├── migrations/
│   │   └── seeders/
│   ├── routes/
│   └── composer.json
├── docs/                     # Documentation
│   ├── api/
│   ├── database/
│   ├── deployment/
│   └── user-guides/
├── docker/                   # Docker Configuration
├── .env.example
└── README.md
```

## 🚀 Development Phases

### Phase 1: Setup & Foundation
- [x] Project initialization
- [ ] Database schema design
- [ ] Laravel API setup
- [ ] React frontend setup
- [ ] Authentication system

### Phase 2: Core Features
- [ ] User management system
- [ ] Patient data management
- [ ] Drug recommendation AI integration
- [ ] Prescription workflow

### Phase 3: Advanced Features
- [ ] Real-time chat system
- [ ] QR Code generation & scanning
- [ ] Medication adherence tracking
- [ ] Educational content management

### Phase 4: Testing & Deployment
- [ ] Unit testing
- [ ] Integration testing
- [ ] Performance optimization
- [ ] Production deployment

## 📚 Documentation Index

- [Database Schema](./docs/database/schema.md)
- [API Endpoints](./docs/api/endpoints.md)
- [Frontend Components](./docs/frontend/components.md)
- [AI Integration](./docs/ai/integration.md)
- [Deployment Guide](./docs/deployment/guide.md)
- [User Guides](./docs/user-guides/)

## 🔧 Quick Start

### Prerequisites
- PHP 8.2+
- Node.js 18+
- MySQL 8.0+
- Composer
- npm/yarn

### Installation
```bash
# Clone repository
git clone <repository-url>
cd drugtwin

# Backend setup
cd backend
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate

# Frontend setup
cd ../frontend
npm install
npm start

# Start development servers
# Backend: php artisan serve
# Frontend: npm start
```

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Developed for Healthcare Innovation**
*Improving patient care through intelligent technology*