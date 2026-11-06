# Product Requirements Document

## IELTS Centre Management & Learning Management System

**Version:** 1.0  
**Date:** 2026-11-06  
**Status:** Draft for Review  
**Project:** IELTS-LMS  
**Location:** Kozhikode, Kerala, India

---

## 1. Executive Summary

A unified web application combining **Centre Management System** (administrative operations) and **IELTS Learning Management System** (academic delivery) for a single IELTS training centre in Kozhikode, Kerala. The system serves four user roles: Administrator, Teacher/Trainer, Student, and Receptionist/Accounts Staff.

**Primary Objective:** Replace manual processes with a reliable, daily-use software product that handles admissions, batches, timetables, attendance, fees, IELTS practice (Listening, Reading, Writing, Speaking), mock examinations, band-score tracking, and reporting.

---

## 2. Business Context

| Attribute | Detail |
|-----------|--------|
| Business Type | IELTS Coaching & Language Training |
| Deployment | Single centre, production web application |
| Primary Language | English (UI) |
| Currency | INR (Indian Rupees) |
| Timezone | Asia/Kolkata (IST) |
| Phone Format | +91 (Indian) |
| Target Launch | 14–18 weeks from start |

---

## 3. Target Users & Roles

| Role | Primary Responsibilities |
|------|-------------------------|
| **Administrator** | Full centre control: students, teachers, courses, batches, timetable, attendance, fees, materials, exams, performance, reports, settings |
| **Teacher / IELTS Trainer** | Assigned batches, schedules, attendance, materials, assignments, practice tests, Writing/Speaking evaluation, feedback, progress monitoring |
| **Student** | Dashboard, course/batch info, timetable, attendance, materials, assignments, IELTS practice (4 skills), mock exams, Writing submission, Speaking recording, results, progress tracking, notifications, payments |
| **Receptionist / Accounts** | Enquiries, admissions, fee payments, receipts, student/financial records (restricted) |

---

## 4. Core Functional Modules

### 4.1 Centre Management Modules

| Module | Key Capabilities |
|--------|------------------|
| **Dashboard** | Role-specific overviews: stats, admissions, batches, classes, attendance, fees, exams, performance |
| **Student & Admission Management** | Registration, unique ID, personal/guardian/educational details, IELTS target/date/type, proficiency assessment, enrolment, batch allocation, documents, status, history |
| **Course & Batch Management** | 6 course types (Academic, General, Foundation, Intensive, Crash, Spoken English), batch creation, teacher/student allocation, capacity, dates, timetable, classroom, transfers |
| **Timetable & Scheduling** | Weekly calendar, recurring classes, teacher/classroom availability, rescheduling, cancellations, replacements, reminders |
| **Attendance Management** | Batch/date recording, 4 statuses (Present/Absent/Late/Excused), teacher attendance, history, percentage, leave, reports, notifications, corrections with audit |
| **Teacher Management** | Profiles, IELTS skill specialisation, batch allocation, timetable, attendance, teaching hours, evaluation workload, feedback |
| **Fees & Payments** | Course/admission fees, installments, partial/full payments, cash/UPI/bank transfer, invoices, receipts, outstanding tracking, discounts, refunds, reminders, reports |
| **Notifications & Communication** | System, class/assignment/exam/fee/attendance reminders, announcements, batch notices, teacher-student messaging, extensible for email/SMS/WhatsApp |
| **Reports & Analytics** | Admissions, enrolment, batch occupancy, attendance, fee collection, outstanding, teacher activity, assignments, mock performance, skill progression, course completion |
| **Certificates** | Course completion, customizable templates, certificate numbers, PDF generation, QR verification |

### 4.2 IELTS Learning Management Modules

| Module | Key Capabilities |
|--------|------------------|
| **Learning Materials** | PDF, audio, video, images, Word, worksheets, passages, samples, cue cards, vocab, grammar; organised by course/skill/topic/lesson/batch; access-controlled |
| **Assignments & Homework** | Create, assign (batch/individual), deadlines, text/doc/image/audio submissions, status tracking, feedback/marks, resubmission, history |
| **IELTS Listening** | Audio player, section-wise exercises, 7 question types, timed tests, auto-evaluation, transcripts, explanations, performance history |
| **IELTS Reading** | Passages with questions, 8 question types, timer, navigation, auto-scoring, review, time-per-passage, topic-wise accuracy |
| **IELTS Writing** | Task 1 & 2 (Academic/General), online editor, word count, deadlines, doc submissions, teacher annotations, 4-criteria evaluation (Task Achievement/Response, Coherence/Cohesion, Lexical Resource, Grammatical Range/Accuracy), resubmission, individual scores |
| **IELTS Speaking** | Parts 1/2/3, cue cards, audio recording/playback, assignments, mock interviews, 4-criteria evaluation (Fluency/Coherence, Lexical Resource, Grammatical Range/Accuracy, Pronunciation), individual scores |
| **Question Bank & Mock Exams** | Reusable questions (categorised by skill/type/difficulty), audio/image attachments, passages, answer keys/explanations, test creation (skill-specific/full-length), scheduling, timed interface, auto-scoring + teacher evaluation, history/retakes |
| **Band Score & Progress Tracking** | 4 skill scores + overall band, IELTS rounding rules, historical results, progress charts, target band, skill comparison, weakness identification, teacher feedback, downloadable reports |

---

## 5. Non-Functional Requirements

| Requirement | Specification |
|-------------|---------------|
| **Responsiveness** | Mobile (320px+), tablet, desktop — mobile-first |
| **Performance** | Normal pages = 2–3s load; test interface optimised for concentration |
| **Security** | HTTPS, bcrypt (=12 rounds), RBAC enforced backend, parameterised queries, CSP, secure cookies, audit logs |
| **Database** | PostgreSQL 15+ with migrations (Flyway/Liquibase) |
| **Availability** | 99.5% monthly target |
| **Backup** | Daily automated database backups; documented restore |
| **Scalability** | Modular monolith; future multi-centre expansion possible |
| **Accessibility** | WCAG 2.1 AA: keyboard nav, contrast, ARIA, focus management |
| **Auditability** | Important admin actions logged |
| **Data Integrity** | Transactions for payments/critical records |
| **File Storage** | Private storage with controlled access (S3-compatible or local) |
| **Privacy** | DPDP Act compliance; consent for recordings; minimised PII collection |

---

## 6. Technical Architecture (Proposed)

**Pattern:** Modular Monolith  
**Frontend:** React 18+ + TypeScript + Tailwind CSS  
**Backend:** Java 21 + Spring Boot 3.x + Spring Security + Spring Data JPA  
**Database:** PostgreSQL 15+  
**Auth:** Session-based (httpOnly, secure, sameSite=lax) + CSRF protection  
**API:** REST + OpenAPI/Swagger documentation  
**File Storage:** S3-compatible (MinIO for dev, AWS S3/DigitalOcean Spaces for prod)  
**Charts:** Recharts  
**PDF Generation:** Apache PDFBox / iText  
**Deployment:** Docker + Docker Compose (Linux VPS)  
**Version Control:** Git + GitHub  

---

## 7. Key Design Decisions

| Decision | Rationale |
|----------|-----------|
| Modular monolith over microservices | Single centre; avoids operational complexity; clear domain boundaries |
| Separate users from students/	eachers | Auth identity ? academic profile; allows role changes without data loss |
| Enrolments link students to courses/batches | Batch changes preserve history |
| Store every mock test attempt separately | Never overwrite; full audit trail |
| Store Writing/Speaking criteria individually | IELTS requires 4-criterion breakdown; not just overall band |
| Payment transactions separate from fee structures | Accurate balance calculation; audit trail |
| Soft deletion/archival for academic/financial records | Compliance; recovery; retention policies |
| Teacher-evaluated Writing/Speaking; auto for Listening/Reading | Matches IELTS assessment reality; AI postponed to v2 |
| In-app notifications first; email/SMS/WhatsApp later | MVP scope; extensible design |

---

## 8. Success Criteria (MVP Launch)

| # | Criterion |
|---|-----------|
| 1 | Secure login + RBAC enforced on all endpoints |
| 2 | Admin dashboard with live centre statistics |
| 3 | Student registration, profiles, unique IDs |
| 4 | Teacher management with skill specialisation |
| 5 | Course & batch creation with capacity/timetable |
| 6 | Timetable with recurring classes & rescheduling |
| 7 | Attendance recording (4 statuses) + reports |
| 8 | Fee structures, invoices, payments, receipts, outstanding reports |
| 9 | Study material upload + batch-restricted access |
| 10 | IELTS Listening practice tests with audio + auto-scoring |
| 11 | IELTS Reading practice tests with timer + auto-scoring |
| 12 | Writing assignment submission + 4-criteria teacher evaluation |
| 13 | Speaking recording + 4-criteria teacher evaluation |
| 14 | Mock test results + band tracking with IELTS rounding |
| 15 | Basic admin/teacher/student reports (exportable) |
| 16 | Responsive mobile interface for all roles |
| 17 | Database backup/restore verified |
| 18 | Zero critical/high security findings |

---

## 9. Out of Scope (Version 2+)

- AI-powered essay evaluation
- AI speaking evaluation
- AI conversational tutor
- WhatsApp automation
- Native mobile applications
- Gamification/leaderboards
- Advanced financial accounting
- Multi-branch/centre support
- Parent portal

---

## 10. Open Questions for Stakeholder Clarification

1. **Student Volume:** Expected concurrent students? (Impacts infra sizing)
2. **Teacher Count:** How many active trainers?
3. **Class Mode:** Classroom-only, hybrid, or online-only? (Affects timetable/classroom design)
4. **Fee Model:** Monthly, course/package, both, or custom installments?
5. **Evaluation Model:** Teacher evaluates all, or auto Listening/Reading + teacher Writing/Speaking?
6. **Account Creation:** Admin-created, self-registration with approval, or both?
7. **Existing Data:** Migration from spreadsheets/legacy system?
8. **Branding:** Centre logo, colours, certificate template preferences?
9. **Payment Gateway:** Preferred provider (Razorpay, Cashfree, PayU, etc.)?
10. **Email Provider:** Transactional email service (SendGrid, Mailgun, SES)?
11. **Backup Retention:** How long to retain backups?
12. **Data Retention:** Student record retention policy post-course-completion?

---

*End of PRD — Awaiting Stakeholder Review*
