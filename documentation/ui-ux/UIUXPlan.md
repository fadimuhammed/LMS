# UI/UX Screen-by-Screen Plan

## IELTS Centre Management and Learning Management System

Version: 1.0
Date: 2026-11-06
Status: Draft for Review

---

## 1. Design System Foundation

### 1.1 Colour Palette
- Primary: Deep Blue (#1E3A8A) - trust, education, professionalism
- Secondary: Teal (#0D9488) - growth, learning, balance
- Accent: Amber (#F59E0B) - highlights, warnings, achievements
- Success: Emerald (#10B981)
- Error: Red (#EF4444)
- Neutral: Slate scale (50-950) for text, borders, backgrounds
- Background: White (#FFFFFF) / Slate 50 (#F8FAFC)
- Surface: White with subtle shadows

### 1.2 Typography
- Font Family: Inter (system fallback: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto)
- Scale:
  - Display: 48px/56px, weight 700 (page titles)
  - H1: 36px/44px, weight 700 (section titles)
  - H2: 28px/36px, weight 600
  - H3: 22px/28px, weight 600
  - Body Large: 18px/28px, weight 400
  - Body: 16px/24px, weight 400
  - Body Small: 14px/20px, weight 400
  - Caption: 12px/16px, weight 400
  - Overline: 11px/16px, weight 500, uppercase

### 1.3 Spacing Scale (8px base)
4, 8, 12, 16, 20, 24, 32, 40, 48, 64, 80, 96px

### 1.4 Border Radius
- Small: 4px (inputs, buttons)
- Medium: 8px (cards, modals)
- Large: 12px (containers)
- Full: 9999px (pills, avatars)

### 1.5 Shadows
- Subtle: 0 1px 2px rgba(0,0,0,0.05)
- Card: 0 4px 6px -1px rgba(0,0,0,0.1)
- Elevated: 0 10px 15px -3px rgba(0,0,0,0.1)
- Modal: 0 25px 50px -12px rgba(0,0,0,0.25)

### 1.6 Breakpoints
- Mobile: < 640px
- Tablet: 640px - 1023px
- Desktop: 1024px - 1279px
- Wide: >= 1280px

---

## 2. Navigation Architecture

### 2.1 Admin Navigation (Sidebar - Collapsible)
Dashboard
  Overview
  Statistics
  Quick Actions
Students
  All Students
  Admissions and Enquiries
  Student Import
Courses
  Course Catalogue
  Batches
  Batch Transfers
Timetable
  Weekly Calendar
  Class Sessions
  Classrooms
  Teacher Availability
Attendance
  Mark Attendance
  Attendance Reports
  Leave Requests
  Corrections
IELTS Learning
  Materials Library
  Assignments
  Question Bank
  Mock Tests
Assessment
  Writing Evaluations
  Speaking Evaluations
  Band Scores
Fees and Payments
  Fee Structures
  Invoices
  Payments
  Receipts
  Outstanding
  Discounts and Refunds
Teachers
  Teacher Directory
  Assignments
  Workload
Reports
  Admissions
  Attendance
  Fees
  Performance
  Teacher Activity
  Custom Reports
Notifications
  Announcements
  Templates
  Message Centre
Settings
  Centre Configuration
  Academic Settings
  User Management
  Roles and Permissions
  Notification Settings
  Backup and Restore
  Audit Logs

### 2.2 Teacher Navigation (Sidebar)
Dashboard
  My Classes Today
  Pending Evaluations
  Upcoming Tests
  Student Alerts
My Batches
  Batch Details
  Students
  Timetable
Attendance
  Mark Attendance
  My Attendance
Learning
  My Materials
  Create Assignment
  Assignments to Evaluate
  Question Bank
Tests
  Scheduled Tests
  Test Results
Evaluation
  Writing Submissions
  Speaking Submissions
  Band Scores
Reports
  Batch Performance
  Student Progress
  My Workload

### 2.3 Student Navigation (Bottom Nav on Mobile, Sidebar on Desktop)
Dashboard
  My Progress
  Next Class
  Pending Tasks
  Latest Results
My Course
  Course Details
  Batch Info
  Teachers
Study Materials
  All Materials
  By Skill
  Downloads
Practice
  Listening
  Reading
  Writing
  Speaking
Mock Tests
  Available Tests
  My Results
Assignments
  To Do
  Submitted
  Evaluated
Results
  Band Scores
  Progress Charts
  Weakness Analysis
  Teacher Feedback
Attendance
  My Attendance
  Leave Requests
Timetable
  Weekly View
  Upcoming Classes
Payments
  Invoices
  Payment History
  Receipts
Profile
  Personal Info
  IELTS Profile
  Documents
  Notifications
  Settings

### 2.4 Receptionist Navigation (Restricted Sidebar)
Dashboard
  Today's Admissions
  Pending Payments
  Quick Actions
Admissions
  Enquiries
  New Student
  Student Search
Fees
  Record Payment
  Outstanding
  Receipts
Students
  Student List
  Student Profile (read-only)
Reports
  Daily Collection
  Admissions Report
  Outstanding Report

---

## 3. Screen-by-Screen Specification

### 3.1 Authentication Screens

| Screen | Key Elements | States |
|--------|--------------|--------|
| Login | Email/phone input, password, remember me, forgot password link, role indicator | Empty, Error, Loading, Success |
| Forgot Password | Email/phone input, send OTP button | Empty, Sent, Error |
| Reset Password | OTP input, new password, confirm password | Empty, Validating, Success, Error |
| First Login / Setup | Set password, update profile, select preferences | Incomplete, Complete |

### 3.2 Admin Screens

#### 3.2.1 Admin Dashboard
- Header: Centre name, date, notifications bell, profile menu
- Stats Cards (4-column grid): Active Students, Active Batches, Today's Classes, Pending Fees
- Charts: Admissions trend (30 days), Fee collection (monthly), Attendance % (weekly)
- Tables: Recent Admissions (5), Upcoming Mock Tests (5), Fee Defaulters (5)
- Quick Actions: New Student, New Batch, Record Payment, Send Announcement
- Responsive: Stack cards on mobile; charts become swipeable

#### 3.2.2 Student List
- Toolbar: Search (name, ID, phone, email), Filter (status, course, batch), Export, Add Student
- Table: Student ID, Name, Phone, Course, Batch, Status, Attendance %, Outstanding, Actions
- Row Actions: View, Edit, Documents, Timeline, Transfer, Deactivate
- Bulk Actions: Export, Status Change, Batch Transfer, Send Notification
- Empty State: Illustration + No students found + Add Student CTA

#### 3.2.3 Student Profile / Detail
- Tabs: Overview, Academic, Documents, Timeline, Fees, Attendance, Progress, Communications
- Overview: Profile card (photo, ID, contact, status), Quick stats, Emergency contact
- Academic: IELTS profile, Course/Batch history, Placement test results
- Documents: Grid view with preview, download, delete; Upload new
- Timeline: Chronological events (enquiry, enrolment, payment, test, evaluation, etc.)
- Fees: Invoice list, Payment history, Outstanding balance, Record Payment button
- Attendance: Calendar heatmap, Percentage by month, Leave requests

#### 3.2.4 Admission / Enquiry Management
- Kanban Board: New -> Contacted -> Assessed -> Converted / Lost
- Enquiry Card: Name, Phone, Source, Assigned to, Follow-up date, Notes preview
- Detail Modal: Full form, Communication log, Convert to Student action
- New Enquiry Form: Multi-step (Contact -> Course Interest -> Assessment -> Documents)

#### 3.2.5 Course and Batch Management
- Course List: Card grid with code, name, type, duration, fee, status, batch count
- Course Form: Basic info, Fees breakdown, Syllabus (rich text), Objectives
- Batch List: Table with name, course, dates, capacity (enrolled/total), status, teachers
- Batch Detail: Students tab (with add/remove/transfer), Teachers tab, Timetable tab, Attendance tab

#### 3.2.6 Timetable / Calendar
- Weekly View: Mon-Sun columns, time slots rows, colour-coded by batch/skill
- Drag and Drop: Reschedule classes, assign teachers/classrooms
- Filters: Batch, Teacher, Classroom, Skill, Date range
- Class Session Modal: Batch, Teacher, Classroom, Date, Time, Skill, Topic, Status
- Conflict Detection: Highlight overlapping bookings

#### 3.2.7 Attendance Management
- Mark Attendance: Batch selector, Date picker, Student grid with 4-status buttons (P/A/L/E)
- Bulk Actions: Mark All Present, Mark All Absent, Apply to Selected
- Leave Requests: List with approve/reject, Student self-service view
- Reports: Batch-wise, Student-wise, Date range, Export Excel/PDF

#### 3.2.8 Fees and Payments
- Fee Structures: Course-linked, component breakdown, effective dates
- Invoices: List with status badges, Generate for batch/student, Send reminder
- Record Payment: Modal with student search, invoice selection, mode, amount, reference
- Receipts: Auto-generated PDF, Reprint, Email/WhatsApp share
- Outstanding Report: Ageing buckets (Current, 30d, 60d, 90d+), Student drill-down

#### 3.2.9 IELTS Learning - Materials Library
- Grid/List Toggle: Cards with thumbnail, title, skill badge, topic, file type
- Filters: Skill, Course, Batch, Level, Tags
- Upload Modal: Drag-drop, Multiple files, Metadata form (skill, topic, level, batch access)
- Access Control: Batch-restricted toggle, Student-specific assignments

#### 3.2.10 Question Bank
- List: Question text preview, Skill, Type, Difficulty, Type, Actions
- Create/Edit Form: Skill, Type, Difficulty, IELTS Type, Rich text editor, Options (for MCQ), Passage/Audio attach, Explanation
- Bulk Import: CSV/Excel template

#### 3.2.11 Mock Test Creation
- Test Builder: Sections (add/remove/reorder), Questions per section (drag from bank), Settings (time, randomization, attempts)
- Preview Mode: Simulate student experience

#### 3.2.12 Writing Evaluation
- Queue: Student, Task, Submitted date, Word count, Status
- Evaluation Screen: Split view - Student response (left, annotated) / Rubric (right)
- Annotation Tools: Highlight, Comment, Strikethrough, Insert suggestion
- Rubric: 4 criteria sliders (0-9, 0.5 steps), Overall auto-calculated, Feedback text area
- Save and Next: Keyboard shortcuts for efficiency

#### 3.2.13 Speaking Evaluation
- Queue: Student, Part, Duration, Submitted date
- Evaluation Screen: Audio player (waveform, speed), 4 criteria sliders, Feedback (text + optional audio)
- Cue Card Reference: Shows prompt during Part 2 evaluation

#### 3.2.14 Band Scores and Progress
- Student Search: Select student -> View all band scores
- Progress Charts: Line chart (band over time), Radar chart (skill comparison), Bar chart (mock test history)
- Target Setting: Input target band, Show gap analysis
- Export: PDF progress report

#### 3.2.15 Reports
- Report Builder: Select report type, Parameters (date range, batch, student), Preview, Export (PDF/Excel/CSV)
- Scheduled Reports: List, Edit schedule, View history

#### 3.2.16 Certificate Management
- Template Editor: Drag-drop or HTML/CSS, Variable placeholders ({{student_name}}, {{course}}, etc.)
- Generate: Batch generate for completed course, Individual generate
- Verification: Public lookup page by certificate number/QR code

#### 3.2.17 Settings
- Centre Config: Name, Logo, Address, Contact, GSTIN, Timezone
- Academic: Grading scale, Attendance threshold, Band rounding rules
- Users: List, Invite, Roles, Permissions matrix editor
- Notifications: Template editor, Channel config (Email/SMS/WhatsApp)
- Backup: Schedule, Retention, Manual trigger, Restore history

---

### 3.3 Teacher Screens

#### 3.3.1 Teacher Dashboard
- Today's Classes: Cards with time, batch, skill, classroom, student count, Mark Attendance action
- Pending Evaluations: Writing (count), Speaking (count), Assignments (count) - click to evaluate
- Upcoming Tests: Mock tests for assigned batches
- Student Alerts: Low attendance, Missing submissions, Fee overdue (read-only)

#### 3.3.2 My Batches
- Batch Cards: Name, Course, Schedule summary, Student count, Progress snapshot
- Batch Detail: Students (with attendance %, last test band), Timetable, Materials, Assignments

#### 3.3.3 Attendance (Teacher)
- Mark Attendance: Same as admin but only assigned batches
- My Attendance: Calendar view of teacher's own class attendance

#### 3.3.4 Materials (Teacher)
- My Uploads: List with edit/delete, Upload new
- Batch Materials: View all materials assigned to my batches

#### 3.3.5 Assignments (Teacher)
- Create Assignment: Form with skill, batch/individual, deadline, max marks, instructions
- To Evaluate: List with submission count, avg score, overdue highlight
- Evaluation View: Submission list -> Individual evaluation (marks, feedback, annotations)

#### 3.3.6 Question Bank (Teacher)
- My Questions: Filter by skill/type, Create new, Edit own
- Shared Bank: View all (read-only for reference)

#### 3.3.7 Test Results (Teacher)
- Batch Results: Test selector, Student grid with scores, bands, completion status
- Student Detail: Answers review, Time analysis, Weakness identification

#### 3.3.8 Evaluations
- Writing Queue: Same as admin but only assigned students
- Speaking Queue: Same as admin but only assigned students

#### 3.3.9 Reports (Teacher)
- Batch Performance: Attendance, Assignment completion, Test averages, Band progression
- Student Progress: Individual reports for parent/guardian meetings

---

### 3.4 Student Screens

#### 3.4.1 Student Dashboard
- Progress Header: Overall estimated band (large), Target band, Gap indicator
- Skill Radar Chart: Current vs Target for L/R/W/S
- Next Class: Time, Skill, Teacher, Classroom, Join/Remind button
- Pending Tasks: Assignments due, Tests scheduled, Materials to review (max 3 each)
- Latest Mock Result: Band scores, Date, View Details
- Quick Actions: Practice Listening, Practice Reading, Write Essay, Record Speaking

#### 3.4.2 My Course
- Course Info: Name, Type, Duration, Progress %, Teachers
- Batch Info: Name, Schedule, Classmates (names only), Classroom
- Syllabus: Week-by-week topics with completion checkboxes

#### 3.4.3 Study Materials
- Filter Tabs: All, Listening, Reading, Writing, Speaking, General
- Material Cards: Title, Skill badge, Topic, File type icon, Size, Download/Preview
- Progress: Checkmark when viewed/downloaded

#### 3.4.4 IELTS Practice - Listening
- Section Selector: Section 1, 2, 3, 4 tabs
- Audio Player: Full-width, Play/Pause, Seek, Speed (0.5x-2x), Volume
- Questions: Sync with audio position, Auto-advance on answer
- Question Types: MCQ (radio), Matching (drag-drop), Map Label (click), Form Completion (inputs)
- Timer: Practice (pause allowed) / Mock (no pause)
- Submit: Confirmation modal, Then show transcript + explanations + score

#### 3.4.5 IELTS Practice - Reading
- Split View: Passage (left, scrollable) / Questions (right, sticky)
- Timer: 60 min, Warning at 10/5/1 min
- Navigation: Question numbers (Answered/Flagged/Unanswered), Jump to
- Question Types: All 8 types with appropriate inputs
- Review Mode: Show correct/incorrect, Explanations, Time per passage

#### 3.4.6 IELTS Practice - Writing
- Task Selector: Task 1 (Academic/General), Task 2
- Editor: Rich text (Bold, Italic, Underline, Lists, Undo/Redo), Live word count, Min/Max indicators
- Prompt: Displayed above editor, Collapsible
- Document Upload: Alternative to typing (PDF/Word)
- Submit: Deadline countdown, Confirm, Submission receipt
- My Submissions: List with status, Marks, Feedback, Resubmit button
- Feedback View: Annotated essay, Criteria scores, Teacher comments

#### 3.4.7 IELTS Practice - Speaking
- Part Selector: Part 1, Part 2, Part 3 tabs
- Part 2 Cue Card: Shows topic + prompts, 1 min prep timer, 2 min speak timer
- Recorder: Large mic button, Waveform visualization, Pause/Resume, Re-record
- Playback: Review before submit
- Mock Interview: Scheduled session with teacher, Video/audio call integration (future)
- My Recordings: List with status, Band scores, Teacher feedback (audio/text)

#### 3.4.8 Mock Tests
- Available Tests: Cards with title, type, duration, attempts left, Start button
- Test Interface: Full-screen, Timer (prominent), Navigation, Flag questions, Auto-save
- Submission: Confirm modal, Then results (instant for L/R, pending for W/S)
- History: List with date, type, scores, bands, Review link

#### 3.4.9 Results and Progress
- Band Score Card: Large overall band, 4 skill cards with bands
- Progress Charts: Toggle Line/Radar/Bar, Date range selector
- Target Tracker: Current vs Target, Gap per skill
- Weakness Analysis: Lowest accuracy question types, Recommended practice
- Teacher Feedback: Consolidated view from all evaluations

#### 3.4.10 Attendance
- Calendar Heatmap: Monthly view, Colour-coded (Present/Late/Absent/Excused)
- Percentage: Overall, By month, By skill
- Leave Request: Form with reason, Date/class selection, Status tracking

#### 3.4.11 Timetable
- Week View: Mon-Sun, Time slots, Class cards (Skill, Teacher, Classroom)
- Today Highlight: Current/next class pulsing
- Sync: Add to Google Calendar / Apple Calendar

#### 3.4.12 Payments
- Invoices: List with status, Amount, Due date, Pay Now button
- Payment History: Date, Amount, Mode, Reference, Receipt download
- Pay Now: Opens payment gateway (UPI/Card/NetBanking) or shows bank details for transfer

#### 3.4.13 Profile and Settings
- Personal Info: Editable fields, Avatar upload
- IELTS Profile: Target band, Test date, Type, Previous scores
- Documents: View uploaded, Upload new
- Notifications: In-app list, Mark all read, Preferences
- Settings: Theme (Light/Dark/System), Language, Password change, Logout all devices

---

### 3.5 Receptionist Screens

#### 3.5.1 Reception Dashboard
- Today's Stats: New enquiries, Walk-ins, Payments collected, Pending receipts
- Quick Actions: New Enquiry, Register Student, Record Payment, Print Receipt

#### 3.5.2 Enquiry Management
- List/Board: Same as admin but limited fields
- New Enquiry: Simplified form (Contact, Course Interest, Source)
- Follow-up: Call log, Notes, Schedule callback

#### 3.5.3 Student Registration
- Multi-step Form: Personal -> Contact -> Academic -> IELTS Profile -> Documents -> Course Selection -> Batch Allocation -> Fee Payment
- Auto-generate: Student ID, Username/Password
- Print: Admission slip with credentials

#### 3.5.4 Fee Collection
- Student Search: By ID, name, phone
- Outstanding View: Invoices with balances, Ageing
- Record Payment: Mode selector (Cash/UPI/Bank/Card), Amount, Reference, Print Receipt
- Daily Collection Report: Summary by mode, Detailed list, Export

#### 3.5.5 Reports
- Daily Collection: Date selector, Mode breakdown, Shift-wise (if applicable)
- Admissions: Date range, Course-wise, Status-wise
- Outstanding: Ageing report, Student contact list for follow-up

---

## 4. Component Library (Shared)

### 4.1 Core Components
- Button: Primary, Secondary, Outline, Ghost, Danger, Loading, Disabled, Icon-only, Group
- Input: Text, Email, Phone, Password, Number, Textarea, Select, Multi-select, Date, DateRange, File
- Form: Field wrapper (label, error, hint), Validation (Zod), FieldArray for dynamic fields
- Table: Sortable, Filterable, Paginated, Row selection, Expandable rows, Sticky header, Responsive (card on mobile)
- Card: Header, Body, Footer, Hoverable, Clickable, Loading skeleton
- Modal: Sizes (sm, md, lg, xl, full), Confirmation, Form, Scrollable, Close on overlay/ESC
- Dropdown/Menu: Trigger, Items, Dividers, Icons, Shortcuts, Searchable
- Tabs: Horizontal, Vertical, Animated indicator, Keyboard nav
- Tooltip/Popover: Delay, Position, Interactive content
- Toast/Notification: Success, Error, Warning, Info, Actionable, Persistent
- Badge: Status (dot + text), Count, Colored variants
- Avatar: Image, Initials, Fallback, Sizes, Group (overlapping)
- Breadcrumb: Separator, Current page, Collapse on mobile
- Pagination: Page numbers, Prev/Next, Page size selector, Total count
- Empty State: Illustration, Title, Description, Primary/Secondary actions
- Loading: Skeleton (text, card, table), Spinner, Progress bar, Progress steps
- Divider: Horizontal, Vertical, With text

### 4.2 Data Display
- Stat Card: Value, Label, Trend (up/down/neutral), Icon, Sparkline
- Chart Containers: Line, Bar, Radar, Pie, Area - Responsive, Tooltips, Legends
- Timeline: Vertical, Alternating, Icons per event type, Expandable details
- Calendar: Month view, Event dots, Click for details, Heatmap variant
- Progress: Linear (with label), Circular, Steps, Multi-segment
- Data Grid: Inline edit, Bulk actions, Column visibility, Density toggle

### 4.3 IELTS-Specific Components
- Audio Player: Waveform, Speed, Volume, Keyboard shortcuts, Section markers
- Reading Panel: Split view, Sync scroll, Highlight text, Question navigator
- Writing Editor: Word count, Min/Max, Auto-save, Version history, Annotations overlay
- Speaking Recorder: Waveform, Timer, Pause/Resume, Re-record, Playback, Visual feedback
- Test Interface: Full-screen, Timer (prominent, colour changes), Question navigator (grid), Flag, Auto-save indicator, Submit confirmation
- Band Score Display: Large number, Skill breakdown, Radar chart, Target comparison
- Rubric Evaluator: 4-criterion sliders (0-9, 0.5), Auto-overall, Feedback tabs
- Annotation Toolbar: Highlight colors, Comment, Strikethrough, Suggestion, Delete

### 4.4 Layout Components
- Sidebar: Collapsible, Responsive (drawer on mobile), Active route highlight, Badge counts
- Header: Logo, Search (global), Notifications, User menu, Theme toggle, Breadcrumb
- Footer: Links, Version, Copyright
- Page Container: Max-width, Padding, Background
- Section: Heading, Description, Actions, Divider

---

## 5. Responsive Behaviour

| Component | Mobile (<640px) | Tablet (640-1023px) | Desktop (>=1024px) |
|-----------|-----------------|---------------------|-------------------|
| Sidebar | Bottom drawer | Collapsible rail | Full sidebar |
| Navigation | Bottom nav bar (5 items) | Sidebar + top tabs | Full sidebar |
| Tables | Card stack | Horizontal scroll | Full table |
| Forms | Single column | Two column | Multi-column |
| Charts | Full width, touch pan | Full width | Sized containers |
| Test Interface | Full screen, no chrome | Full screen | Full screen / split |
| Modals | Full screen | Centered (max-w-lg) | Centered (max-w-2xl) |
| Cards | Single column | 2-column grid | 3-4 column grid |
| Stats | 2x2 grid | 4-column | 4-column |
| Editor | Full width | Split view | Split view |

---

## 6. Accessibility (WCAG 2.1 AA)

### 6.1 Keyboard Navigation
- All interactive elements reachable via Tab
- Focus visible (ring-2 ring-primary ring-offset-2)
- Skip to main content link
- Logical focus order
- Modal focus trap
- Arrow keys for menus, tabs, sliders

### 6.2 Screen Readers
- Semantic HTML (header, nav, main, section, article, aside, footer)
- ARIA labels for icon-only buttons
- Live regions for toasts, loading, auto-save
- Heading hierarchy (h1-h6, no skipping)
- Table headers with scope
- Form labels associated with inputs
- Error messages linked via aria-describedby

### 6.3 Colour and Contrast
- Text: 4.5:1 minimum (3:1 for large text)
- UI components: 3:1 minimum
- Not colour-only for status (icons + text)
- Focus indicators visible in all themes

### 6.4 Motion and Animation
- Respect prefers-reduced-motion
- No auto-playing audio/video
- Animation duration < 200ms for UI transitions

---

## 7. Empty and Error States

| State | Pattern |
|-------|---------|
| No Data | Illustration + Clear title + Helpful description + Primary CTA |
| No Results (Search/Filter) | Illustration + No matches found + Clear filters action |
| Network Error | Friendly message + Retry button + Offline indicator |
| Server Error | Something went wrong + Error ID + Support contact + Refresh |
| Permission Denied | Lock icon + Access restricted + Contact admin |
| Session Expired | Modal with Session expired + Login button (preserve form data) |
| Validation Error | Inline field errors + Summary at top + Focus first error |

---

## 8. Design Tokens (Tailwind Config Extension)

`js
// tailwind.config.js additions
theme: {
  extend: {
    colors: {
      primary: { 50: '#EFF6FF', ..., 900: '#1E3A8A', 950: '#172554' },
      secondary: { 50: '#F0FDFA', ..., 900: '#134E4A' },
      accent: { 50: '#FFFBEB', ..., 900: '#78350F' },
      slate: { ... }, // default
    },
    fontFamily: {
      sans: ['Inter', 'system-ui', 'sans-serif'],
    },
    spacing: {
      '18': '4.5rem', // 72px
      '88': '22rem', // 352px
    },
    boxShadow: {
      'card': '0 4px 6px -1px rgba(0,0,0,0.1)',
      'elevated': '0 10px 15px -3px rgba(0,0,0,0.1)',
      'modal': '0 25px 50px -12px rgba(0,0,0,0.25)',
    },
    borderRadius: {
      '4xl': '2rem', // 32px
    },
    transitionDuration: {
      '0': '0ms',
      '2000': '2000ms',
    },
  },
}
`

---

## 9. User Journey Maps (Key Flows)

### 9.1 Student: First Login -> First Practice Test
1. Login -> Dashboard (onboarding tooltip) -> Practice tab -> Listening -> Section 1 -> Audio play -> Answer -> Submit -> Results -> Transcript -> Back to Dashboard (progress updated)

### 9.2 Teacher: Evaluate Writing Batch
1. Dashboard -> Pending Evaluations (Writing: 12) -> Student A -> Annotate essay -> Rubric score -> Feedback -> Save and Next -> Student B -> ... -> Complete -> Dashboard updated

### 9.3 Admin: New Admission -> Active Student
1. Enquiries -> New Enquiry -> Fill form -> Save -> Kanban: Contacted -> Schedule Assessment -> Assessment done -> Convert -> Course Select -> Batch Allocate -> Fee Invoice -> Payment -> Active -> Welcome email

### 9.4 Receptionist: Walk-in Payment
1. Dashboard -> Record Payment -> Search Student -> Select Invoice -> Cash 5000 -> Reference: Cash-001 -> Print Receipt -> Hand to student -> Daily report updated

---

## 10. Design Handoff Checklist

- [ ] Figma file with all screens (Admin, Teacher, Student, Receptionist)
- [ ] Component library documented with variants
- [ ] Design tokens exported (JSON for Tailwind)
- [ ] Icon set selected (Lucide / Heroicons)
- [ ] Illustrations for empty states (undraw / custom)
- [ ] Responsive breakpoints verified
- [ ] Accessibility audit completed
- [ ] Dark mode variants for all screens
- [ ] Loading/skeleton states for all async components
- [ ] Error states for all forms and data fetching
- [ ] Animation specifications (duration, easing)
- [ ] Print stylesheets for receipts, certificates, reports

---

End of UI/UX Screen-by-Screen Plan
