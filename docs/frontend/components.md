# Frontend Components Documentation

## 🎨 Frontend Architecture Overview

Drug Twin frontend menggunakan React.js dengan Bootstrap 5 untuk UI framework. Aplikasi dirancang dengan komponen yang reusable dan modular untuk memudahkan maintenance dan scalability.

## 🛠️ Tech Stack Detail

### Core Technologies
- **React.js 18+**: Main frontend framework
- **React Router DOM**: Client-side routing
- **Redux Toolkit**: State management
- **Bootstrap 5**: CSS framework
- **React Bootstrap**: React components for Bootstrap
- **Axios**: HTTP client for API calls
- **React Hook Form**: Form handling and validation
- **React Query**: Data fetching and caching

### Additional Libraries
- **React QR Scanner**: QR code scanning for patients
- **Chart.js / React-Chartjs-2**: Data visualization
- **React DatePicker**: Date selection components
- **React Select**: Enhanced select dropdowns
- **React Toastify**: Notification system
- **Socket.io-client**: Real-time communication

## 📁 Project Structure

```
frontend/
├── public/
│   ├── index.html
│   ├── favicon.ico
│   └── manifest.json
├── src/
│   ├── components/           # Reusable UI components
│   │   ├── common/          # Common components
│   │   ├── forms/           # Form components
│   │   ├── charts/          # Chart components
│   │   └── modals/          # Modal components
│   ├── pages/               # Page components
│   │   ├── admin/           # Admin dashboard pages
│   │   ├── doctor/          # Doctor dashboard pages
│   │   ├── pharmacist/      # Pharmacist dashboard pages
│   │   └── patient/         # Patient dashboard pages
│   ├── hooks/               # Custom React hooks
│   ├── services/            # API service functions
│   ├── store/               # Redux store and slices
│   ├── utils/               # Utility functions
│   ├── styles/              # Custom CSS files
│   └── constants/           # Application constants
├── package.json
└── README.md
```

## 🎯 Role-Based Dashboard Design

### 🏥 Admin/Perawat Dashboard
**Primary Functions**: Patient management, doctor assignment, system monitoring

#### Main Components:
1. **AdminDashboard** (`/pages/admin/Dashboard.jsx`)
2. **PatientManagement** (`/pages/admin/PatientManagement.jsx`)
3. **UserManagement** (`/pages/admin/UserManagement.jsx`)
4. **SystemMonitoring** (`/pages/admin/SystemMonitoring.jsx`)

#### Layout Structure:
```jsx
// AdminLayout.jsx
<Container fluid>
  <Row>
    <Col md={2}>
      <AdminSidebar />
    </Col>
    <Col md={10}>
      <AdminHeader />
      <AdminContent>
        {children}
      </AdminContent>
    </Col>
  </Row>
</Container>
```

### 👨‍⚕️ Doctor Dashboard
**Primary Functions**: Patient review, prescription management, AI recommendation review

#### Main Components:
1. **DoctorDashboard** (`/pages/doctor/Dashboard.jsx`)
2. **PatientList** (`/pages/doctor/PatientList.jsx`)
3. **PrescriptionReview** (`/pages/doctor/PrescriptionReview.jsx`)
4. **AIRecommendationPanel** (`/pages/doctor/AIRecommendationPanel.jsx`)

### 💊 Pharmacist Dashboard
**Primary Functions**: Prescription validation, drug availability check, substitution management

#### Main Components:
1. **PharmacistDashboard** (`/pages/pharmacist/Dashboard.jsx`)
2. **PrescriptionValidation** (`/pages/pharmacist/PrescriptionValidation.jsx`)
3. **DrugInventory** (`/pages/pharmacist/DrugInventory.jsx`)
4. **SubstitutionManagement** (`/pages/pharmacist/SubstitutionManagement.jsx`)

### 🤒 Patient Dashboard
**Primary Functions**: Medication tracking, appointment management, educational content

#### Main Components:
1. **PatientDashboard** (`/pages/patient/Dashboard.jsx`)
2. **MedicationSchedule** (`/pages/patient/MedicationSchedule.jsx`)
3. **AdherenceTracking** (`/pages/patient/AdherenceTracking.jsx`)
4. **EducationalContent** (`/pages/patient/EducationalContent.jsx`)

## 🧩 Detailed Component Specifications

### 1. Authentication Components

#### LoginForm (`/components/auth/LoginForm.jsx`)
```jsx
import React, { useState } from 'react';
import { Form, Button, Alert, Card } from 'react-bootstrap';
import { useAuth } from '../hooks/useAuth';

const LoginForm = () => {
  const [credentials, setCredentials] = useState({
    email: '',
    password: '',
    deviceName: 'Web Browser'
  });
  const { login, loading, error } = useAuth();

  const handleSubmit = async (e) => {
    e.preventDefault();
    await login(credentials);
  };

  return (
    <Card className="login-card">
      <Card.Header className="text-center">
        <h3>Drug Twin Login</h3>
      </Card.Header>
      <Card.Body>
        {error && <Alert variant="danger">{error}</Alert>}
        <Form onSubmit={handleSubmit}>
          <Form.Group className="mb-3">
            <Form.Label>Email</Form.Label>
            <Form.Control
              type="email"
              placeholder="Enter email"
              value={credentials.email}
              onChange={(e) => setCredentials({...credentials, email: e.target.value})}
              required
            />
          </Form.Group>
          
          <Form.Group className="mb-3">
            <Form.Label>Password</Form.Label>
            <Form.Control
              type="password"
              placeholder="Enter password"
              value={credentials.password}
              onChange={(e) => setCredentials({...credentials, password: e.target.value})}
              required
            />
          </Form.Group>
          
          <Button 
            variant="primary" 
            type="submit" 
            className="w-100"
            disabled={loading}
          >
            {loading ? 'Logging in...' : 'Login'}
          </Button>
        </Form>
      </Card.Body>
    </Card>
  );
};

export default LoginForm;
```

#### QRLogin (`/components/auth/QRLogin.jsx`)
```jsx
import React, { useState } from 'react';
import { Button, Modal } from 'react-bootstrap';
import QrScanner from 'react-qr-scanner';
import { useAuth } from '../hooks/useAuth';

const QRLogin = () => {
  const [showScanner, setShowScanner] = useState(false);
  const { qrLogin, loading } = useAuth();

  const handleScan = (data) => {
    if (data) {
      qrLogin(data);
      setShowScanner(false);
    }
  };

  const handleError = (err) => {
    console.error(err);
  };

  return (
    <>
      <Button 
        variant="outline-primary" 
        onClick={() => setShowScanner(true)}
        className="w-100 mt-3"
      >
        Scan QR Code to Login
      </Button>

      <Modal show={showScanner} onHide={() => setShowScanner(false)} centered>
        <Modal.Header closeButton>
          <Modal.Title>Scan QR Code</Modal.Title>
        </Modal.Header>
        <Modal.Body className="text-center">
          <QrScanner
            delay={300}
            onError={handleError}
            onScan={handleScan}
            style={{ width: '100%' }}
          />
          <p className="mt-3 text-muted">
            Point your camera at the QR code
          </p>
        </Modal.Body>
      </Modal>
    </>
  );
};

export default QRLogin;
```

### 2. Patient Management Components

#### PatientCard (`/components/patient/PatientCard.jsx`)
```jsx
import React from 'react';
import { Card, Badge, Button, Row, Col } from 'react-bootstrap';
import { useNavigate } from 'react-router-dom';

const PatientCard = ({ patient }) => {
  const navigate = useNavigate();

  const getStatusColor = (admissionType) => {
    return admissionType === 'inpatient' ? 'danger' : 'success';
  };

  const calculateAge = (birthDate) => {
    return new Date().getFullYear() - new Date(birthDate).getFullYear();
  };

  return (
    <Card className="patient-card mb-3">
      <Card.Body>
        <Row>
          <Col md={8}>
            <h5 className="card-title">
              {patient.user.name}
              <Badge 
                bg={getStatusColor(patient.admission_type)}
                className="ms-2"
              >
                {patient.admission_type}
              </Badge>
            </h5>
            <p className="text-muted mb-1">MRN: {patient.mrn}</p>
            <p className="text-muted mb-1">
              Age: {calculateAge(patient.birth_date)} years | 
              Gender: {patient.gender}
            </p>
            {patient.assigned_doctor && (
              <p className="text-muted mb-1">
                Doctor: {patient.assigned_doctor.name}
              </p>
            )}
            
            {/* Latest Lab Results */}
            {patient.latest_lab_results && patient.latest_lab_results.length > 0 && (
              <div className="mt-2">
                <small className="text-muted">Latest Results:</small>
                {patient.latest_lab_results.map((result, index) => (
                  <Badge 
                    key={index}
                    bg={result.status === 'normal' ? 'success' : 'warning'}
                    className="ms-1"
                  >
                    {result.test_name}: {result.test_value} {result.unit}
                  </Badge>
                ))}
              </div>
            )}
          </Col>
          
          <Col md={4} className="text-end">
            <Button 
              variant="primary" 
              size="sm"
              onClick={() => navigate(`/patients/${patient.id}`)}
            >
              View Details
            </Button>
            <Button 
              variant="outline-secondary" 
              size="sm"
              className="ms-2"
              onClick={() => navigate(`/patients/${patient.id}/prescriptions`)}
            >
              Prescriptions
            </Button>
          </Col>
        </Row>
      </Card.Body>
    </Card>
  );
};

export default PatientCard;
```

#### PatientForm (`/components/patient/PatientForm.jsx`)
```jsx
import React, { useState } from 'react';
import { Form, Row, Col, Button, Card } from 'react-bootstrap';
import { useForm } from 'react-hook-form';
import { usePatients } from '../hooks/usePatients';

const PatientForm = ({ patient = null, onSuccess }) => {
  const { createPatient, updatePatient, loading } = usePatients();
  const { register, handleSubmit, formState: { errors } } = useForm({
    defaultValues: patient || {}
  });

  const onSubmit = async (data) => {
    try {
      if (patient) {
        await updatePatient(patient.id, data);
      } else {
        await createPatient(data);
      }
      onSuccess?.();
    } catch (error) {
      console.error('Form submission error:', error);
    }
  };

  return (
    <Card>
      <Card.Header>
        <h5>{patient ? 'Edit Patient' : 'Add New Patient'}</h5>
      </Card.Header>
      <Card.Body>
        <Form onSubmit={handleSubmit(onSubmit)}>
          {/* User Information */}
          <Card className="mb-3">
            <Card.Header>User Information</Card.Header>
            <Card.Body>
              <Row>
                <Col md={6}>
                  <Form.Group className="mb-3">
                    <Form.Label>Full Name *</Form.Label>
                    <Form.Control
                      type="text"
                      {...register('user.name', { required: 'Name is required' })}
                      isInvalid={!!errors.user?.name}
                    />
                    <Form.Control.Feedback type="invalid">
                      {errors.user?.name?.message}
                    </Form.Control.Feedback>
                  </Form.Group>
                </Col>
                <Col md={6}>
                  <Form.Group className="mb-3">
                    <Form.Label>Email *</Form.Label>
                    <Form.Control
                      type="email"
                      {...register('user.email', { 
                        required: 'Email is required',
                        pattern: {
                          value: /^\S+@\S+$/i,
                          message: 'Invalid email address'
                        }
                      })}
                      isInvalid={!!errors.user?.email}
                    />
                    <Form.Control.Feedback type="invalid">
                      {errors.user?.email?.message}
                    </Form.Control.Feedback>
                  </Form.Group>
                </Col>
              </Row>
              
              <Row>
                <Col md={6}>
                  <Form.Group className="mb-3">
                    <Form.Label>Phone</Form.Label>
                    <Form.Control
                      type="tel"
                      {...register('user.phone')}
                    />
                  </Form.Group>
                </Col>
                {!patient && (
                  <Col md={6}>
                    <Form.Group className="mb-3">
                      <Form.Label>Password *</Form.Label>
                      <Form.Control
                        type="password"
                        {...register('user.password', { 
                          required: 'Password is required',
                          minLength: {
                            value: 8,
                            message: 'Password must be at least 8 characters'
                          }
                        })}
                        isInvalid={!!errors.user?.password}
                      />
                      <Form.Control.Feedback type="invalid">
                        {errors.user?.password?.message}
                      </Form.Control.Feedback>
                    </Form.Group>
                  </Col>
                )}
              </Row>
            </Card.Body>
          </Card>

          {/* Medical Information */}
          <Card className="mb-3">
            <Card.Header>Medical Information</Card.Header>
            <Card.Body>
              <Row>
                <Col md={4}>
                  <Form.Group className="mb-3">
                    <Form.Label>MRN *</Form.Label>
                    <Form.Control
                      type="text"
                      {...register('mrn', { required: 'MRN is required' })}
                      isInvalid={!!errors.mrn}
                    />
                    <Form.Control.Feedback type="invalid">
                      {errors.mrn?.message}
                    </Form.Control.Feedback>
                  </Form.Group>
                </Col>
                <Col md={4}>
                  <Form.Group className="mb-3">
                    <Form.Label>Birth Date *</Form.Label>
                    <Form.Control
                      type="date"
                      {...register('birth_date', { required: 'Birth date is required' })}
                      isInvalid={!!errors.birth_date}
                    />
                    <Form.Control.Feedback type="invalid">
                      {errors.birth_date?.message}
                    </Form.Control.Feedback>
                  </Form.Group>
                </Col>
                <Col md={4}>
                  <Form.Group className="mb-3">
                    <Form.Label>Gender *</Form.Label>
                    <Form.Select
                      {...register('gender', { required: 'Gender is required' })}
                      isInvalid={!!errors.gender}
                    >
                      <option value="">Select Gender</option>
                      <option value="male">Male</option>
                      <option value="female">Female</option>
                    </Form.Select>
                    <Form.Control.Feedback type="invalid">
                      {errors.gender?.message}
                    </Form.Control.Feedback>
                  </Form.Group>
                </Col>
              </Row>

              <Row>
                <Col md={3}>
                  <Form.Group className="mb-3">
                    <Form.Label>Weight (kg)</Form.Label>
                    <Form.Control
                      type="number"
                      step="0.1"
                      {...register('weight')}
                    />
                  </Form.Group>
                </Col>
                <Col md={3}>
                  <Form.Group className="mb-3">
                    <Form.Label>Height (cm)</Form.Label>
                    <Form.Control
                      type="number"
                      {...register('height')}
                    />
                  </Form.Group>
                </Col>
                <Col md={6}>
                  <Form.Group className="mb-3">
                    <Form.Label>Admission Type</Form.Label>
                    <Form.Select {...register('admission_type')}>
                      <option value="outpatient">Outpatient</option>
                      <option value="inpatient">Inpatient</option>
                    </Form.Select>
                  </Form.Group>
                </Col>
              </Row>

              <Form.Group className="mb-3">
                <Form.Label>Address</Form.Label>
                <Form.Control
                  as="textarea"
                  rows={2}
                  {...register('address')}
                />
              </Form.Group>

              <Row>
                <Col md={6}>
                  <Form.Group className="mb-3">
                    <Form.Label>Allergies (comma separated)</Form.Label>
                    <Form.Control
                      type="text"
                      placeholder="e.g., Penicillin, Aspirin"
                      {...register('allergies')}
                    />
                  </Form.Group>
                </Col>
                <Col md={6}>
                  <Form.Group className="mb-3">
                    <Form.Label>Medical History (comma separated)</Form.Label>
                    <Form.Control
                      type="text"
                      placeholder="e.g., Hypertension, Diabetes"
                      {...register('medical_history')}
                    />
                  </Form.Group>
                </Col>
              </Row>
            </Card.Body>
          </Card>

          {/* Form Actions */}
          <div className="d-flex justify-content-end">
            <Button 
              variant="secondary" 
              className="me-2"
              onClick={() => onSuccess?.()}
            >
              Cancel
            </Button>
            <Button 
              variant="primary" 
              type="submit"
              disabled={loading}
            >
              {loading ? 'Saving...' : (patient ? 'Update Patient' : 'Create Patient')}
            </Button>
          </div>
        </Form>
      </Card.Body>
    </Card>
  );
};

export default PatientForm;
```

### 3. Prescription Management Components

#### AIRecommendationPanel (`/components/prescription/AIRecommendationPanel.jsx`)
```jsx
import React, { useState } from 'react';
import { Card, Alert, Badge, Button, Row, Col, Modal } from 'react-bootstrap';
import { FaRobot, FaExclamationTriangle, FaCheckCircle } from 'react-icons/fa';

const AIRecommendationPanel = ({ 
  prescription, 
  onApprove, 
  onEdit, 
  onReject 
}) => {
  const [showDetails, setShowDetails] = useState(false);
  const aiRec = prescription.ai_recommendation;

  const getConfidenceColor = (score) => {
    if (score >= 0.9) return 'success';
    if (score >= 0.7) return 'warning';
    return 'danger';
  };

  const getSeverityIcon = (severity) => {
    switch (severity) {
      case 'high': return <FaExclamationTriangle className="text-danger" />;
      case 'medium': return <FaExclamationTriangle className="text-warning" />;
      default: return <FaCheckCircle className="text-success" />;
    }
  };

  return (
    <Card className="ai-recommendation-panel">
      <Card.Header className="d-flex align-items-center">
        <FaRobot className="me-2 text-primary" />
        <h6 className="mb-0">AI Recommendation</h6>
        <Badge 
          bg={getConfidenceColor(aiRec.confidence_score)}
          className="ms-auto"
        >
          Confidence: {(aiRec.confidence_score * 100).toFixed(1)}%
        </Badge>
      </Card.Header>

      <Card.Body>
        {/* Warnings */}
        {aiRec.warnings && aiRec.warnings.length > 0 && (
          <Alert variant="warning" className="mb-3">
            <h6>⚠️ Warnings:</h6>
            {aiRec.warnings.map((warning, index) => (
              <div key={index} className="d-flex align-items-center mb-1">
                {getSeverityIcon(warning.severity)}
                <span className="ms-2">{warning.message}</span>
              </div>
            ))}
          </Alert>
        )}

        {/* Recommendations */}
        {aiRec.recommendations && (
          <div className="mb-3">
            <h6>Recommended Medications:</h6>
            {aiRec.recommendations.map((rec, index) => (
              <Card key={index} className="mb-2 border-light">
                <Card.Body className="py-2">
                  <Row>
                    <Col md={6}>
                      <strong>{rec.medication_name}</strong>
                      <br />
                      <small className="text-muted">
                        {rec.recommended_dosage} - {rec.recommended_frequency}
                      </small>
                    </Col>
                    <Col md={6}>
                      <small className="text-muted">
                        Reasoning: {rec.reasoning}
                      </small>
                      {rec.adjustments && rec.adjustments.length > 0 && (
                        <div>
                          <Badge bg="info" className="me-1">Adjustments Made</Badge>
                        </div>
                      )}
                    </Col>
                  </Row>
                </Card.Body>
              </Card>
            ))}
          </div>
        )}

        {/* Drug Interactions */}
        {aiRec.drug_interactions && aiRec.drug_interactions.length > 0 && (
          <Alert variant="danger" className="mb-3">
            <h6>🚫 Drug Interactions:</h6>
            {aiRec.drug_interactions.map((interaction, index) => (
              <div key={index}>
                <strong>{interaction.drug_pair}</strong>: {interaction.description}
                <br />
                <small>Severity: {interaction.severity}</small>
              </div>
            ))}
          </Alert>
        )}

        {/* Action Buttons */}
        <div className="d-flex justify-content-between mt-3">
          <Button 
            variant="outline-secondary" 
            size="sm"
            onClick={() => setShowDetails(true)}
          >
            View Details
          </Button>
          
          <div>
            <Button 
              variant="success" 
              size="sm" 
              className="me-2"
              onClick={() => onApprove(prescription.id)}
            >
              Approve
            </Button>
            <Button 
              variant="warning" 
              size="sm" 
              className="me-2"
              onClick={() => onEdit(prescription.id)}
            >
              Edit
            </Button>
            <Button 
              variant="danger" 
              size="sm"
              onClick={() => onReject(prescription.id)}
            >
              Reject
            </Button>
          </div>
        </div>
      </Card.Body>

      {/* Details Modal */}
      <Modal 
        show={showDetails} 
        onHide={() => setShowDetails(false)}
        size="lg"
      >
        <Modal.Header closeButton>
          <Modal.Title>AI Analysis Details</Modal.Title>
        </Modal.Header>
        <Modal.Body>
          <pre>{JSON.stringify(aiRec, null, 2)}</pre>
        </Modal.Body>
      </Modal>
    </Card>
  );
};

export default AIRecommendationPanel;
```

### 4. Chat System Components

#### ChatWindow (`/components/chat/ChatWindow.jsx`)
```jsx
import React, { useState, useEffect, useRef } from 'react';
import { Card, Form, Button, ListGroup } from 'react-bootstrap';
import { useChat } from '../hooks/useChat';

const ChatWindow = ({ conversationId, currentUser }) => {
  const [message, setMessage] = useState('');
  const messagesEndRef = useRef(null);
  const { 
    messages, 
    sendMessage, 
    markAsRead, 
    loading 
  } = useChat(conversationId);

  const scrollToBottom = () => {
    messagesEndRef.current?.scrollIntoView({ behavior: "smooth" });
  };

  useEffect(() => {
    scrollToBottom();
  }, [messages]);

  const handleSendMessage = async (e) => {
    e.preventDefault();
    if (message.trim()) {
      await sendMessage({
        message: message.trim(),
        message_type: 'text'
      });
      setMessage('');
    }
  };

  const formatTime = (timestamp) => {
    return new Date(timestamp).toLocaleTimeString([], {
      hour: '2-digit',
      minute: '2-digit'
    });
  };

  return (
    <Card className="chat-window">
      <Card.Header>
        <h6 className="mb-0">Chat</h6>
      </Card.Header>
      
      <Card.Body className="p-0">
        <div className="chat-messages" style={{ height: '400px', overflowY: 'auto' }}>
          <ListGroup variant="flush">
            {messages.map((msg) => (
              <ListGroup.Item 
                key={msg.id}
                className={`message ${msg.sender?.id === currentUser.id ? 'own-message' : 'other-message'}`}
              >
                <div className="d-flex">
                  <div className="message-content flex-grow-1">
                    {msg.sender && (
                      <small className="text-muted">
                        {msg.sender.name} ({msg.sender.role})
                      </small>
                    )}
                    <div className="message-text">
                      {msg.message}
                    </div>
                    <small className="text-muted">
                      {formatTime(msg.created_at)}
                    </small>
                  </div>
                </div>
              </ListGroup.Item>
            ))}
          </ListGroup>
          <div ref={messagesEndRef} />
        </div>
      </Card.Body>
      
      <Card.Footer>
        <Form onSubmit={handleSendMessage}>
          <div className="d-flex">
            <Form.Control
              type="text"
              placeholder="Type a message..."
              value={message}
              onChange={(e) => setMessage(e.target.value)}
              disabled={loading}
            />
            <Button 
              type="submit" 
              variant="primary" 
              className="ms-2"
              disabled={loading || !message.trim()}
            >
              Send
            </Button>
          </div>
        </Form>
      </Card.Footer>
    </Card>
  );
};

export default ChatWindow;
```

### 5. Medication Adherence Components

#### MedicationSchedule (`/components/adherence/MedicationSchedule.jsx`)
```jsx
import React, { useState } from 'react';
import { Card, ListGroup, Button, Badge, Modal, Form } from 'react-bootstrap';
import { FaClock, FaCheck, FaTimes } from 'react-icons/fa';
import { useAdherence } from '../hooks/useAdherence';

const MedicationSchedule = ({ patientId, date = new Date() }) => {
  const [showConfirmModal, setShowConfirmModal] = useState(false);
  const [selectedDose, setSelectedDose] = useState(null);
  const [notes, setNotes] = useState('');
  
  const { 
    schedule, 
    markAsTaken, 
    markAsMissed, 
    loading 
  } = useAdherence(patientId, date);

  const handleMarkTaken = async () => {
    if (selectedDose) {
      await markAsTaken(selectedDose.id, {
        taken_time: new Date().toISOString(),
        notes: notes
      });
      setShowConfirmModal(false);
      setNotes('');
      setSelectedDose(null);
    }
  };

  const getStatusIcon = (status) => {
    switch (status) {
      case 'taken': return <FaCheck className="text-success" />;
      case 'missed': return <FaTimes className="text-danger" />;
      default: return <FaClock className="text-warning" />;
    }
  };

  const getStatusColor = (status, scheduledTime) => {
    if (status === 'taken') return 'success';
    if (status === 'missed') return 'danger';
    
    const now = new Date();
    const scheduled = new Date(scheduledTime);
    const diffMinutes = (now - scheduled) / (1000 * 60);
    
    if (diffMinutes > 60) return 'danger'; // Late
    if (diffMinutes > 30) return 'warning'; // Almost late
    return 'primary'; // On time
  };

  const formatTime = (time) => {
    return new Date(`2000-01-01T${time}`).toLocaleTimeString([], {
      hour: '2-digit',
      minute: '2-digit'
    });
  };

  return (
    <Card>
      <Card.Header>
        <h5>Today's Medication Schedule</h5>
        <small className="text-muted">
          {date.toLocaleDateString()}
        </small>
      </Card.Header>
      
      <Card.Body className="p-0">
        {schedule && schedule.today && schedule.today.length > 0 ? (
          <ListGroup variant="flush">
            {schedule.today.map((dose) => (
              <ListGroup.Item key={dose.id} className="d-flex align-items-center">
                <div className="me-3">
                  {getStatusIcon(dose.status)}
                </div>
                
                <div className="flex-grow-1">
                  <h6 className="mb-1">{dose.medication_name}</h6>
                  <p className="mb-1 text-muted">
                    {dose.dosage} - {formatTime(dose.scheduled_time)}
                  </p>
                  {dose.instructions && (
                    <small className="text-muted">{dose.instructions}</small>
                  )}
                </div>
                
                <div className="text-end">
                  <Badge 
                    bg={getStatusColor(dose.status, dose.scheduled_time)}
                    className="mb-2"
                  >
                    {dose.status}
                  </Badge>
                  
                  {dose.status === 'scheduled' && (
                    <div>
                      <Button
                        size="sm"
                        variant="success"
                        className="me-1"
                        onClick={() => {
                          setSelectedDose(dose);
                          setShowConfirmModal(true);
                        }}
                      >
                        Mark Taken
                      </Button>
                      <Button
                        size="sm"
                        variant="outline-danger"
                        onClick={() => markAsMissed(dose.id)}
                      >
                        Mark Missed
                      </Button>
                    </div>
                  )}
                </div>
              </ListGroup.Item>
            ))}
          </ListGroup>
        ) : (
          <div className="text-center p-4">
            <p className="text-muted">No medications scheduled for today</p>
          </div>
        )}
      </Card.Body>

      {/* Confirmation Modal */}
      <Modal 
        show={showConfirmModal} 
        onHide={() => setShowConfirmModal(false)}
      >
        <Modal.Header closeButton>
          <Modal.Title>Confirm Medication Taken</Modal.Title>
        </Modal.Header>
        <Modal.Body>
          {selectedDose && (
            <>
              <p>
                <strong>{selectedDose.medication_name}</strong><br />
                Dosage: {selectedDose.dosage}<br />
                Scheduled: {formatTime(selectedDose.scheduled_time)}
              </p>
              
              <Form.Group>
                <Form.Label>Notes (optional)</Form.Label>
                <Form.Control
                  as="textarea"
                  rows={2}
                  placeholder="Any notes about taking this medication..."
                  value={notes}
                  onChange={(e) => setNotes(e.target.value)}
                />
              </Form.Group>
            </>
          )}
        </Modal.Body>
        <Modal.Footer>
          <Button 
            variant="secondary" 
            onClick={() => setShowConfirmModal(false)}
          >
            Cancel
          </Button>
          <Button 
            variant="success" 
            onClick={handleMarkTaken}
            disabled={loading}
          >
            {loading ? 'Confirming...' : 'Confirm Taken'}
          </Button>
        </Modal.Footer>
      </Modal>
    </Card>
  );
};

export default MedicationSchedule;
```

## 🎨 Custom Styling Guide

### CSS Variables (`/src/styles/variables.css`)
```css
:root {
  /* Primary Colors */
  --primary-color: #2c5aa0;
  --secondary-color: #28a745;
  --accent-color: #ffc107;
  --danger-color: #dc3545;
  
  /* Background Colors */
  --bg-primary: #f8f9fa;
  --bg-secondary: #ffffff;
  --bg-accent: #e9ecef;
  
  /* Text Colors */
  --text-primary: #212529;
  --text-secondary: #6c757d;
  --text-muted: #adb5bd;
  
  /* Spacing */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 3rem;
  
  /* Border Radius */
  --border-radius-sm: 0.25rem;
  --border-radius-md: 0.375rem;
  --border-radius-lg: 0.5rem;
  
  /* Shadows */
  --shadow-sm: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.075);
  --shadow-md: 0 0.5rem 1rem rgba(0, 0, 0, 0.15);
  --shadow-lg: 0 1rem 3rem rgba(0, 0, 0, 0.175);
}
```

### Component Specific Styles (`/src/styles/components.css`)
```css
/* Patient Card Styles */
.patient-card {
  transition: transform 0.2s ease-in-out;
  border-left: 4px solid var(--primary-color);
}

.patient-card:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}

/* AI Recommendation Panel */
.ai-recommendation-panel {
  border: 2px solid var(--primary-color);
  background: linear-gradient(135deg, #f8f9ff 0%, #ffffff 100%);
}

.ai-recommendation-panel .card-header {
  background: var(--primary-color);
  color: white;
}

/* Chat Styles */
.chat-window .chat-messages {
  padding: 1rem;
  background-color: var(--bg-primary);
}

.message.own-message {
  margin-left: 20%;
}

.message.own-message .message-content {
  background-color: var(--primary-color);
  color: white;
  border-radius: 18px 18px 4px 18px;
  padding: 0.5rem 1rem;
}

.message.other-message .message-content {
  background-color: white;
  border: 1px solid #e1e5e9;
  border-radius: 18px 18px 18px 4px;
  padding: 0.5rem 1rem;
  margin-right: 20%;
}

/* Dashboard Cards */
.dashboard-card {
  border: none;
  box-shadow: var(--shadow-sm);
  transition: all 0.3s ease;
}

.dashboard-card:hover {
  box-shadow: var(--shadow-md);
  transform: translateY(-1px);
}

.stat-card {
  background: linear-gradient(135deg, var(--primary-color) 0%, #1e4a8c 100%);
  color: white;
  border-radius: var(--border-radius-lg);
}

/* Form Styles */
.form-card {
  border: none;
  box-shadow: var(--shadow-sm);
}

.form-card .card-header {
  background-color: var(--bg-accent);
  border-bottom: 2px solid var(--primary-color);
}

/* Loading States */
.loading-spinner {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 200px;
}

/* Responsive Design */
@media (max-width: 768px) {
  .patient-card .row {
    flex-direction: column;
  }
  
  .patient-card .text-end {
    text-align: left !important;
    margin-top: 1rem;
  }
  
  .message.own-message,
  .message.other-message .message-content {
    margin-left: 0;
    margin-right: 0;
  }
}
```

## 🔧 Custom Hooks

### useAuth Hook (`/src/hooks/useAuth.js`)
```javascript
import { useState, useContext, createContext } from 'react';
import { useNavigate } from 'react-router-dom';
import { authService } from '../services/authService';
import { toast } from 'react-toastify';

const AuthContext = createContext();

export const useAuth = () => {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
};

export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const navigate = useNavigate();

  const login = async (credentials) => {
    setLoading(true);
    setError(null);
    try {
      const response = await authService.login(credentials);
      setUser(response.data.user);
      localStorage.setItem('token', response.data.token);
      
      // Redirect based on role
      const dashboardRoutes = {
        admin: '/admin/dashboard',
        doctor: '/doctor/dashboard',
        pharmacist: '/pharmacist/dashboard',
        patient: '/patient/dashboard'
      };
      
      navigate(dashboardRoutes[response.data.user.role] || '/dashboard');
      toast.success('Login successful!');
    } catch (err) {
      setError(err.response?.data?.message || 'Login failed');
      toast.error('Login failed!');
    } finally {
      setLoading(false);
    }
  };

  const qrLogin = async (qrCode) => {
    setLoading(true);
    setError(null);
    try {
      const response = await authService.qrLogin({
        qr_code: qrCode,
        device_name: 'Mobile App'
      });
      setUser(response.data.user);
      localStorage.setItem('token', response.data.token);
      navigate('/patient/dashboard');
      toast.success('QR Login successful!');
    } catch (err) {
      setError(err.response?.data?.message || 'QR Login failed');
      toast.error('QR Login failed!');
    } finally {
      setLoading(false);
    }
  };

  const logout = async () => {
    try {
      await authService.logout();
    } catch (err) {
      console.error('Logout error:', err);
    } finally {
      setUser(null);
      localStorage.removeItem('token');
      navigate('/login');
      toast.success('Logged out successfully!');
    }
  };

  const value = {
    user,
    loading,
    error,
    login,
    qrLogin,
    logout
  };

  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
};
```

## 📱 Responsive Design Guidelines

### Breakpoints
- **Extra Small (xs)**: < 576px (Mobile phones)
- **Small (sm)**: ≥ 576px (Small tablets)
- **Medium (md)**: ≥ 768px (Tablets)
- **Large (lg)**: ≥ 992px (Desktops)
- **Extra Large (xl)**: ≥ 1200px (Large desktops)

### Mobile-First Design Principles
1. **Touch-Friendly Interfaces**: Minimum 44px touch targets
2. **Simplified Navigation**: Collapsible menus and bottom navigation
3. **Optimized Forms**: Single-column layouts on mobile
4. **Performance**: Lazy loading and image optimization
5. **Accessibility**: Screen reader support and keyboard navigation

## 🚀 Performance Optimization

### Code Splitting
```javascript
// Lazy loading for better performance
const AdminDashboard = lazy(() => import('./pages/admin/Dashboard'));
const DoctorDashboard = lazy(() => import('./pages/doctor/Dashboard'));
const PharmacistDashboard = lazy(() => import('./pages/pharmacist/Dashboard'));
const PatientDashboard = lazy(() => import('./pages/patient/Dashboard'));

// Route with Suspense
<Suspense fallback={<LoadingSpinner />}>
  <Routes>
    <Route path="/admin/dashboard" element={<AdminDashboard />} />
    {/* Other routes */}
  </Routes>
</Suspense>
```

### State Management Best Practices
```javascript
// Redux slice example
const prescriptionSlice = createSlice({
  name: 'prescriptions',
  initialState: {
    items: [],
    loading: false,
    error: null,
    currentPrescription: null
  },
  reducers: {
    setLoading: (state, action) => {
      state.loading = action.payload;
    },
    setPrescriptions: (state, action) => {
      state.items = action.payload;
      state.loading = false;
    },
    addPrescription: (state, action) => {
      state.items.push(action.payload);
    },
    updatePrescription: (state, action) => {
      const index = state.items.findIndex(p => p.id === action.payload.id);
      if (index !== -1) {
        state.items[index] = action.payload;
      }
    },
    setError: (state, action) => {
      state.error = action.payload;
      state.loading = false;
    }
  }
});
```

---
**Next Steps**: [AI Integration Documentation](../ai/integration.md)