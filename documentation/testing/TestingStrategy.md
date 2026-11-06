# Testing Strategy

## IELTS Centre Management and Learning Management System

Version: 1.0
Date: 2026-11-06
Status: Draft for Review

---

## 1. Testing Objectives

- Verify all functional requirements from FRS are met
- Ensure security and data protection compliance
- Validate performance under expected load
- Confirm accessibility (WCAG 2.1 AA)
- Verify mobile responsiveness across devices
- Ensure data integrity and transactional consistency
- Validate IELTS-specific scoring and evaluation accuracy

---

## 2. Test Types and Coverage

### 2.1 Unit Tests (Target: 80%+ coverage for business logic)

| Module | Key Areas | Framework |
|--------|-----------|-----------|
| Auth & RBAC | Password hashing, session management, permission evaluation, token validation | JUnit 5 + Mockito |
| Student/Admission | ID generation, status transitions, enrolment workflow, document validation | JUnit 5 |
| Course/Batch | Capacity checking, teacher assignment, transfer logic, schedule conflicts | JUnit 5 |
| Timetable | Recurring pattern generation, conflict detection, rescheduling rules | JUnit 5 |
| Attendance | Percentage calculation, leave workflow, correction audit trail | JUnit 5 |
| Fees/Payments | Invoice generation, payment allocation, balance computation, discount application, refund logic | JUnit 5 |
| IELTS Scoring | Band calculation (IELTS rounding rules), criteria weighting, overall band from 4 skills | JUnit 5 |
| Question Bank | Question categorization, test assembly, randomization, scoring | JUnit 5 |
| Writing/Speaking | Criteria storage, annotation serialization, version comparison | JUnit 5 |
| Notifications | Template rendering, channel routing, scheduling | JUnit 5 |

### 2.2 Integration Tests (Target: Critical paths 100%)

| Flow | Scenarios |
|------|-----------|
| Student Admission | Enquiry -> Assessment -> Course Select -> Batch Allocate -> Invoice -> Payment -> Active |
| Batch Transfer | Student moved between batches, attendance/grades preserved, fees adjusted |
| Attendance Marking | Bulk entry, leave approval, correction, percentage recalculation, reports |
| Fee Payment | Partial payment, multiple installments, discount, receipt generation, outstanding update |
| Mock Test Taking | Timer, navigation, auto-save, submission, L/R auto-score, W/S evaluation queue |
| Writing Evaluation | Annotation save, rubric scoring, feedback, resubmission, version history |
| Speaking Evaluation | Audio playback, rubric scoring, audio feedback, band calculation |
| Band Progression | Multiple test attempts, snapshot creation, progress charts data, weakness analysis |

### 2.3 End-to-End Tests (Playwright / Cypress)

| User Journey | Key Assertions |
|--------------|----------------|
| Admin: Create Course -> Batch -> Assign Teacher -> Schedule -> Student Enrol | All entities linked, timetable visible, attendance markable |
| Teacher: Upload Material -> Create Assignment -> Student Submits -> Evaluate -> Band Update | Material accessible, submission received, evaluation updates progress |
| Student: Practice Listening -> Submit -> View Score -> Practice Writing -> Submit -> Teacher Evaluates -> View Band | Audio plays, answers scored, editor works, evaluation visible |
| Student: Take Mock Test -> Complete L/R -> Submit W/S -> View Results | Timer works, navigation persists, results accurate |
| Receptionist: New Enquiry -> Convert -> Register -> Payment -> Receipt | Full flow completes, data consistent |
| Mobile: All above flows on 375px viewport | No horizontal scroll, touch targets >= 44px, readable text |

### 2.4 Performance Tests (k6 / JMeter)

| Scenario | Target | Threshold |
|----------|--------|-----------|
| Login (100 concurrent) | < 500ms p95 | < 1s |
| Dashboard load (50 concurrent) | < 2s p95 | < 3s |
| Attendance marking (batch of 30) | < 1s | < 2s |
| Mock test submission (L/R auto-score) | < 3s | < 5s |
| Report generation (1000 records) | < 5s | < 10s |
| File upload (50MB video) | < 30s | < 60s |
| Search students (10k records) | < 500ms | < 1s |

### 2.5 Security Tests

| Area | Tools / Approach |
|------|------------------|
| Authentication | Brute force, session fixation, password policy, MFA (future) |
| Authorization | IDOR testing, role escalation, resource access matrix |
| Input Validation | SQLi, XSS, command injection, file upload validation |
| API Security | Rate limiting, CORS, CSRF, security headers |
| Data Protection | Encryption at rest, PII masking in logs, backup encryption |
| Dependencies | OWASP Dependency Check, Snyk, npm audit / mvn dependency-check |

### 2.6 Accessibility Tests (axe-core + Manual)

| Checklist | Method |
|-----------|--------|
| Keyboard navigation | Tab through all pages, verify focus order, skip links |
| Screen reader | NVDA/JAWS testing on key flows (login, test taking, evaluation) |
| Colour contrast | axe-core automated + manual verification |
| ARIA labels | All icon buttons, form fields, live regions |
| Heading hierarchy | No skipped levels, logical structure |
| Focus management | Modals, drawers, dynamic content updates |
| Reduced motion | Verify animations disabled |

### 2.7 Mobile Responsiveness Tests

| Device | Viewport | Key Checks |
|--------|----------|------------|
| iPhone SE | 375x667 | Bottom nav, touch targets, no horizontal scroll |
| iPhone 14 Pro | 393x852 | Safe areas, full-screen test interface |
| iPad | 768x1024 | Sidebar collapse, split views |
| Android (Chrome) | 360x800 | Audio recording, file upload |
| Desktop Chrome | 1920x1080 | Full layout, hover states |
| Desktop Firefox | 1920x1080 | Cross-browser compatibility |

---

## 3. Test Data Strategy

### 3.1 Test Data Categories

| Category | Source | Refresh |
|----------|--------|---------|
| Reference Data | Courses, Skills, Question Types, Statuses | Static (seeded) |
| Synthetic Users | Admin, Teachers, Students, Receptionists | Per test run |
| Synthetic Academic Data | Batches, Timetables, Materials, Questions | Per test module |
| Synthetic Financial Data | Fee Structures, Invoices, Payments | Per test module |
| Production-like Data | Anonymized export from staging | Weekly |

### 3.2 Data Generation

- Use Testcontainers for PostgreSQL in integration tests
- Factory pattern for entity creation (Java: Instancio / Kotlin: Kluent)
- Frontend: MSW (Mock Service Worker) for API mocking in component tests
- Seed scripts for consistent baseline (Flyway repeatable migrations)

---

## 4. Test Environments

| Environment | Purpose | Data | Access |
|-------------|---------|------|--------|
| Local (Dev) | Unit, Component, Integration | Synthetic | Developer |
| CI Pipeline | Unit, Integration, Security, Lint | Synthetic | Automated |
| Staging | E2E, Performance, Accessibility, UAT | Production-like (anonymized) | QA, Stakeholders |
| Production | Monitoring, Canary, Smoke | Real | Restricted |

---

## 5. CI/CD Pipeline Gates

`
┌─────────────┐
│   Commit    │
└──────┬──────┘
       │
       ▼
┌─────────────┐     ┌─────────────┐
│  Lint/Type  │────▶│  Unit Tests │
│   Check     │     │  (coverage) │
└─────────────┘     └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │ Integration │
                    │   Tests     │
                    └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │  Security   │
                    │   Scan      │
                    └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │   Build     │
                    │  Docker     │
                    └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │  Deploy to  │
                    │  Staging    │
                    └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │  E2E Tests  │
                    │ Performance │
                    │Accessibility│
                    └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │  Manual     │
                    │  Approval   │
                    └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │  Deploy to  │
                    │ Production  │
                    └─────────────┘
`

### 5.1 Required Gates (Blocking)

- [ ] TypeScript compilation (zero errors)
- [ ] ESLint/Prettier (zero errors)
- [ ] Unit tests pass (coverage >= 80% changed lines)
- [ ] Integration tests pass
- [ ] Security scan (zero critical/high)
- [ ] Dependency audit (zero critical/high)
- [ ] Docker build succeeds

### 5.2 Staging Gates (Blocking)

- [ ] E2E critical paths pass
- [ ] Performance benchmarks met
- [ ] Accessibility audit (zero critical/serious)
- [ ] Mobile responsiveness verified
- [ ] Stakeholder sign-off

---

## 6. Test Case Management

- **Tool:** GitHub Issues / Project Board (or TestRail if preferred)
- **Traceability:** Each test case linked to FRS requirement ID (FR-XX.YY)
- **Organization:** By module, then by user role, then by flow
- **Execution:** Automated tests run on CI; Manual tests tracked in test runs

---

## 7. Defect Management

| Severity | Definition | SLA |
|----------|------------|-----|
| Critical | Data loss, security breach, complete workflow block | Fix in 24h |
| High | Major feature broken, incorrect calculations, access control bypass | Fix in 3 days |
| Medium | Minor feature issue, UI bug, performance degradation | Fix in 1 week |
| Low | Cosmetic, enhancement, documentation | Next sprint |

---

## 8. IELTS-Specific Validation

### 8.1 Band Score Accuracy

| Test | Expected | Tolerance |
|------|----------|-----------|
| Average of 4 skills rounded to nearest 0.5 | 6.125 -> 6.0, 6.375 -> 6.5 | Exact match |
| Individual skill: raw score -> band (official conversion) | Per IELTS tables | Exact match |
| Writing: 4 criteria -> overall | Mean of 4, rounded to 0.5 | Exact match |
| Speaking: 4 criteria -> overall | Mean of 4, rounded to 0.5 | Exact match |

### 8.2 Test Interface Requirements

- Timer accuracy: +/- 1 second over 60 minutes
- Answer persistence: Survives refresh, navigation, network blip
- Auto-save: Every 30 seconds + on navigation
- Submission: Confirmation modal, no accidental submit

### 8.3 Audio/Video Handling

- Recording: Works on Chrome/Firefox/Safari (MediaRecorder API)
- Playback: Seeking, speed control, volume
- File size: Max 100MB per recording, chunked upload
- Format: WebM (Opus) for recording, MP3/MP4 for uploads

---

## 9. Regression Strategy

- **Automated:** All unit/integration/E2E tests run on every PR
- **Critical Paths:** Smoke test suite (15 min) on every deploy
- **Full Regression:** Weekly on staging (2 hours)
- **Performance Regression:** Compare p95 against baseline (alert if > 10% degradation)

---

## 10. Test Documentation

- **Test Plan:** This document
- **Test Cases:** GitHub Issues with labels (test-case, module, priority)
- **Test Runs:** GitHub Actions workflow runs + Manual test run records
- **Test Report:** Allure Report / Playwright HTML Report generated per run
- **Metrics Dashboard:** Grafana (test pass rate, coverage, defect trends)

---

## 11. Risks and Mitigations

| Risk | Impact | Mitigation |
|------|--------|------------|
| Audio recording fails on iOS Safari | High (Speaking core) | Test early, fallback to file upload, document limitations |
| Band rounding edge cases | Medium | Comprehensive unit tests with official IELTS test vectors |
| Concurrent attendance marking | Medium | Optimistic locking, test with 30 simultaneous users |
| Payment gateway callback replay | High | Idempotency keys, test duplicate callbacks |
| Large file upload timeout | Medium | Chunked upload, presigned URLs, test 100MB+ |
| Mobile test interface usability | High | Early usability testing with real students |
| Data migration complexity | Medium | Automated migration scripts, dry-run validation |

---

End of Testing Strategy
