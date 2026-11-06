# User Role & Permission Matrix

## IELTS Centre Management & Learning Management System

**Version:** 1.0  
**Date:** 2026-11-06

---

## 1. Roles Overview

| Role | Description | Authentication |
|------|-------------|----------------|
| Administrator | Full centre control | Email/Phone + Password |
| Teacher/Trainer | Academic delivery | Email/Phone + Password |
| Student | Learning & practice | Email/Phone + Password |
| Receptionist/Accounts | Administrative support | Email/Phone + Password |

---

## 2. Permission Matrix

Legend: R=Read, W=Write, C=Create, D=Delete, A=Admin config

### 2.1 Dashboard & Analytics

| Resource | Admin | Teacher | Student | Receptionist |
|----------|-------|---------|---------|--------------|
| Centre-wide statistics | R,W,A | R (own batches only) | R (own) | R (admissions/fees only) |
| Student performance overview | R,W | R (assigned batches) | R (own) | R |
| Teacher activity report | R,W,A | R (own) | - | - |
| Financial reports | R,W,A | - | - | R |
| Real-time alerts | R,W,A | R | R | R |

### 2.2 Student Management

| Resource | Admin | Teacher | Student | Receptionist |
|----------|-------|---------|---------|--------------|
| Student list | R,W,C,D | R (assigned batches) | R (own) | R,W,C |
| Student profile | R,W,C,D | R,W (academic fields) | R,W (limited) | R,W,C |
| Student documents | R,W,C,D | R | R (own) | R,W,C |
| Admission/enquiry management | R,W,C,D | - | - | R,W,C |
| Batch allocation | R,W,C,D | R,W | - | R,W |
| Student status changes | R,W,A | - | - | R,W |

### 2.3 Teacher Management

| Resource | Admin | Teacher | Student | Receptionist |
|----------|-------|---------|---------|--------------|
| Teacher list | R,W,C,D | R (own) | - | - |
| Teacher profile | R,W,C,D | R,W (own) | - | - |
| Teacher assignments | R,W,C,D | R | - | - |
| Teacher attendance | R,W | R,W (own) | - | - |

### 2.4 Course & Batch Management

| Resource | Admin | Teacher | Student | Receptionist |
|----------|-------|---------|---------|--------------|
| Course creation/edit | R,W,C,D,A | R | - | - |
| Batch creation/edit | R,W,C,D,A | R | R | R |
| Student enrollment | R,W,C,D | R,W | - | R,W |
| Batch transfers | R,W | - | - | - |

### 2.5 Timetable & Attendance

| Resource | Admin | Teacher | Student | Receptionist |
|----------|-------|---------|---------|--------------|
| Class schedule | R,W,C,D,A | R,W (assigned) | R | R |
| Attendance marking | R,W,C,D | R,W,C (own batches) | R (own) | - |
| Attendance reports | R,W,A | R (assigned) | R (own) | R |
| Leave requests | R,W,A | R,W | C,R,W (own) | - |

### 2.6 Fees & Payments

| Resource | Admin | Teacher | Student | Receptionist |
|----------|-------|---------|---------|--------------|
| Fee structures | R,W,C,D,A | - | - | R |
| Invoices | R,W,C,D | - | R (own) | R,W,C |
| Payment recording | R,W,C,D | - | - | R,W,C |
| Receipts | R,W,C,D | - | R (own) | R,W,C |
| Outstanding fees | R,W,A | - | R (own) | R,W |

### 2.7 IELTS Learning & Materials

| Resource | Admin | Teacher | Student | Receptionist |
|----------|-------|---------|---------|--------------|
| Learning materials | R,W,C,D,A | R,W,C | R (assigned) | - |
| Assignment creation | R,W,C,D,A | R,W,C | - | - |
| Assignment submission | - | R,W | R,W,C | - |
| Assignment evaluation | R,W,A | R,W,C,D | R | - |
| Writing evaluation | R,W,A | R,W,C | R (own) | - |
| Speaking evaluation | R,W,A | R,W,C | R (own) | - |

### 2.8 Question Bank & Mock Tests

| Resource | Admin | Teacher | Student | Receptionist |
|----------|-------|---------|---------|--------------|
| Question bank | R,W,C,D,A | R,W,C | R (own) | - |
| Test creation | R,W,C,D,A | R,W,C | - | - |
| Test taking | - | - | R,W,C | - |
| Test results | R,W,A | R,W (assigned) | R (own) | - |

### 2.9 Band Scores & Progress

| Resource | Admin | Teacher | Student | Receptionist |
|----------|-------|---------|---------|--------------|
| Band scores | R,W,A | R,W (assigned) | R (own) | - |
| Progress charts | R,W,A | R (assigned) | R (own) | - |
| Weakness identification | R,W,A | R,W | R (own) | - |

### 2.10 Communication

| Resource | Admin | Teacher | Student | Receptionist |
|----------|-------|---------|---------|--------------|
| Announcements | R,W,C,D,A | R,W,C | R | R,W,C |
| Notifications | R,W,A | R,W | R | R,W |
| Direct messaging | R,W,A | R,W | R,W | R,W |

### 2.11 Reports & Certificates

| Resource | Admin | Teacher | Student | Receptionist |
|----------|-------|---------|---------|--------------|
| All reports | R,W,A | R (assigned) | R (own) | R (admissions/fees) |
| Certificate generation | R,W,C,D,A | R | R (own) | - |
| Certificate verification | R | - | R | - |

### 2.12 System Settings

| Resource | Admin | Teacher | Student | Receptionist |
|----------|-------|---------|---------|--------------|
| Centre config | R,W,A | - | - | - |
| Academic settings | R,W,A | - | - | - |
| User permissions | R,W,A | - | - | - |

---

## 3. Data Access Rules

### 3.1 Student Data Privacy
- Students can ONLY access their own academic records
- Teachers can access students only in their assigned batches
- Admin can access all students
- Receptionist can access admissions/fees only

### 3.2 Financial Data
- Financial transactions accessible only to Admin, Receptionist (restricted)
- Students see only their own invoice/payment history
- Teachers have no financial access

### 3.3 IELTS Assessment Data
- Writing/Speaking evaluation visible to assigned teacher and Admin
- Students see only their own scores and feedback
- Teachers see only their assigned students

### 3.4 Audit Logging
- All permission-escalating actions logged
- Failed access attempts logged
- Data exports logged

---

## 4. API Enforcement

- ALL permission checks enforced on BACKEND (never client-side only)
- Role validation on every protected endpoint
- Resource ownership checks (batch membership, student ownership)
- Field-level permissions (e.g., teachers cannot edit student contact info)
- Rate limiting on sensitive endpoints

---

*End of Permission Matrix*
