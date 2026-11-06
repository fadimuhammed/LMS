# Functional Requirements Specification

## IELTS Centre Management & Learning Management System

**Version:** 1.0  
**Date:** 2026-11-06  
**Status:** Draft for Review

---

## 1. Introduction

This document specifies the detailed functional requirements for the IELTS Centre Management & LMS. Each requirement is uniquely identified, traceable, and testable.

---

## 2. Module FR-01: Authentication & Authorization

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-01.01 | System shall support email/phone + password login | Must |
| FR-01.02 | System shall enforce role-based access control (Admin, Teacher, Student, Receptionist) on all API endpoints | Must |
| FR-01.03 | Passwords shall be hashed with bcrypt (cost >= 12) | Must |
| FR-01.04 | Sessions shall use httpOnly, secure, sameSite=lax cookies with CSRF protection | Must |
| FR-01.05 | System shall support password reset via email/phone OTP | Must |
| FR-01.06 | Failed login attempts shall be rate-limited (max 10 per 15 min per IP) | Must |
| FR-01.07 | Session timeout after 30 minutes inactivity; extendable on activity | Should |
| FR-01.08 | Admin shall be able to impersonate users for support (audit logged) | Could |

---

## 3. Module FR-02: User & Role Management

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-02.01 | Admin shall create/update/deactivate user accounts | Must |
| FR-02.02 | System shall maintain separate users (auth) and students/teachers (academic) entities | Must |
| FR-02.03 | User profiles: name, email, phone, avatar, role(s), status (active/inactive) | Must |
| FR-02.04 | Role assignment shall support multiple roles per user | Should |
| FR-02.05 | Permission matrix shall be configurable per role | Must |

---

## 4. Module FR-03: Student & Admission Management

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-03.01 | Student registration with auto-generated unique Student ID (format: IELTS-YYYY-NNNN) | Must |
| FR-03.02 | Personal details: full name, DOB, gender, nationality, address, phone, email | Must |
| FR-03.03 | Guardian/emergency contact: name, relationship, phone, email | Must |
| FR-03.04 | Educational background: highest qualification, institution, year | Should |
| FR-03.05 | IELTS profile: target band, intended test date, Academic/General preference, previous scores | Must |
| FR-03.06 | Initial English proficiency assessment (placement test) | Should |
| FR-03.07 | Document upload: ID proof, photo, certificates, previous IELTS TRF | Must |
| FR-03.08 | Student statuses: Enquiry, Assessed, Enrolled, Active, On Hold, Completed, Withdrawn | Must |
| FR-03.09 | Enrolment workflow: Enquiry to Assessment to Course Selection to Batch Allocation to Fee Payment to Active | Must |
| FR-03.10 | Batch allocation with capacity checking and waitlist support | Must |
| FR-03.11 | Student profile timeline showing all academic/administrative events | Must |
| FR-03.12 | Bulk student import from CSV/Excel | Could |
| FR-03.13 | Student search/filter by name, ID, phone, email, status, batch, course | Must |

---

## 5. Module FR-04: Teacher & Staff Management

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-04.01 | Teacher profile: name, email, phone, qualification, certifications, specializations (L/R/W/S) | Must |
| FR-04.02 | Employment details: join date, employment type (full-time/part-time/contractor), status | Must |
| FR-04.03 | Skill tags for assignment matching (e.g., "Writing Task 2", "Speaking Mock") | Should |
| FR-04.04 | Availability calendar for batch assignment | Must |
| FR-04.05 | Teacher dashboard: assigned batches, upcoming classes, pending evaluations, materials | Must |
| FR-04.06 | Performance metrics: classes taught, evaluations completed, student feedback score | Should |
| FR-04.07 | Document upload: resume, certificates, ID proof | Must |
| FR-04.08 | Bulk teacher import from CSV/Excel | Could |

---

## 6. Module FR-05: Course & Batch Management

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-05.01 | Course definition: name, code, description, duration (weeks), total hours, fee, target band, type (Academic/General) | Must |
| FR-05.02 | Batch creation: course, name, start/end date, capacity, teacher(s), classroom, schedule pattern | Must |
| FR-05.03 | Recurring schedule pattern (e.g., Mon/Wed/Fri 10:00-13:00) with holiday/exception handling | Must |
| FR-05.04 | Batch statuses: Draft, Scheduled, Active, Completed, Cancelled | Must |
| FR-05.05 | Student enrolment in batch with capacity enforcement and waitlist | Must |
| FR-05.06 | Batch transfer: move student between batches with attendance/grade preservation | Must |
| FR-05.07 | Batch archive/restore with data retention | Should |
| FR-05.08 | Course prerequisites and co-requisites | Could |

---

## 7. Module FR-06: Timetable & Scheduling

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-06.01 | Master timetable view: all batches, teachers, classrooms, time slots | Must |
| FR-06.02 | Conflict detection: teacher double-booking, classroom double-booking, student clash | Must |
| FR-06.03 | Recurring session generation from pattern (with holiday calendar) | Must |
| FR-06.04 | Ad-hoc session: make-up class, extra session, cancellation with notification | Must |
| FR-06.05 | Teacher availability blocking (unavailable slots) | Must |
| FR-06.06 | Classroom management: name, capacity, equipment, type (regular/computer lab/speaking room) | Must |
| FR-06.07 | Timetable export (PDF/CSV) for teachers and students | Should |
| FR-06.08 | Substitute teacher assignment with notification | Should |

---

## 8. Module FR-07: Attendance Management

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-07.01 | Attendance marking: Present, Absent, Late, Excused (per session per student) | Must |
| FR-07.02 | Bulk attendance entry with keyboard shortcuts (P/A/L/E) | Must |
| FR-07.03 | Attendance percentage calculation per student per batch (sessions attended / sessions held) | Must |
| FR-07.04 | Leave request workflow: student submits, teacher/admin approves/rejects | Must |
| FR-07.05 | Attendance correction workflow with audit trail (who, when, old value, new value, reason) | Must |
| FR-07.06 | Low attendance alerts (configurable threshold, default 75%) to student, teacher, admin | Must |
| FR-07.07 | Attendance reports: daily, weekly, monthly, per batch, per student | Must |
| FR-07.08 | QR code check-in for students (future enhancement) | Could |

---

## 9. Module FR-08: Fee & Payment Management

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-08.01 | Fee structure: course fee, registration fee, material fee, exam fee, discounts (early bird, sibling, referral) | Must |
| FR-08.02 | Invoice generation: auto on enrolment, manual for ad-hoc fees, GST-compliant (GSTIN, HSN/SAC) | Must |
| FR-08.03 | Installment plans: configurable number, amounts, due dates, late fee rules | Must |
| FR-08.04 | Payment recording: cash, card, UPI, bank transfer, cheque, online gateway (Razorpay) | Must |
| FR-08.05 | Partial payment allocation logic (oldest invoice first, or configurable) | Must |
| FR-08.06 | Payment receipt generation (PDF) with GST details, unique receipt number | Must |
| FR-08.07 | Fee balance tracking: total due, paid, outstanding, overdue per student | Must |
| FR-08.08 | Fee waiver/discount approval workflow (teacher recommends, admin approves) | Should |
| FR-08.09 | Refund processing with approval workflow and audit trail | Should |
| FR-08.10 | Outstanding fee reports: ageing, by batch, by student, collection summary | Must |
| FR-08.11 | Payment gateway integration: Razorpay (server-side verification, webhook handling, idempotency) | Should |
| FR-08.12 | Financial year rollover and reporting | Could |

---

## 10. Module FR-09: Study Materials & Content Management

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-09.01 | Material categories: Listening audio, Reading passages, Writing prompts, Speaking topics, Vocabulary, Grammar, Mock Tests, Answer Keys | Must |
| FR-09.02 | Material metadata: title, skill, sub-skill, difficulty (1-9), tags, estimated time, IELTS version (Academic/General) | Must |
| FR-09.03 | File upload: PDF, MP3, MP4, DOCX, images with preview | Must |
| FR-09.04 | Version control: replace file, keep history, student sees latest | Should |
| FR-09.05 | Batch-specific material assignment with release schedule | Must |
| FR-09.06 | Student material library: filter by skill, difficulty, assigned/recommended | Must |
| FR-09.07 | Teacher material library: all materials, create/edit, bulk upload | Must |
| FR-09.08 | Material access logging (who accessed what, when) | Could |

---

## 11. Module FR-10: IELTS Practice - Listening

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-10.01 | Audio player with play/pause, seek, speed (0.5x-2x), volume, fullscreen | Must |
| FR-10.02 | Section-based playback (4 sections) with independent audio per section | Must |
| FR-10.03 | Question types: MCQ, Matching, Plan/Map/Diagram Labelling, Form/Note/Table/Flow-chart/Summary Completion, Sentence Completion | Must |
| FR-10.04 | Answer input: text, dropdown, drag-drop (matching), clickable image (map labelling) | Must |
| FR-10.05 | Timer: 30 minutes total + 10 minutes transfer (simulated) | Must |
| FR-10.06 | Auto-save answers every 30 seconds and on navigation | Must |
| FR-10.07 | Submit: confirmation modal, no accidental submit | Must |
| FR-10.08 | Auto-scoring: answer key comparison, band score conversion (official IELTS table) | Must |
| FR-10.09 | Review mode: show correct/incorrect, play audio for specific question, explanation | Must |
| FR-10.10 | Attempt history with scores, band, date, time taken | Must |

---

## 12. Module FR-11: IELTS Practice - Reading

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-11.01 | Passage viewer: scrollable text with question sidebar, highlight text, search | Must |
| FR-11.02 | Question types: MCQ, True/False/Not Given, Yes/No/Not Given, Matching Headings/Information/Features/Sentence Endings, Sentence/Note/Table/Flow-chart/Summary Completion, Diagram Label Completion, Short Answer | Must |
| FR-11.03 | Timer: 60 minutes for 3 passages | Must |
| FR-11.04 | Auto-save every 30 seconds and on navigation | Must |
| FR-11.05 | Submit with confirmation | Must |
| FR-11.06 | Auto-scoring with band conversion (Academic/General separate tables) | Must |
| FR-11.07 | Review mode: highlight answer location in passage, explanation | Must |
| FR-11.08 | Attempt history with scores, band, date, time taken | Must |

---

## 13. Module FR-12: IELTS Practice - Writing

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-12.01 | Task 1 (Academic: report; General: letter) and Task 2 (essay) editors | Must |
| FR-12.02 | Word count display (live), target word count indicators | Must |
| FR-12.03 | Timer: 20 min Task 1 + 40 min Task 2 = 60 min total | Must |
| FR-12.04 | Auto-save every 30 seconds | Must |
| FR-12.05 | Submit for evaluation: goes to teacher queue | Must |
| FR-12.06 | Teacher evaluation interface: side-by-side student response + rubric | Must |
| FR-12.07 | Rubric: 4 criteria (Task Achievement/Response, Coherence & Cohesion, Lexical Resource, Grammatical Range & Accuracy) each 0-9 | Must |
| FR-12.08 | Inline annotations: highlight text, add comment (Grammar, Vocab, Structure, Task) | Must |
| FR-12.09 | Overall band calculation: mean of 4 criteria rounded to nearest 0.5 (IELTS rounding) | Must |
| FR-12.10 | Feedback: written summary + audio feedback (optional) | Should |
| FR-12.11 | Resubmission support: student revises, teacher re-evaluates, version history | Must |
| FR-12.12 | Model answer / sample response attachment | Should |
| FR-12.13 | Attempt history with all versions, bands, feedback | Must |

---

## 14. Module FR-13: IELTS Practice - Speaking

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-13.01 | Part 1 (Introduction), Part 2 (Cue Card 1-2 min), Part 3 (Discussion) structure | Must |
| FR-13.02 | Audio recorder: start/stop, visual level meter, max duration per part | Must |
| FR-13.03 | Cue card display with 1 min prep timer + 2 min speaking timer | Must |
| FR-13.04 | Auto-upload recording on stop (chunked, resumable) | Must |
| FR-13.05 | Teacher evaluation: audio player with seek, speed, volume | Must |
| FR-13.06 | Rubric: 4 criteria (Fluency & Coherence, Lexical Resource, Grammatical Range, Pronunciation) each 0-9 | Must |
| FR-13.07 | Timestamped annotations on audio waveform | Should |
| FR-13.08 | Audio feedback recording by teacher | Should |
| FR-13.09 | Overall band: mean of 4 criteria rounded to nearest 0.5 | Must |
| FR-13.10 | Resubmission with version history | Must |
| FR-13.11 | Attempt history with recordings, bands, feedback | Must |

---

## 15. Module FR-14: Mock Test Management

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-14.01 | Mock test definition: name, date, time limit, sections (L/R/W/S), pass mark | Must |
| FR-14.02 | Question assembly: select from question bank per section, randomize order (configurable) | Must |
| FR-14.03 | Scheduled vs on-demand mock tests | Must |
| FR-14.04 | Test interface: unified timer, section navigation, flag for review, progress indicator | Must |
| FR-14.05 | Auto-save every 30 seconds + on section change | Must |
| FR-14.06 | Submission: L/R auto-scored immediately, W/S queued for evaluation | Must |
| FR-14.07 | Results: overall band, section bands, percentile (if cohort > 10), time analysis | Must |
| FR-14.08 | Review mode: all sections with answers, explanations, teacher feedback (W/S) | Must |
| FR-14.09 | Mock test attempt history with comparison charts | Must |
| FR-14.10 | Certificate of completion (configurable template) | Could |

---

## 16. Module FR-15: Question Bank & Test Assembly

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-15.01 | Question CRUD: stem, options, correct answer, explanation, skill, sub-skill, difficulty, tags | Must |
| FR-15.02 | Question types: all IELTS types for L/R/W/S (see FR-10, FR-11, FR-12, FR-13) | Must |
| FR-15.03 | Media attachments: audio (L), passage (R), prompt (W), cue card (S) | Must |
| FR-15.04 | Versioning: edit creates new version, old attempts reference original | Should |
| FR-15.05 | Bulk import from structured format (CSV/JSON) | Could |
| FR-15.06 | Test assembly: blueprint (counts per type/difficulty), auto-generate or manual select | Must |
| FR-15.07 | Question analytics: facility index, discrimination index, exposure count | Should |

---

## 17. Module FR-16: Band Score Tracking & Progress Analytics

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-16.01 | Band snapshot: record overall + 4 skills per attempt (practice, mock, official) | Must |
| FR-16.02 | Progress chart: line chart over time (overall + 4 skills) with target band line | Must |
| FR-16.03 | Weakness analysis: lowest skill, recurring sub-skill gaps, question type struggles | Must |
| FR-16.04 | Comparison: student vs batch average, vs centre average, vs target | Must |
| FR-16.05 | Predictive band estimate based on recent practice (weighted) | Should |
| FR-16.06 | Teacher view: all students in batch with progress indicators | Must |
| FR-16.07 | Export progress report (PDF) for student/parent | Should |

---

## 18. Module FR-17: Notifications & Communication

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-17.01 | In-app notification centre: bell icon, unread count, mark read, archive | Must |
| FR-17.02 | Notification types: attendance alert, fee due, class reschedule, evaluation ready, mock test scheduled, announcement | Must |
| FR-17.03 | Email delivery: transactional (SendGrid/SMTP) with templates | Must |
| FR-17.04 | SMS/WhatsApp delivery (provider integration) | Should |
| FR-17.05 | Notification preferences per user (channel, frequency, digest) | Should |
| FR-17.06 | Announcement broadcast: all, by role, by batch, by course | Must |
| FR-17.07 | Notification audit log (sent, delivered, failed, read) | Should |

---

## 19. Module FR-18: Reporting & Analytics

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-18.01 | Admission funnel: enquiries to enrolled, conversion rates, source tracking | Must |
| FR-18.02 | Attendance summary: by batch, by student, by teacher, trends | Must |
| FR-18.03 | Fee collection: daily/weekly/monthly, outstanding ageing, payment mode split | Must |
| FR-18.04 | Teacher workload: classes, evaluations pending/completed, student count | Must |
| FR-18.05 | Student performance: band distribution, improvement rate, mock test trends | Must |
| FR-18.06 | Course effectiveness: completion rate, average band improvement, feedback score | Should |
| FR-18.07 | Custom report builder: drag-drop fields, filters, group by, schedule | Could |
| FR-18.08 | Export all reports: PDF, Excel, CSV | Must |

---

## 20. Module FR-19: System Administration

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-19.01 | Audit log: all CRUD on sensitive entities (users, students, fees, grades) with who, when, what, old/new | Must |
| FR-19.02 | System settings: centre name, address, GSTIN, contact, branding, defaults (attendance threshold, late fee) | Must |
| FR-19.03 | Holiday calendar management (affects timetable generation) | Must |
| FR-19.04 | Backup trigger and status monitoring | Should |
| FR-19.05 | Data retention policy enforcement (configurable per entity) | Should |
| FR-19.06 | Feature flags for gradual rollout | Could |

---

## 21. Module FR-20: Mobile & Offline Support (Future)

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-20.01 | PWA: installable, offline caching for static assets | Could |
| FR-20.02 | Offline practice: download mock test, attempt offline, sync on reconnect | Could |
| FR-20.03 | Push notifications (web push) | Could |
| FR-20.04 | Mobile app (React Native/Expo) | Could |

---

## 22. Cross-Cutting Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-X.01 | All timestamps in Asia/Kolkata timezone | Must |
| FR-X.02 | All currency in INR (₹) with 2 decimal precision | Must |
| FR-X.03 | Soft delete for all academic/financial records (deleted_at, deleted_by) | Must |
| FR-X.04 | Optimistic locking on concurrent edits (version column) | Must |
| FR-X.05 | API rate limiting: 100 req/min per user, 1000 req/min per IP | Must |
| FR-X.06 | File upload limits: 100MB max, type validation, virus scan (ClamAV) | Must |
| FR-X.07 | GDPR/DPDP compliance: data export, deletion, consent records | Must |
| FR-X.08 | Accessibility: WCAG 2.1 AA compliance | Must |
| FR-X.09 | Responsive: desktop-first, mobile usable (320px+) | Must |
| FR-X.10 | Browser support: Chrome 120+, Firefox 120+, Safari 17+, Edge 120+ | Must |

---

End of Functional Requirements Specification