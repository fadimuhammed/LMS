# Phased Implementation Roadmap

## IELTS Centre Management & Learning Management System

**Version:** 1.0  
**Date:** 2026-11-06

---

## 1. Overview

This roadmap outlines the phased implementation of the IELTS Centre Management & Learning Management System. The project follows a structured, incremental approach to ensure a functional MVP before expanding scope.

---

## 2. Project Timeline (14–18 Weeks)

| Phase | Duration | Key Deliverables |
|-------|----------|------------------|
| **Phase 1: Discovery & Planning** | Weeks 1–2 | - Finalised requirements
|                       |              | - Database ER diagram and schema design
|                       |              | - Module dependency map
|                       |              | - UI/UX screen-by-screen plan
|                       |              | - Phased implementation roadmap
|                       |              | - Testing strategy
|                       |              | - Assumptions, risks, and unresolved questions
|                       |              | - Security & compliance review

| **Phase 2: Foundation & Authentication** | Weeks 3–4 | - Backend and frontend setup
|                       |              | - Database configuration
|                       |              | - Login/logout functionality
|                       |              | - Role-based access control
|                       |              | - Dashboard layouts
|                       |              | - Basic CRUD for users

| **Phase 3: Centre Management** | Weeks 5–7 | - Student registration and profiles
|                       |              | - Teacher management
|                       |              | - Course and batch management
|                       |              | - Timetable and scheduling
|                       |              | - Attendance management
|                       |              | - Fees and payment management

| **Phase 4: Academic Development** | Weeks 8–11 | - Learning materials upload and management
|                       |              | - Assignments and homework
|                       |              | - IELTS Listening and Reading practice tests
|                       |              | - Writing assignment submission and evaluation
|                       |              | - Speaking recording and evaluation

| **Phase 5: Assessment & Analytics** | Weeks 12–13 | - Question bank and mock tests
|                       |              | - Band score and progress tracking
|                       |              | - Teacher feedback and evaluation workflows
|                       |              | - Basic reports and analytics

| **Phase 6: Quality & Deployment** | Weeks 14–15 | - Functional testing
|                       |              | - Security testing
|                       |              | - Mobile responsiveness testing
|                       |              | - Backup and restore testing
|                       |              | - Production deployment

| **Phase 7: Pilot & Improvements** | Weeks 16–18 | - Integration with real centre data
|                       |              | - Teacher and administrator training
|                       |              | - Pilot with a small batch of students
|                       |              | - Usability and performance optimizations
|                       |              | - Rollout to the entire centre

---

## 3. Key Milestones

### 3.1 MVP Launch (Week 14)

**Must-have features:**
- Secure login with RBAC
- Admin dashboard with centre statistics
- Student registration and profiles
- Teacher management
- Course and batch creation
- Timetable and attendance
- Fees and payment management
- Basic IELTS Listening and Reading practice tests
- Writing assignment submission and evaluation
- Speaking recording and evaluation
- Band score tracking
- Basic reports
- Responsive mobile interface
- Database backup and restore

### 3.2 Post-Launch (Weeks 15–18)

**Enhancements:**
- Question bank and mock tests
- Advanced analytics and progress tracking
- Enhanced teacher feedback workflows
- Mobile app integration (future)
- WhatsApp/SMS notifications
- Gamification elements

---

## 4. Technical Implementation Strategy

### 4.1 Modular Development

The system will be developed in vertical slices, focusing on one complete feature path at a time. This approach ensures:
- Early delivery of working features
- Clear separation of concerns
- Easier testing and debugging

### 4.2 Database Schema

The database will be designed around the following entities:
- Users (authentication)
- Students (academic profiles)
- Teachers (academic profiles)
- Courses and Course Levels
- Batches and Batch Students
- Class Sessions and Timetables
- Attendance Records
- Fees and Payments
- Learning Materials
- Assignments and Submissions
- IELTS Questions, Tests, and Attempts
- Band Scores and Progress Tracking

### 4.3 Security & Compliance

- All user input validated at system boundaries
- Parameterized queries to prevent SQL injection
- Secure session management with CSRF protection
- Role-based access control enforced on all endpoints
- Data encryption for sensitive fields
- Audit logging for all administrative actions
- Compliance with DPDP Act for data protection

---

## 5. Testing Strategy

### 5.1 Test Types

| Type | Purpose |
|------|---------|
| Unit Tests | Individual components and functions |
| Integration Tests | Module interactions |
| End-to-End Tests | Full workflows |
| Performance Tests | Page load times, API response times |
| Security Tests | Authentication, authorization, input validation |
| Accessibility Tests | WCAG 2.1 AA compliance |
| Usability Tests | User experience and navigation |

### 5.2 Test Coverage

- 80%+ code coverage for critical business logic
- 100% coverage for user-facing interactions
- Manual testing for edge cases and error handling
- Automated testing for regression and performance

### 5.3 Test Environment

- Development: Local Dockerized environment
- Staging: Production-like environment with test data
- Production: Deployed application with monitoring

---

## 6. Risks & Mitigations

| Risk Area | Potential Risk | Mitigation Strategy |
|------------|---------------|----------------------|
| Scope Creep | Uncontrolled feature expansion | Strict phase-based approach
| Technical Debt | Complexity in modular design | Refactor incrementally
| Data Migration | Legacy system integration | Dedicated import scripts
| Security Vulnerabilities | Unpatched dependencies | Regular dependency audits
| User Adoption | Low engagement with new system | Training sessions and support

---

## 7. Assumptions & Open Questions

### 7.1 Assumptions

- Centre will use INR for all financial transactions
- All users will authenticate via email/phone + password
- IELTS band scores will follow standard rounding rules
- Teacher evaluations will be manual for Writing/Speaking
- Students will primarily use the web interface

### 7.2 Open Questions

1. **Student Volume:** Expected concurrent students?
2. **Teacher Count:** How many active trainers?
3. **Class Mode:** Classroom-only, hybrid, or online-only?
4. **Fee Model:** Monthly, course/package, or custom installments?
5. **Evaluation Model:** Teacher evaluates all, or auto Listening/Reading + teacher Writing/Speaking?
6. **Account Creation:** Admin-created, self-registration with approval, or both?
7. **Existing Data:** Migration from spreadsheets/legacy system?
8. **Branding:** Centre logo, colours, certificate template preferences?
9. **Payment Gateway:** Preferred provider (Razorpay, Cashfree, PayU, etc.)?
10. **Email Provider:** Transactional email service (SendGrid, Mailgun, SES)?

---

## 8. Next Steps

1. Finalise stakeholder input on open questions
2. Confirm database schema design
3. Finalise UI/UX design for critical screens
4. Begin Phase 2: Foundation & Authentication

*End of Roadmap - Awaiting Stakeholder Review*
