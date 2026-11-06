# Assumptions, Risks & Unresolved Questions

## IELTS Centre Management and Learning Management System

Version: 1.0
Date: 2026-11-06
Status: Draft for Review

---

## 1. Assumptions

### 1.1 Business Assumptions

| ID | Assumption | Impact if Wrong |
|----|------------|-----------------|
| BA-01 | Centre operates in INR only; no multi-currency needed | Payment module would need redesign |
| BA-02 | Single physical centre in Kozhikode; no multi-branch initially | Architecture supports future expansion but not in MVP |
| BA-03 | Primary interface language is English; no Malayalam/Hindi UI in MVP | Localization would require i18n framework |
| BA-04 | Classes are primarily classroom-based; online/hybrid optional | Timetable/classroom model may need virtual classroom support |
| BA-05 | Fee model is course/package-based with installments; not purely monthly | Monthly billing would need recurring invoice automation |
| BA-06 | Teacher evaluates all Writing/Speaking; Listening/Reading auto-scored | AI evaluation would need separate integration |
| BA-07 | Student accounts created by admin (not self-registration) | Self-registration would need approval workflow |
| BA-08 | Centre has 2-5 teachers and 50-150 concurrent students | Scaling assumptions affect infrastructure sizing |
| BA-09 | Existing data in spreadsheets/legacy system needs migration | Migration effort not in MVP scope |
| BA-10 | Centre provides own IELTS practice content (no copyrighted material) | Content acquisition not in scope |

### 1.2 Technical Assumptions

| ID | Assumption | Impact if Wrong |
|----|------------|-----------------|
| TA-01 | PostgreSQL 15+ available (managed or self-hosted) | Database choice affects migration syntax |
| TA-02 | Java 21 + Spring Boot 3.2+ for backend | Version compatibility for libraries |
| TA-03 | React 18 + TypeScript + Tailwind for frontend | Component library selection |
| TA-04 | Docker + Docker Compose for deployment | Production may need K8s later |
| TA-05 | S3-compatible storage (MinIO dev, AWS/DO Spaces prod) | File storage abstraction |
| TA-06 | Linux VPS or cloud VM for hosting | OS-specific configs |
| TA-07 | SMTP/Transactional email provider available | Notification delivery |
| TA-08 | Payment gateway (Razorpay/Cashfree) with server callbacks | Integration effort |
| TA-09 | Domain name and SSL certificate available | HTTPS setup |
| TA-10 | Team has Java/Spring and React/TypeScript skills | Onboarding/training needs |

### 1.3 Regulatory Assumptions

| ID | Assumption | Impact if Wrong |
|----|------------|-----------------|
| RA-01 | DPDP Act compliance required (consent, retention, deletion) | Legal review needed |
| RA-02 | GST-compliant invoices required (GSTIN, HSN/SAC) | Invoice template complexity |
| RA-10 | Student recording consent needed for Speaking audio | Consent UI and audit trail |
| RA-11 | No special education sector regulations beyond DPDP | Additional compliance |

---

## 2. Risks

### 2.1 Technical Risks

| ID | Risk | Likelihood | Impact | Mitigation |
|----|------|------------|--------|------------|
| TR-01 | Audio recording/playback issues on mobile Safari | High | High | Early spike (Week 2), fallback to file upload, document known limitations |
| TR-02 | IELTS band rounding complexity (official rules) | Medium | High | Pure function with exhaustive unit tests using official test vectors |
| TR-03 | Concurrent attendance marking race conditions | Medium | Medium | Optimistic locking, batch-level mutex for bulk operations |
| TR-04 | Payment gateway callback security (replay, tampering) | Low | Critical | Idempotency keys, signature verification, server-only validation |
| TR-05 | Large file uploads (video/audio) timeout or fail | Medium | Medium | Chunked upload, presigned URLs, progress tracking, size limits |
| TR-06 | Data migration from spreadsheets incomplete/incorrect | Medium | High | Automated scripts with validation, dry-run mode, manual verification |
| TR-07 | Teacher evaluation bottleneck (many submissions) | Medium | Medium | Queue-based assignment, workload dashboard, auto-assign |
| TR-08 | Real-time timer sync in mock tests (network latency) | Medium | Medium | Server-authoritative timer, client interpolation, offline queue |
| TR-09 | Spring Boot / React version upgrade breaking changes | Low | Medium | Pin versions, test upgrades in staging, dependency monitoring |
| TR-10 | PostgreSQL performance with complex queries (reports) | Low | Medium | Indexing strategy, materialized views, query optimization |

### 2.2 Business Risks

| ID | Risk | Likelihood | Impact | Mitigation |
|----|------|------------|--------|------------|
| BR-01 | Scope creep during development | High | High | Strict phase gates, change request process, MVP definition fixed |
| BR-02 | Stakeholder unavailable for clarifications | Medium | High | Weekly sync meetings, documented decisions, default assumptions |
| BR-03 | Centre operational workflow changes mid-project | Medium | High | Modular design, configuration over code, regular demos |
| BR-04 | Teacher resistance to digital evaluation | Medium | Medium | Training sessions, UX focus on efficiency, keyboard shortcuts |
| BR-05 | Student low adoption of practice platform | Medium | Medium | Gamification (v2), teacher-driven assignments, progress visibility |
| BR-06 | Payment gateway integration delays | Medium | High | Start integration early, sandbox testing, fallback manual recording |
| BR-07 | Content copyright issues (IELTS materials) | Low | High | Clear policy: only original/licensed content, no commercial books |
| BR-08 | Data privacy complaint (DPDP) | Low | Critical | Privacy by design, consent records, deletion endpoint, audit |
| BR-09 | Key personnel turnover | Low | High | Documentation, knowledge sharing, cross-training |
| BR-10 | Budget overrun | Medium | Medium | Fixed scope MVP, time-boxed phases, regular budget review |

### 2.3 Schedule Risks

| ID | Risk | Likelihood | Impact | Mitigation |
|----|------|------------|--------|------------|
| SR-01 | Phase 3 (Centre Management) takes longer than 3 weeks | Medium | High | Vertical slicing, parallel frontend/backend, daily standups |
| SR-02 | IELTS Learning module (Phase 4) underestimated | High | High | Spike on test interface (Week 2), reusable components |
| SR-03 | Integration testing reveals cross-module defects | Medium | Medium | Contract tests, shared types, early integration |
| SR-04 | Mobile responsiveness fixes cascade | Medium | Medium | Mobile-first from start, component-level responsive testing |
| SR-05 | Security audit findings delay production | Low | High | Security built-in, pre-audit self-assessment |

---

## 3. Unresolved Questions (Requiring Stakeholder Input)

### 3.1 Critical (Blocking Decisions)

| ID | Question | Options | Recommendation | Decision Needed By |
|----|----------|---------|----------------|---------------------|
| UQ-01 | Expected concurrent students? | <50 / 50-150 / 150-300 / 300+ | 50-150 assumed | Week 1 |
| UQ-02 | Number of active teachers? | 1 / 2-5 / 6-10 / 10+ | 2-5 assumed | Week 1 |
| UQ-03 | Class delivery mode? | Classroom only / Hybrid / Online only | Hybrid assumed (classroom + optional online) | Week 1 |
| UQ-04 | Fee charging model? | Monthly / Course-package / Both / Custom installments | Course-package with installments | Week 1 |
| UQ-05 | Evaluation model? | Teacher all / Auto L+R + Teacher W+S / AI-assisted | Auto L+R + Teacher W+S | Week 1 |
| UQ-06 | Student account creation? | Admin only / Self-reg + approval / Both | Admin creates | Week 1 |
| UQ-07 | Existing data migration needed? | Yes (spreadsheets) / No / Partial | Yes - plan for migration sprint | Week 2 |
| UQ-08 | Branding assets available? | Logo, colours, certificate template | Need assets by Week 3 | Week 2 |

### 3.2 Important (Affects Design)

| ID | Question | Options | Recommendation | Decision Needed By |
|----|----------|---------|----------------|---------------------|
| UQ-09 | Preferred payment gateway? | Razorpay / Cashfree / PayU / Other / Manual only | Razorpay (popular in Kerala) | Week 3 |
| UQ-10 | Transactional email provider? | SendGrid / Mailgun / SES / SMTP | SendGrid (good deliverability) | Week 3 |
| UQ-11 | Backup retention period? | 30 days / 90 days / 1 year / Custom | 90 days | Week 4 |
| UQ-12 | Student data retention after course completion? | 1 year / 3 years / 7 years / Indefinite | 3 years (educational records) | Week 4 |
| UQ-13 | WhatsApp integration for notifications? | MVP / Phase 2 / Not needed | Phase 2 | Week 4 |
| UQ-14 | Certificate template design? | Standard / Custom / Multiple templates | Custom (provide design) | Week 5 |
| UQ-15 | Attendance threshold for alerts? | 75% / 80% / 85% / Configurable | 75% (configurable) | Week 5 |
| UQ-16 | Grading scale customization? | Standard IELTS / Custom bands / Both | Standard IELTS | Week 5 |

### 3.3 Nice to Have (Future Phases)

| ID | Question | Options | Notes |
|----|----------|---------|-------|
| UQ-17 | Parent/Guardian portal access? | Yes / No | Phase 2 |
| UQ-18 | AI essay evaluation (experimental)? | Yes / No | Phase 2+ |
| UQ-19 | Mobile app (React Native/Flutter)? | Yes / No | Phase 2+ |
| UQ-20 | Gamification (streaks, badges, leaderboards)? | Yes / No | Phase 2+ |
| UQ-21 | Multi-branch expansion timeline? | 6 months / 1 year / 2+ years / Never | Architecture ready |

---

## 4. Decisions Log

| Date | Decision ID | Decision | Rationale | Author |
|------|-------------|----------|-----------|--------|
| 2026-11-06 | DEC-001 | Modular monolith architecture | Single centre, operational simplicity, transactional consistency | Architect |
| 2026-11-06 | DEC-002 | Session-based auth (not JWT) | Simpler revocation, CSRF built-in, less client complexity | Architect |
| 2026-11-06 | DEC-003 | Separate USERS from STUDENTS/TEACHERS | Auth identity != academic profile | Architect |
| 2026-11-06 | DEC-004 | Immutable test attempts (never overwrite) | Audit trail, progress tracking, retake support | Architect |
| 2026-11-06 | DEC-005 | Criteria-level storage for Writing/Speaking | IELTS requires 4-criterion breakdown | Architect |
| 2026-11-06 | DEC-006 | Payment transactions separate from fee structures | Accurate balance, audit trail | Architect |
| 2026-11-06 | DEC-007 | Soft deletes for academic/financial records | Compliance, recovery, retention | Architect |
| 2026-11-06 | DEC-008 | Teacher-evaluated W/S; Auto L/R | Matches IELTS reality; AI postponed | Architect |
| 2026-11-06 | DEC-009 | In-app notifications first; Email/SMS/WhatsApp later | MVP scope; extensible provider pattern | Architect |
| 2026-11-06 | DEC-010 | Mobile-first responsive design | Students primarily mobile users | Architect |

---

## 5. Open Technical Decisions (To Be Finalized in Phase 1)

| ID | Decision | Options | Factors |
|----|----------|---------|---------|
| TD-01 | Build tool: Maven vs Gradle | Maven (standard) / Gradle (faster, Kotlin DSL) | Team familiarity, build speed |
| TD-02 | API documentation: Springdoc vs manual OpenAPI | Springdoc (auto) / Manual (control) | Maintenance burden |
| TD-03 | Frontend state: React Query + Context vs Zustand | React Query (server) + Context (client) / Zustand (all) | Simplicity vs power |
| TD-04 | Component library: Headless UI vs Radix vs shadcn/ui | Headless UI / Radix / shadcn/ui | Accessibility, customization |
| TD-05 | Form validation: Zod vs Yup vs class-validator | Zod (TypeScript-first) / Yup / class-validator (backend) | Consistency FE/BE |
| TD-06 | Date handling: java.time vs Joda-Time | java.time (standard) / Joda-Time | JDK 21 has java.time |
| TD-07 | PDF generation: PDFBox vs iText vs Flying Saucer | PDFBox (free) / iText (AGPL/commercial) / Flying Saucer (HTML->PDF) | License, features |
| TD-08 | Chart library: Recharts vs Chart.js vs Tremor | Recharts (React-native) / Chart.js (canvas) / Tremor (Tailwind) | Bundle size, features |
| TD-09 | File upload: Direct to S3 vs Backend proxy | Presigned URLs (direct) / Multipart (backend) | Security, progress, size |
| TD-10 | Testing: Playwright vs Cypress vs Selenium | Playwright (modern, multi-browser) / Cypress (DX) / Selenium (legacy) | Team experience |

---

## 6. Dependency Map (External)

| Dependency | Purpose | Version | License | Risk |
|------------|---------|---------|---------|------|
| Spring Boot | Backend framework | 3.2+ | Apache 2.0 | Low |
| Spring Security | Auth/Authorization | 6.x | Apache 2.0 | Low |
| Spring Data JPA | ORM | 3.x | Apache 2.0 | Low |
| PostgreSQL | Database | 15+ | PostgreSQL | Low |
| Flyway | Migrations | 10.x | Apache 2.0 | Low |
| React | Frontend | 18+ | MIT | Low |
| TypeScript | Type safety | 5.x | Apache 2.0 | Low |
| Tailwind CSS | Styling | 3.x | MIT | Low |
| TanStack Query | Server state | 5.x | MIT | Low |
| React Hook Form | Forms | 7.x | MIT | Low |
| Zod | Validation | 3.x | MIT | Low |
| Recharts | Charts | 2.x | MIT | Low |
| PDFBox | PDF generation | 3.x | Apache 2.0 | Low |
| MinIO | Object storage | Latest | AGPL v3 | Medium (license) |
| Razorpay SDK | Payments | Latest | MIT | Low |
| SendGrid SDK | Email | Latest | MIT | Low |

---

End of Assumptions, Risks and Unresolved Questions
