# Database ER Diagram & Schema Design

## IELTS Centre Management & Learning Management System

**Version:** 1.0  
**Date:** 2026-11-06  
**Status:** Draft for Review

---

## 1. ER Diagram (Text Representation)

`
USERS (Authentication)
+-- id (PK)
+-- email (unique)
+-- phone (unique)
+-- password_hash
+-- full_name
+-- avatar_url
+-- status (ACTIVE/INACTIVE)
+-- last_login_at
+-- created_at
+-- updated_at
+-- deleted_at (soft delete)

ROLES
+-- id (PK)
+-- code (ADMIN, TEACHER, STUDENT, RECEPTIONIST)
+-- name
+-- description

USER_ROLES
+-- user_id (FK ? USERS)
+-- role_id (FK ? ROLES)
+-- PRIMARY KEY (user_id, role_id)

PERMISSIONS
+-- id (PK)
+-- resource
+-- action (CREATE/READ/UPDATE/DELETE/ADMIN)
+-- description

ROLE_PERMISSIONS
+-- role_id (FK ? ROLES)
+-- permission_id (FK ? PERMISSIONS)
+-- PRIMARY KEY (role_id, permission_id)

SESSIONS
+-- id (PK)
+-- user_id (FK ? USERS)
+-- token_hash
+-- expires_at
+-- created_at
+-- ip_address
+-- user_agent

AUDIT_LOGS
+-- id (PK)
+-- user_id (FK ? USERS)
+-- action
+-- entity_type
+-- entity_id
+-- old_values (JSON)
+-- new_values (JSON)
+-- ip_address
+-- created_at
+-- correlation_id

---

STUDENTS (Academic Profile)
+-- id (PK)
+-- user_id (FK ? USERS, unique, nullable)
+-- student_id (unique, format: IELTS-YYYY-NNNN)
+-- first_name
+-- last_name
+-- date_of_birth
+-- gender
+-- nationality
+-- address
+-- phone
+-- email
+-- guardian_name
+-- guardian_relationship
+-- guardian_phone
+-- guardian_email
+-- highest_qualification
+-- institution
+-- graduation_year
+-- target_band (decimal, e.g., 7.5)
+-- intended_test_date
+-- ielts_type (ACADEMIC/GENERAL)
+-- previous_listening (decimal)
+-- previous_reading (decimal)
+-- previous_writing (decimal)
+-- previous_speaking (decimal)
+-- previous_overall (decimal)
+-- proficiency_level (BEGINNER/INTERMEDIATE/ADVANCED)
+-- status (ENQUIRY/ASSESSED/ENROLLED/ACTIVE/ON_HOLD/COMPLETED/WITHDRAWN)
+-- enrolled_at
+-- completed_at
+-- created_at
+-- updated_at
+-- deleted_at

STUDENT_DOCUMENTS
+-- id (PK)
+-- student_id (FK ? STUDENTS)
+-- document_type (ID_PROOF/PHOTO/CERTIFICATE/TRF/OTHER)
+-- file_name
+-- file_url
+-- file_size
+-- mime_type
+-- uploaded_by (FK ? USERS)
+-- created_at
+-- deleted_at

ENQUIRIES
+-- id (PK)
+-- student_id (FK ? STUDENTS, nullable)
+-- source (WALK_IN/PHONE/WEBSITE/REFERRAL/OTHER)
+-- status (NEW/CONTACTED/ASSESSED/CONVERTED/LOST)
+-- notes
+-- assigned_to (FK ? USERS)
+-- follow_up_date
+-- created_at
+-- updated_at

---

TEACHERS (Academic Profile)
+-- id (PK)
+-- user_id (FK ? USERS, unique, nullable)
+-- employee_id (unique)
+-- first_name
+-- last_name
+-- phone
+-- email
+-- qualification
+-- ielts_certifications (JSON array)
+-- years_experience
+-- specialisation (JSON array of skills)
+-- hourly_rate
+-- status (ACTIVE/INACTIVE/ON_LEAVE)
+-- joined_at
+-- created_at
+-- updated_at
+-- deleted_at

TEACHER_SPECIALISATIONS
+-- id (PK)
+-- teacher_id (FK ? TEACHERS)
+-- skill (LISTENING/READING/WRITING/SPEAKING/SPOKEN_ENGLISH)
+-- proficiency_level
+-- PRIMARY KEY (teacher_id, skill)

TEACHER_WORKLOAD
+-- id (PK)
+-- teacher_id (FK ? TEACHERS)
+-- batch_id (FK ? BATCHES)
+-- skill
+-- hours_per_week
+-- effective_from
+-- effective_to
+-- created_at

---

COURSES
+-- id (PK)
+-- code (unique)
+-- name
+-- description
+-- type (ACADEMIC/GENERAL/FOUNDATION/INTENSIVE/CRASH/SPOKEN_ENGLISH)
+-- duration_weeks
+-- total_hours
+-- fee_amount (decimal, INR)
+-- admission_fee (decimal, INR)
+-- material_fee (decimal, INR)
+-- exam_fee (decimal, INR)
+-- syllabus (JSON)
+-- learning_objectives (JSON)
+-- is_active
+-- created_at
+-- updated_at
+-- deleted_at

BATCHES
+-- id (PK)
+-- batch_id (unique, format: BATCH-YYYY-NNNN)
+-- course_id (FK ? COURSES)
+-- name
+-- start_date
+-- end_date
+-- capacity
+-- status (SCHEDULED/ACTIVE/COMPLETED/ARCHIVED/CANCELLED)
+-- classroom_id (FK ? CLASSROOMS, nullable)
+-- created_at
+-- updated_at
+-- deleted_at

BATCH_STUDENTS
+-- id (PK)
+-- batch_id (FK ? BATCHES)
+-- student_id (FK ? STUDENTS)
+-- enrolled_at
+-- status (ACTIVE/TRANSFERRED/COMPLETED/WITHDRAWN)
+-- transferred_from_batch_id (FK ? BATCHES, nullable)
+-- transferred_at
+-- PRIMARY KEY (batch_id, student_id)

BATCH_TEACHERS
+-- id (PK)
+-- batch_id (FK ? BATCHES)
+-- teacher_id (FK ? TEACHERS)
+-- skill (LISTENING/READING/WRITING/SPEAKING/SPOKEN_ENGLISH)
+-- is_primary
+-- assigned_at
+-- PRIMARY KEY (batch_id, teacher_id, skill)

CLASSROOMS
+-- id (PK)
+-- name
+-- capacity
+-- has_projector
+-- has_audio_system
+-- is_active
+-- created_at
+-- updated_at

---

CLASS_SESSIONS
+-- id (PK)
+-- batch_id (FK ? BATCHES)
+-- teacher_id (FK ? TEACHERS)
+-- classroom_id (FK ? CLASSROOMS)
+-- session_date
+-- start_time
+-- end_time
+-- skill (LISTENING/READING/WRITING/SPEAKING/SPOKEN_ENGLISH)
+-- topic
+-- status (SCHEDULED/COMPLETED/CANCELLED/RESCHEDULED)
+-- rescheduled_from_id (FK ? CLASS_SESSIONS, nullable)
+-- cancellation_reason
+-- created_at
+-- updated_at
+-- deleted_at

TEACHER_AVAILABILITY
+-- id (PK)
+-- teacher_id (FK ? TEACHERS)
+-- day_of_week (0-6)
+-- start_time
+-- end_time
+-- is_available
+-- effective_from
+-- effective_to
+-- created_at

---

ATTENDANCE_RECORDS
+-- id (PK)
+-- class_session_id (FK ? CLASS_SESSIONS)
+-- student_id (FK ? STUDENTS)
+-- status (PRESENT/ABSENT/LATE/EXCUSED)
+-- marked_by (FK ? USERS)
+-- marked_at
+-- leave_request_id (FK ? LEAVE_REQUESTS, nullable)
+-- correction_id (FK ? ATTENDANCE_CORRECTIONS, nullable)
+-- created_at
+-- updated_at

LEAVE_REQUESTS
+-- id (PK)
+-- student_id (FK ? STUDENTS)
+-- class_session_id (FK ? CLASS_SESSIONS)
+-- reason
+-- status (PENDING/APPROVED/REJECTED)
+-- reviewed_by (FK ? USERS)
+-- reviewed_at
+-- created_at
+-- updated_at

ATTENDANCE_CORRECTIONS
+-- id (PK)
+-- attendance_record_id (FK ? ATTENDANCE_RECORDS)
+-- old_status
+-- new_status
+-- reason
+-- corrected_by (FK ? USERS)
+-- corrected_at
+-- created_at

---

FEE_STRUCTURES
+-- id (PK)
+-- course_id (FK ? COURSES)
+-- component (ADMISSION/COURSE/MATERIAL/EXAM/OTHER)
+-- amount (decimal, INR)
+-- is_mandatory
+-- description
+-- effective_from
+-- effective_to
+-- created_at
+-- updated_at

INVOICES
+-- id (PK)
+-- invoice_number (unique)
+-- student_id (FK ? STUDENTS)
+-- batch_id (FK ? BATCHES)
+-- total_amount (decimal, INR)
+-- paid_amount (decimal, INR)
+-- balance_amount (decimal, INR)
+-- status (DRAFT/SENT/PARTIALLY_PAID/PAID/OVERDUE/CANCELLED)
+-- due_date
+-- gst_number
+-- gst_amount (decimal, INR)
+-- created_at
+-- updated_at
+-- deleted_at

INVOICE_ITEMS
+-- id (PK)
+-- invoice_id (FK ? INVOICES)
+-- fee_structure_id (FK ? FEE_STRUCTURES)
+-- description
+-- quantity
+-- unit_price (decimal, INR)
+-- total_price (decimal, INR)
+-- created_at

PAYMENTS
+-- id (PK)
+-- payment_number (unique)
+-- invoice_id (FK ? INVOICES)
+-- student_id (FK ? STUDENTS)
+-- amount (decimal, INR)
+-- mode (CASH/UPI/BANK_TRANSFER/CARD/OTHER)
+-- reference_number
+-- paid_at
+-- received_by (FK ? USERS)
+-- status (SUCCESS/FAILED/PENDING/REFUNDED)
+-- gateway_response (JSON, nullable)
+-- idempotency_key (unique, for gateway callbacks)
+-- created_at
+-- updated_at

RECEIPTS
+-- id (PK)
+-- receipt_number (unique)
+-- payment_id (FK ? PAYMENTS)
+-- student_id (FK ? STUDENTS)
+-- amount (decimal, INR)
+-- mode
+-- reference_number
+-- issued_at
+-- issued_by (FK ? USERS)
+-- pdf_url
+-- created_at
+-- updated_at

DISCOUNTS
+-- id (PK)
+-- student_id (FK ? STUDENTS)
+-- invoice_id (FK ? INVOICES, nullable)
+-- type (PERCENTAGE/FIXED)
+-- value (decimal)
+-- reason
+-- approved_by (FK ? USERS)
+-- valid_from
+-- valid_to
+-- is_active
+-- created_at
+-- updated_at

REFUNDS
+-- id (PK)
+-- payment_id (FK ? PAYMENTS)
+-- amount (decimal, INR)
+-- reason
+-- status (PENDING/APPROVED/REJECTED/PROCESSED)
+-- approved_by (FK ? USERS)
+-- processed_at
+-- created_at
+-- updated_at

---

LEARNING_MATERIALS
+-- id (PK)
+-- title
+-- description
+-- file_url
+-- file_name
+-- file_size
+-- mime_type
+-- course_id (FK ? COURSES, nullable)
+-- skill (LISTENING/READING/WRITING/SPEAKING/GENERAL)
+-- topic
+-- level (BEGINNER/INTERMEDIATE/ADVANCED)
+-- tags (JSON array)
+-- uploaded_by (FK ? USERS)
+-- is_published
+-- created_at
+-- updated_at
+-- deleted_at

MATERIAL_ASSIGNMENTS
+-- id (PK)
+-- material_id (FK ? LEARNING_MATERIALS)
+-- batch_id (FK ? BATCHES, nullable)
+-- student_id (FK ? STUDENTS, nullable)
+-- assigned_by (FK ? USERS)
+-- assigned_at
+-- due_date (nullable)
+-- PRIMARY KEY (material_id, batch_id, student_id)

---

ASSIGNMENTS
+-- id (PK)
+-- title
+-- description
+-- skill (LISTENING/READING/WRITING/SPEAKING/SPOKEN_ENGLISH)
+-- batch_id (FK ? BATCHES, nullable)
+-- student_id (FK ? STUDENTS, nullable)
+-- max_marks
+-- deadline
+-- allow_late_submission
+-- late_penalty_percentage
+-- created_by (FK ? USERS)
+-- created_at
+-- updated_at
+-- deleted_at

SUBMISSIONS
+-- id (PK)
+-- assignment_id (FK ? ASSIGNMENTS)
+-- student_id (FK ? STUDENTS)
+-- submission_type (TEXT/DOCUMENT/IMAGE/AUDIO)
+-- content_text (TEXT, nullable)
+-- file_url (nullable)
+-- file_name (nullable)
+-- word_count (nullable)
+-- submitted_at
+-- status (SUBMITTED/UNDER_REVIEW/EVALUATED/RESUBMISSION_REQUIRED)
+-- evaluated_by (FK ? USERS, nullable)
+-- evaluated_at
+-- marks_obtained (decimal, nullable)
+-- feedback (TEXT, nullable)
+-- version
+-- parent_submission_id (FK ? SUBMISSIONS, nullable)
+-- created_at
+-- updated_at

SUBMISSION_FEEDBACK
+-- id (PK)
+-- submission_id (FK ? SUBMISSIONS)
+-- feedback_type (ANNOTATION/COMMENT/RUBRIC)
+-- content (JSON)
+-- created_by (FK ? USERS)
+-- created_at
+-- updated_at

---

QUESTIONS
+-- id (PK)
+-- skill (LISTENING/READING/WRITING/SPEAKING)
+-- type (MCQ/MATCHING/MAP_LABEL/FORM_COMPLETION/SENTENCE_COMPLETION/TRUE_FALSE_NOT_GIVEN/YES_NO_NOT_GIVEN/MATCHING_HEADINGS/MATCHING_INFO/SUMMARY_COMPLETION/SHORT_ANSWER/ESSAY/CUE_CARD/DISCUSSION)
+-- difficulty (EASY/MEDIUM/HARD)
+-- ielts_type (ACADEMIC/GENERAL/BOTH)
+-- question_text (TEXT)
+-- passage_id (FK ? PASSAGES, nullable)
+-- audio_clip_id (FK ? AUDIO_CLIPS, nullable)
+-- image_url (nullable)
+-- options (JSON array, for objective questions)
+-- correct_answer (JSON)
+-- explanation (TEXT)
+-- marks
+-- created_by (FK ? USERS)
+-- created_at
+-- updated_at
+-- deleted_at

PASSAGES
+-- id (PK)
+-- title
+-- content (TEXT)
+-- skill (READING/WRITING)
+-- ielts_type (ACADEMIC/GENERAL)
+-- word_count
+-- source
+-- created_by (FK ? USERS)
+-- created_at
+-- updated_at

AUDIO_CLIPS
+-- id (PK)
+-- title
+-- file_url
+-- duration_seconds
+-- transcript (TEXT)
+-- skill (LISTENING/SPEAKING)
+-- created_by (FK ? USERS)
+-- created_at
+-- updated_at

TESTS
+-- id (PK)
+-- title
+-- description
+-- test_type (SKILL_SPECIFIC/FULL_LENGTH/CUSTOM)
+-- skill (LISTENING/READING/WRITING/SPEAKING/ALL)
+-- ielts_type (ACADEMIC/GENERAL)
+-- duration_minutes
+-- total_marks
+-- passing_marks
+-- start_window (nullable)
+-- end_window (nullable)
+-- max_attempts
+-- randomize_questions
+-- randomize_options
+-- is_published
+-- created_by (FK ? USERS)
+-- created_at
+-- updated_at
+-- deleted_at

TEST_SECTIONS
+-- id (PK)
+-- test_id (FK ? TESTS)
+-- section_order
+-- skill
+-- title
+-- instructions
+-- duration_minutes
+-- passage_id (FK ? PASSAGES, nullable)
+-- audio_clip_id (FK ? AUDIO_CLIPS, nullable)
+-- created_at
+-- updated_at

TEST_QUESTIONS
+-- id (PK)
+-- test_section_id (FK ? TEST_SECTIONS)
+-- question_id (FK ? QUESTIONS)
+-- question_order
+-- marks
+-- PRIMARY KEY (test_section_id, question_id)

TEST_ATTEMPTS
+-- id (PK)
+-- test_id (FK ? TESTS)
+-- student_id (FK ? STUDENTS)
+-- batch_id (FK ? BATCHES, nullable)
+-- started_at
+-- submitted_at
+-- status (IN_PROGRESS/SUBMITTED/EVALUATED/EXPIRED)
+-- total_marks_obtained (decimal, nullable)
+-- listening_score (decimal, nullable)
+-- reading_score (decimal, nullable)
+-- writing_band (decimal, nullable)
+-- speaking_band (decimal, nullable)
+-- overall_band (decimal, nullable)
+-- time_spent_seconds
+-- created_at
+-- updated_at

STUDENT_ANSWERS
+-- id (PK)
+-- test_attempt_id (FK ? TEST_ATTEMPTS)
+-- question_id (FK ? QUESTIONS)
+-- answer (JSON)
+-- is_correct (nullable, for objective)
+-- marks_obtained (decimal, nullable)
+-- evaluated_by (FK ? USERS, nullable)
+-- evaluated_at
+-- created_at
+-- updated_at

SKILL_SCORES
+-- id (PK)
+-- test_attempt_id (FK ? TEST_ATTEMPTS)
+-- skill (LISTENING/READING/WRITING/SPEAKING)
+-- raw_score (decimal)
+-- band_score (decimal, 0-9, 0.5 increments)
+-- criterion_scores (JSON: {task_achievement: 7, coherence: 6.5, ...})
+-- evaluator_id (FK ? USERS, nullable)
+-- evaluated_at
+-- created_at
+-- updated_at

---

WRITING_SUBMISSIONS
+-- id (PK)
+-- test_attempt_id (FK ? TEST_ATTEMPTS, nullable)
+-- assignment_id (FK ? ASSIGNMENTS, nullable)
+-- student_id (FK ? STUDENTS)
+-- task_type (TASK_1/TASK_2)
+-- ielts_type (ACADEMIC/GENERAL)
+-- prompt (TEXT)
+-- response_text (TEXT)
+-- word_count
+-- file_url (nullable)
+-- submitted_at
+-- status (SUBMITTED/EVALUATED/RESUBMISSION_REQUIRED)
+-- created_at
+-- updated_at

WRITING_EVALUATIONS
+-- id (PK)
+-- writing_submission_id (FK ? WRITING_SUBMISSIONS)
+-- task_achievement (decimal, 0-9)
+-- coherence_cohesion (decimal, 0-9)
+-- lexical_resource (decimal, 0-9)
+-- grammatical_range_accuracy (decimal, 0-9)
+-- overall_band (decimal, 0-9)
+-- feedback (TEXT)
+-- annotated_text (JSON, for inline annotations)
+-- evaluated_by (FK ? USERS)
+-- evaluated_at
+-- created_at
+-- updated_at

---

SPEAKING_SUBMISSIONS
+-- id (PK)
+-- test_attempt_id (FK ? TEST_ATTEMPTS, nullable)
+-- assignment_id (FK ? ASSIGNMENTS, nullable)
+-- student_id (FK ? STUDENTS)
+-- part (PART_1/PART_2/PART_3)
+-- cue_card_id (FK ? CUE_CARDS, nullable)
+-- audio_url
+-- duration_seconds
+-- submitted_at
+-- status (SUBMITTED/EVALUATED/RESUBMISSION_REQUIRED)
+-- created_at
+-- updated_at

SPEAKING_EVALUATIONS
+-- id (PK)
+-- speaking_submission_id (FK ? SPEAKING_SUBMISSIONS)
+-- fluency_coherence (decimal, 0-9)
+-- lexical_resource (decimal, 0-9)
+-- grammatical_range_accuracy (decimal, 0-9)
+-- pronunciation (decimal, 0-9)
+-- overall_band (decimal, 0-9)
+-- feedback (TEXT)
+-- feedback_audio_url (nullable)
+-- evaluated_by (FK ? USERS)
+-- evaluated_at
+-- created_at
+-- updated_at

CUE_CARDS
+-- id (PK)
+-- topic
+-- prompts (JSON array)
+-- preparation_time_seconds
+-- speaking_time_seconds
+-- created_by (FK ? USERS)
+-- created_at
+-- updated_at

---

BAND_SCORES
+-- id (PK)
+-- student_id (FK ? STUDENTS)
+-- test_attempt_id (FK ? TEST_ATTEMPTS, nullable)
+-- listening_band (decimal)
+-- reading_band (decimal)
+-- writing_band (decimal)
+-- speaking_band (decimal)
+-- overall_band (decimal)
+-- target_band (decimal)
+-- recorded_at
+-- source (MOCK_TEST/PRACTICE/ASSIGNMENT/TEACHER_ASSESSMENT)
+-- notes
+-- created_at

PROGRESS_SNAPSHOTS
+-- id (PK)
+-- student_id (FK ? STUDENTS)
+-- batch_id (FK ? BATCHES, nullable)
+-- snapshot_date
+-- listening_band (decimal)
+-- reading_band (decimal)
+-- writing_band (decimal)
+-- speaking_band (decimal)
+-- overall_band (decimal)
+-- attendance_percentage (decimal)
+-- assignments_completed (int)
+-- assignments_pending (int)
+-- mock_tests_taken (int)
+-- created_at
+-- updated_at

WEAKNESS_ANALYSIS
+-- id (PK)
+-- student_id (FK ? STUDENTS)
+-- skill (LISTENING/READING/WRITING/SPEAKING)
+-- question_type
+-- accuracy_percentage (decimal)
+-- attempts_count
+-- last_attempt_date
+-- recommendation (TEXT)
+-- created_at
+-- updated_at

---

NOTIFICATIONS
+-- id (PK)
+-- user_id (FK ? USERS)
+-- type (SYSTEM/CLASS_REMINDER/ASSIGNMENT_REMINDER/EXAM_REMINDER/FEE_REMINDER/ATTENDANCE_ALERT/ANNOUNCEMENT/BATCH_NOTICE/TEACHER_MESSAGE)
+-- title
+-- message
+-- reference_type (STUDENT/BATCH/ASSIGNMENT/TEST/INVOICE/OTHER)
+-- reference_id (nullable)
+-- is_read
+-- read_at
+-- created_at
+-- updated_at

ANNOUNCEMENTS
+-- id (PK)
+-- title
+-- content
+-- target_roles (JSON array)
+-- target_batch_ids (JSON array, nullable)
+-- published_by (FK ? USERS)
+-- published_at
+-- expires_at
+-- is_pinned
+-- created_at
+-- updated_at

MESSAGES
+-- id (PK)
+-- conversation_id
+-- sender_id (FK ? USERS)
+-- recipient_id (FK ? USERS)
+-- content
+-- is_read
+-- read_at
+-- created_at
+-- updated_at

NOTIFICATION_TEMPLATES
+-- id (PK)
+-- name
+-- subject_template
+-- body_template
+-- channel (IN_APP/EMAIL/SMS/WHATSAPP)
+-- variables (JSON array)
+-- is_active
+-- created_at
+-- updated_at

---

REPORTS
+-- id (PK)
+-- name
+-- description
+-- query_template (JSON)
+-- parameters_schema (JSON)
+-- output_format (PDF/EXCEL/CSV)
+-- is_scheduled
+-- schedule_cron (nullable)
+-- created_by (FK ? USERS)
+-- created_at
+-- updated_at

CERTIFICATES
+-- id (PK)
+-- certificate_number (unique)
+-- student_id (FK ? STUDENTS)
+-- course_id (FK ? COURSES)
+-- batch_id (FK ? BATCHES)
+-- template_id (FK ? CERTIFICATE_TEMPLATES)
+-- issued_at
+-- issued_by (FK ? USERS)
+-- pdf_url
+-- verification_code (unique)
+-- is_revoked
+-- revoked_at
+-- revoked_by (FK ? USERS, nullable)
+-- created_at
+-- updated_at

CERTIFICATE_TEMPLATES
+-- id (PK)
+-- name
+-- html_template
+-- css_styles
+-- variables (JSON array)
+-- is_default
+-- created_at
+-- updated_at

---

SYSTEM_SETTINGS
+-- id (PK)
+-- key (unique)
+-- value (TEXT)
+-- description
+-- category (CENTRE/ACADEMIC/NOTIFICATION/SECURITY/FILE_STORAGE)
+-- is_public
+-- created_at
+-- updated_at

BACKUP_CONFIG
+-- id (PK)
+-- schedule_cron
+-- retention_days
+-- storage_location
+-- is_enabled
+-- last_run_at
+-- last_status
+-- created_at
+-- updated_at
`

---

## 2. Key Design Decisions

### 2.1 Separation of Authentication and Academic Entities
- USERS table handles only authentication and authorization
- STUDENTS and TEACHERS are separate academic entities linked via user_id (nullable)
- Allows a user to have multiple roles (e.g., teacher who is also a student)
- Supports staff who don't need academic profiles

### 2.2 Enrolment History Preservation
- BATCH_STUDENTS maintains enrolment history with status transitions
- 	ransferred_from_batch_id tracks batch transfers
- Never delete enrolment records; use soft deletes and status changes

### 2.3 Immutable Test Attempts
- Each TEST_ATTEMPT is a new record; never update scores in place
- STUDENT_ANSWERS stores each answer separately
- SKILL_SCORES stores criteria-level breakdown for Writing/Speaking

### 2.4 Criteria-Level IELTS Assessment
- WRITING_EVALUATIONS stores 4 criteria: Task Achievement, Coherence & Cohesion, Lexical Resource, Grammatical Range & Accuracy
- SPEAKING_EVALUATIONS stores 4 criteria: Fluency & Coherence, Lexical Resource, Grammatical Range & Accuracy, Pronunciation
- Overall band calculated from criteria scores per IELTS rules

### 2.5 Financial Transaction Integrity
- PAYMENTS are separate from INVOICES and FEE_STRUCTURES
- Balance computed from payment transactions, not stored
- idempotency_key on payments for gateway callback safety
- RECEIPTS generated from payments, not invoices

### 2.6 Soft Deletes for Academic/Financial Records
- All academic and financial entities have deleted_at timestamp
- Queries filter WHERE deleted_at IS NULL by default
- Archive instead of delete for compliance

### 2.7 Audit Trail
- AUDIT_LOGS captures all administrative actions
- ATTENDANCE_CORRECTIONS tracks attendance changes with reason
- created_by/updated_by on all entities

---

## 3. Indexing Strategy

| Table | Key Indexes |
|-------|-------------|
| USERS | email (unique), phone (unique), status |
| STUDENTS | student_id (unique), user_id, status, batch_id (via BATCH_STUDENTS) |
| BATCHES | course_id, status, start_date, end_date |
| CLASS_SESSIONS | batch_id, session_date, teacher_id, status |
| ATTENDANCE_RECORDS | class_session_id, student_id, status |
| INVOICES | student_id, status, due_date |
| PAYMENTS | invoice_id, student_id, paid_at, idempotency_key (unique) |
| TEST_ATTEMPTS | test_id, student_id, status, started_at |
| STUDENT_ANSWERS | test_attempt_id, question_id |
| SUBMISSIONS | assignment_id, student_id, status |
| NOTIFICATIONS | user_id, is_read, created_at |
| AUDIT_LOGS | user_id, entity_type, entity_id, created_at |

---

## 4. Migration Strategy

- Use Flyway for versioned migrations
- Naming: V{version}__{description}.sql (e.g., V1.0.0__initial_schema.sql)
- Repeatable migrations for reference data: R__{description}.sql
- Run migrations on application startup
- CI validates migration syntax

---

*End of Database ER Diagram & Schema Design*

