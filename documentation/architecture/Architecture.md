# System Architecture Document

## IELTS Centre Management & Learning Management System

**Version:** 1.0  
**Date:** 2026-11-06  
**Status:** Draft for Review

---

## 1. Architectural Style

**Modular Monolith** — A single deployable application with clearly separated internal modules aligned to business domains. Each module owns its data and exposes well-defined interfaces.

### Why Not Microservices?
- Single centre deployment — no scaling need for independent services
- Operational simplicity: one deploy, one database, one monitoring stack
- Transactional consistency across domains (e.g., enrolment → invoice → batch allocation)
- Team size: small; microservices add coordination overhead
- Future extraction possible: modules designed with clean boundaries

---

## 2. High-Level Architecture

`
┌─────────────────────────────────────────────────────────────────┐
│                        PRESENTATION LAYER                        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │  Admin App  │  │ Teacher App │  │ Student App │  (React SPA) │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘             │
└─────────┼────────────────┼────────────────┼────────────────────┘
          │                │                │
          │      HTTPS / REST API (JSON)   │
          ▼                ▼                ▼
┌─────────────────────────────────────────────────────────────────┐
│                      BACKEND APPLICATION                         │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                    API GATEWAY / CONTROLLERS             │    │
│  │  Auth │ Students │ Courses │ Batches │ Timetable ...    │    │
│  └──────────────────────────┬──────────────────────────────┘    │
│                             │                                    │
│  ┌──────────────────────────┴──────────────────────────────┐    │
│  │                   DOMAIN MODULES                          │    │
│  │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌───────────────┐  │    │
│  │  │ Auth &  │ │ Student │ │ Course  │ │ IELTS Learn   │  │    │
│  │  │  RBAC   │ │  & Adm  │ │ & Batch │ │  & Assess     │  │    │
│  │  └─────────┘ └─────────┘ └─────────┘ └───────────────┘  │    │
│  │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌───────────────┐  │    │
│  │  │Teacher  │ │  Fee &  │ │  QBank  │ │  Notify &     │  │    │
│  │  │ Mgmt    │ │ Payments│ │ & Tests │ │  Reports      │  │    │
│  │  └─────────┘ └─────────┘ └─────────┘ └───────────────┘  │    │
│  └──────────────────────────┬──────────────────────────────┘    │
│                             │                                    │
│  ┌──────────────────────────┴──────────────────────────────┐    │
│  │                  INFRASTRUCTURE                           │    │
│  │  JPA/Hibernate │ Spring Security │ Validation │ Audit    │    │
│  └──────────────────────────────────────────────────────────┘    │
└─────────────────────────────┬───────────────────────────────────┘
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
       ┌────────────┐ ┌────────────┐ ┌────────────┐
       │ PostgreSQL │ │  File      │ │ External   │
       │  (Primary) │ │  Storage   │ │ Services   │
       └────────────┘ └────────────┘ └────────────┘
                              (S3/MinIO)   (Email, SMS, PGW)
`

---

## 3. Technology Stack

| Layer | Technology | Version | Rationale |
|-------|------------|---------|-----------|
| Frontend | React + TypeScript | 18+ / 5.x | Type safety, ecosystem, SPA |
| Styling | Tailwind CSS | 3.x | Utility-first, responsive, no CSS bloat |
| State | React Query (TanStack Query) | 5.x | Server state, caching, mutations |
| Routing | React Router | 6.x | Standard SPA routing |
| Forms | React Hook Form + Zod | 7.x / 3.x | Validation, performance |
| UI Components | Headless UI / Radix UI | latest | Accessible primitives |
| Charts | Recharts | 2.x | React-native, declarative |
| Backend | Java + Spring Boot | 21 / 3.2+ | Strong typing, domain modelling, ecosystem |
| Security | Spring Security | 6.x | RBAC, session mgmt, CSRF |
| ORM | Spring Data JPA / Hibernate | 3.x | Type-safe queries, migrations |
| API Docs | Springdoc OpenAPI | 2.x | Auto-generated Swagger |
| Database | PostgreSQL | 15+ | ACID, JSONB, indexing, reliability |
| Migrations | Flyway | 10.x | Versioned, repeatable, CI-friendly |
| File Storage | MinIO (dev) / S3 (prod) | latest | S3-compatible, private buckets |
| PDF | Apache PDFBox | 3.x | Open source, robust |
| Build | Maven / Gradle | latest | Standard Java build |
| Container | Docker + Docker Compose | latest | Reproducible deployments |
| CI/CD | GitHub Actions | - | Integrated with repo |

---

## 4. Module Boundaries & Domain Model

### 4.1 Auth & RBAC Module
**Responsibility:** Authentication, authorization, sessions, password management, audit logging
**Entities:** User, Role, Permission, UserRole, Session, AuditLog
**Exports:** AuthService, PermissionEvaluator, CurrentUserProvider

### 4.2 Student & Admission Module
**Responsibility:** Student lifecycle, enquiries, documents, status transitions
**Entities:** Student, Guardian, StudentDocument, Enquiry, Enrollment, PlacementTest
**Exports:** StudentService, EnrollmentService, AdmissionWorkflow

### 4.3 Course & Batch Module
**Responsibility:** Course catalogue, batch management, capacity, teacher assignment
**Entities:** Course, CourseLevel, Batch, BatchStudent, BatchTeacher, Classroom
**Exports:** CourseService, BatchService, CapacityService

### 4.4 Timetable Module
**Responsibility:** Scheduling, recurring classes, rescheduling, teacher/classroom availability
**Entities:** ClassSession, TimetableEntry, TeacherAvailability, ClassroomBooking
**Exports:** TimetableService, SchedulingService

### 4.5 Attendance Module
**Responsibility:** Attendance recording, leave, reports, corrections
**Entities:** AttendanceRecord, LeaveRequest, AttendanceCorrection
**Exports:** AttendanceService, LeaveService

### 4.6 Fee & Payment Module
**Responsibility:** Fee structures, invoices, payments, receipts, outstanding, discounts
**Entities:** FeeStructure, Invoice, Payment, Receipt, Discount, Refund
**Exports:** FeeService, PaymentService, InvoiceService

### 4.7 Teacher Module
**Responsibility:** Teacher profiles, specialisations, workload, feedback
**Entities:** Teacher, TeacherSpecialisation, TeacherWorkload, TeacherFeedback
**Exports:** TeacherService, WorkloadService

### 4.8 IELTS Learning Module
**Responsibility:** Learning materials, assignments, 4 skills practice
**Entities:** LearningMaterial, MaterialAssignment, Assignment, Submission, SubmissionFeedback
**Exports:** MaterialService, AssignmentService, SkillPracticeService

### 4.9 Assessment Module (Question Bank & Tests)
**Responsibility:** Question bank, test creation, test taking, evaluation
**Entities:** Question, QuestionOption, Passage, AudioClip, Test, TestSection, TestAttempt, StudentAnswer, SkillScore
**Exports:** QuestionBankService, TestService, EvaluationService

### 4.10 Band Score & Progress Module
**Responsibility:** Band calculation, progress tracking, reports
**Entities:** BandScore, ProgressSnapshot, ProgressReport, WeaknessAnalysis
**Exports:** BandCalculationService, ProgressService

### 4.11 Notification Module
**Responsibility:** In-app notifications, announcements, messaging, provider abstraction
**Entities:** Notification, Announcement, Message, NotificationTemplate, NotificationProvider
**Exports:** NotificationService, MessagingService

### 4.12 Report & Certificate Module
**Responsibility:** Report generation, certificate templates, PDF, verification
**Entities:** ReportDefinition, Certificate, CertificateTemplate
**Exports:** ReportService, CertificateService

---

## 5. Database Design Principles

- **Separate auth from domain:** users table distinct from students/	eachers
- **Enrolments as join with history:** Student↔Batch via enrollments with dates/status
- **Immutable test attempts:** Each test attempt = new record; never update scores in place
- **Criteria-level storage:** Writing/Speaking store 4 criterion scores + overall
- **Payment transactions:** Separate from fee structures; balance computed from transactions
- **Soft deletes:** deleted_at timestamp on academic/financial entities
- **Audit fields:** created_at, updated_at, created_by, updated_by on all tables
- **Indexes:** Foreign keys, frequently queried columns (student_id, batch_id, date ranges)

---

## 6. Security Architecture

### 6.1 Authentication
- Session-based (not JWT) — simpler revocation, CSRF protection built-in
- bcrypt with cost ≥ 12
- Secure cookies: HttpOnly; Secure; SameSite=Lax; Path=/
- Rate limiting on /login, /password-reset

### 6.2 Authorization
- @PreAuthorize on all controller methods
- Custom PermissionEvaluator for resource-level checks (batch membership, ownership)
- Method-level security in services

### 6.3 Data Protection
- Field-level encryption for sensitive PII (Aadhaar, passport) — optional
- File storage: private buckets, signed URLs with expiry
- No PII in logs (sanitised log format)
- DPDP compliance: consent records, retention policies, deletion endpoints

### 6.4 Transport & Headers
- HTTPS enforced (HSTS)
- CSP: default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data:; font-src 'self'; connect-src 'self'
- Security headers via Spring Security defaults

---

## 7. API Design

### 7.1 Conventions
- RESTful: plural nouns, HTTP verbs, standard status codes
- Versioned: /api/v1/
- Pagination: page, size, sort params; envelope response
- Filtering: query params (?status=active&batchId=123)
- Errors: RFC 7807 Problem Details ({type, title, status, detail, instance})

### 7.2 Key Endpoints (Sample)

`
POST   /api/v1/auth/login
POST   /api/v1/auth/logout
GET    /api/v1/users/me

GET    /api/v1/students
POST   /api/v1/students
GET    /api/v1/students/{id}
PATCH  /api/v1/students/{id}
GET    /api/v1/students/{id}/timeline

GET    /api/v1/courses
POST   /api/v1/courses
GET    /api/v1/batches
POST   /api/v1/batches
POST   /api/v1/batches/{id}/students
POST   /api/v1/batches/{id}/teachers

GET    /api/v1/timetable?batchId=1&week=2026-01-01
POST   /api/v1/class-sessions
PATCH  /api/v1/class-sessions/{id}/reschedule

POST   /api/v1/attendance/batch/{batchId}/date/{date}
GET    /api/v1/attendance/student/{studentId}

GET    /api/v1/fee-structures
POST   /api/v1/invoices
POST   /api/v1/payments
GET    /api/v1/students/{id}/outstanding

GET    /api/v1/materials?batchId=1&skill=LISTENING
POST   /api/v1/assignments
POST   /api/v1/submissions
PATCH  /api/v1/submissions/{id}/evaluate

GET    /api/v1/questions?skill=READING&difficulty=MEDIUM
POST   /api/v1/tests
POST   /api/v1/test-attempts
GET    /api/v1/test-attempts/{id}/results

GET    /api/v1/students/{id}/band-scores
GET    /api/v1/students/{id}/progress

GET    /api/v1/notifications
POST   /api/v1/announcements

GET    /api/v1/reports/attendance?batchId=1&from=2026-01-01&to=2026-01-31
GET    /api/v1/certificates/student/{studentId}
`

---

## 8. Frontend Architecture

### 8.1 Application Structure
`
src/
├── apps/
│   ├── admin/          # Admin routes, components
│   ├── teacher/        # Teacher routes, components
│   └── student/        # Student routes, components
├── shared/
│   ├── components/     # Reusable UI (Button, Table, Modal, Form, Chart)
│   ├── hooks/          # useAuth, usePermissions, useQuery, useMutation
│   ├── services/       # API client (Axios + React Query)
│   ├── types/          # TypeScript interfaces (generated from OpenAPI)
│   ├── utils/          # Formatters, validators, constants
│   └── styles/         # Tailwind config, global CSS
└── main.tsx            # Entry, providers, routing
`

### 8.2 Routing Strategy
- Role-based route guards (redirect to appropriate dashboard)
- Lazy-loaded route chunks per role
- Deep linking support for all screens

### 8.3 State Management
- Server state: React Query (caching, invalidation, optimistic updates)
- Client state: React Context (auth, theme, locale) + useState/useReducer
- URL state: searchParams for filters, pagination, sorting

### 8.4 Responsive Design
- Mobile-first Tailwind breakpoints: sm: 640px, md: 768px, lg: 1024px, xl: 1280px
- Collapsible sidebar (desktop) → bottom nav (mobile < 768px)
- Touch-friendly targets (≥ 44px)
- Test interface: full-screen on mobile, no horizontal scroll

---

## 9. Deployment Architecture

`
┌─────────────────────────────────────────────────────────┐
│                    INTERNET                              │
└─────────────────────────┬───────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│                  REVERSE PROXY (Nginx)                   │
│  SSL Termination │ Rate Limiting │ Static Assets        │
└─────────────────────────┬───────────────────────────────┘
                          │
          ┌───────────────┼───────────────┐
          ▼               ▼               ▼
   ┌──────────┐    ┌──────────┐    ┌──────────┐
   │ Backend  │    │ Backend  │    │ Backend  │  (Docker containers)
   │  App #1  │    │  App #2  │    │  App #3  │  (Horizontal scaling)
   └────┬─────┘    └────┬─────┘    └────┬─────┘
        │               │               │
        └───────────────┼───────────────┘
                        │
                        ▼
              ┌──────────────────┐
              │   PostgreSQL     │  (Primary + Replica)
              │   (Managed/VM)   │
              └────────┬─────────┘
                       │
                       ▼
              ┌──────────────────┐
              │   MinIO / S3     │  (Private buckets)
              └──────────────────┘
`

### 9.1 Environments
- **Development:** Docker Compose (all services local)
- **Staging:** Single VPS, Docker Compose, staging DB
- **Production:** VPS/Cloud, Docker Swarm or K8s (future), managed PostgreSQL, S3

### 9.2 Backup Strategy
- PostgreSQL: Daily pg_dump to S3 (30-day retention)
- File storage: Versioned S3 bucket (cross-region replication)
- Configuration: GitOps (all infra as code)

---

## 10. Observability

- **Logging:** Structured JSON logs (Logback); correlation IDs via MDC
- **Metrics:** Micrometer + Prometheus (JVM, HTTP, business metrics)
- **Tracing:** OpenTelemetry (Spring Boot 3 auto-instrumentation)
- **Alerting:** Prometheus Alertmanager → Email/Slack
- **Health:** Spring Boot Actuator (/actuator/health, /actuator/info)

---

## 11. Risks & Mitigations

| Risk | Impact | Mitigation |
|------|--------|------------|
| Audio recording playback issues on mobile | High (Speaking core feature) | Test early; use MediaRecorder API; fallback to file upload |
| IELTS band rounding complexity | Medium | Implement as pure function with comprehensive unit tests |
| Concurrent attendance marking | Medium | Optimistic locking; batch-level lock for bulk ops |
| Payment gateway callback security | High | Server-side verification; idempotency keys |
| Large file uploads (video/audio) | Medium | Chunked upload; presigned URLs; size limits |
| Data migration from spreadsheets | Medium | Dedicated import scripts; validation; dry-run mode |
| Teacher evaluation bottleneck | Medium | Queue-based assignment; workload balancing UI |

---

*End of Architecture Document*
