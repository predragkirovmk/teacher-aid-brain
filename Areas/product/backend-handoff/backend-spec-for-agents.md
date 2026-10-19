---
created: 2026-10-18
updated: 2026-10-18
type: area
version: 1.0
status: current — backend handoff for AI coding agents
audience: AI coding agents (Claude Code, Codex, Cursor and similar) working on the TeacherAid backend
companion: backend-guide-for-developer.md (the same content, simplified, for the human developer)
---

# TeacherAid backend — specification for coding agents

Version 1.0 · updated 2026-10-18 · scope: the whole of TeacherAid v1

## 0. Read this first

### 0.1 What this file is

The complete brief for building the TeacherAid API and backend. It is written for an AI coding
agent that has never seen the project. Load it into context before writing backend code, and
re-read the relevant section before each task. It is self-contained: nothing outside this file
is needed to start.

Put it in the backend repository (for example `docs/teacheraid-backend-spec.md`) and point the
repository's `AGENTS.md` or `CLAUDE.md` at it.

### 0.2 Sources and precedence

This file is compiled from, in order of authority:

1. The product owner's answers of 2026-10-18 (scope, demo versus spec, stack).
2. The pilot decisions of 2026-10-15 (student identity, web focus flag).
3. `feature-spec-v1` — the build specification of record, confirmed 2026-10-13.

When this file and anything else disagree, this file wins. These are **not** sources of truth
and must not be used to infer requirements:

- The mock data in the student web demo (`teacher-aid-app`, route `/app`): invented classmates,
  points, subjects (maths, Macedonian, English), class "7B", the timetable.
- The Startup Weekend opener prototype API (`/api/join`, `/api/answer`, `/api/state`,
  `/api/advance` in the same repository). It is a nickname-based Redis demo with a shared host
  key. Reuse ideas, not its contract or its auth.
- Older product notes written for the September pitch (features, product overview, user flows,
  technical architecture, data and privacy, the handbook). They are partly superseded.

### 0.3 Status labels — obey them

Every value and rule carries one of these labels:

| Label | Meaning | What the agent does |
|---|---|---|
| **DECIDED** | Signed off by the product owner | Implement exactly |
| **PROPOSED** | A starting value nobody has signed off | Implement as a named config constant, never hard-coded inline, so it can change without a migration |
| **OPEN** | Not decided | Do not guess. Implement the surrounding structure, leave the decision behind a clearly named TODO, and ask the human developer, who asks the product owner. Section 14 lists every OPEN item |
| **INTERPRETATION** | The spec is silent or ambiguous and this file chose a reading | Implement, but mention it in the pull request so a human can confirm |

Keywords: **MUST** is required for v1. **SHOULD** is expected unless there is a stated reason.
**MAY** is optional. **MUST NOT** is forbidden.

### 0.4 Hard rules for agents

1. Never invent a number, a rule or a field the product needs. Use a PROPOSED constant or stop
   at an OPEN item.
2. Never send a student's name, email or any roster data to the AI provider (BR-SEC-04).
3. Never expose to a student another student's answers, reasons or results, or who sent an
   anonymous question (section 2).
4. Never expose individual student answers, reasons, anonymous questions or per-topic results
   to the school admin role.
5. The server is the timer authority for every countdown. Clients only render it.
6. Never auto-end a live session. Only the teacher ends it.
7. Never mark a student absent because of a phone, battery, data or focus-lock problem.
8. Never build what section 1.6 lists as out of scope, even if a frontend screen seems to need
   it. Raise it instead.
9. Keep IDs stable. Requirement IDs (FR-, BR-, EC-, AC-, US-, F-, OPEN-) are referenced from
   tests, commits and pull requests. Do not renumber; retire an ID instead of reusing it.

### 0.5 Change control

This file changes whenever a product decision changes. The header shows the version and date;
section 15 is the changelog. Before starting a task, check you hold the latest version. If the
code and this file disagree, raise it; do not silently follow either.

---

## 1. Product and business context

### 1.1 What TeacherAid is

An AI classroom system for secondary schools in North Macedonia. A teacher prepares a lesson
with AI in minutes from the national e-textbook (е-учебник), then runs it live: students join
by scanning a QR code on their own phones, answer an opening prediction or debate question,
get pop-up questions the teacher fires at unpredictable moments, and send questions the class
cannot see. At the end the teacher gets the absentee list and the most-missed questions; the
headmaster gets a monthly report by email.

The unit of the product is the **lesson period: 45 minutes**. Everything is designed around
that clock.

Born at Startup Weekend Bitola, 11 to 13 September 2026. Working name TeacherAid, one word.

### 1.2 Market and pilot

- **Schools (DECIDED):** secondary schools in North Macedonia — гимназија (general), стручно
  (vocational) and уметничко (arts).
- **Pilot subjects (DECIDED):** history, electronics, physics. These have their curriculum and
  e-textbooks loaded. Any other subject works from the teacher's own typed input, with no
  curriculum grounding.
- **Theory periods only.** Practical and lab periods are out.
- **Interface language (DECIDED in the spec):** Macedonian. The student web demo also has an
  English UI; whether v1 ships English is OPEN-15. All generated lesson content is Macedonian.

### 1.3 Who pays and who uses

- The **school** is the customer. Pricing is being reconsidered and billing is out of scope:
  build no payments, plans, seats or invoices.
- **Teachers** and **students** use the product daily.
- The **school admin** (secretary or IT person) sets up classes, rosters and staff.
- The **headmaster** has no account and receives a monthly PDF report by email.

### 1.4 The 45-minute lesson

| Minute | What happens |
|---|---|
| 0 | Teacher projects the class view: QR plus short code. Students scan and join. Joining is attendance. |
| 1–5 | Opener: a prediction (default) or debate question. Students vote, then type a one-line reason. Live bars, then the reveal. |
| 5–40 | Teaching. The teacher fires pop-up questions with one tap; each lands on every phone with a server-run timer. Students may send anonymous questions at any time. |
| 40–45 | Review: most-missed questions, worst first, and the anonymous questions, answered aloud by the teacher. |
| End | The teacher presses End: points settle, attendance is written, the absentee list appears, the lesson is archived. |
| After | Optional practice set from the session's missed questions. Dashboards update. Monthly headmaster report. |

### 1.5 Product principles that become backend rules

- **Being wrong with a reason scores.** A reasoned wrong answer must score close to a silent
  right one (productive failure, the pretesting and hypercorrection effects). Points are fixed
  and identical in every school; teachers cannot change them, except turning off the speed bonus.
- **Nothing unreviewed reaches students.** All AI output is reviewed by the teacher before it is
  saved. No student-facing AI, no AI re-explanations in the review.
- **The teacher decides when phones are used.** The live session is the sanctioned use.
- **Anonymous means anonymous to classmates, not to the teacher.** The teacher sees the sender;
  this is what makes the channel safe.
- **The buyer never logs in.** The headmaster gets a forwardable report, not a dashboard.
- **TeacherAid does not replace e-Дневник** (the official electronic register). It never writes
  to it. It gives the teacher the absentee names to copy.

### 1.6 Out of scope for v1 (DECIDED) — do not build

Universities · practical or lab periods · national leaderboard · streaks · open-ended teacher
chatbot (replaced by fixed refinements) · e-Дневник sync · Albanian-language interface ·
timetables, periods or schedules · parent app or parent login · per-student extra time ·
self-serve data export or deletion (support does it manually) · reassigning a class to another
teacher · a second teacher on one class (no co-teaching, no substitutes) · in-app billing ·
question types beyond the four in BR-Q-01 (no ordering, matching, tap-the-map, drawing) ·
cross-school or public lesson sharing · pairing a phoneless student with a classmate's device ·
a teacher entering answers on a student's behalf.

Owner decision 2026-10-18: the student demo's timetable, next-lesson card, scheduled pop quiz
and streak badge get **no API support**. Those demo screens will be removed or reworked when the
frontend is wired to the real API.

### 1.7 Clients the backend serves

| Client | Form | Status on 2026-10-18 |
|---|---|---|
| Student app | Mobile-first web app (PWA), Next.js, installable, works offline | Built as a clickable demo on mock data at `/app`; wired to nothing |
| Teacher app | Web app: lesson authoring, dashboard, and the **console** used during a lesson | Not built |
| Class view | A second browser window the teacher projects | Not built |
| School admin | Web screens | Not built |
| Headmaster | Monthly email with a PDF and a permanent private link | Not built |
| Native student app | Needed later for the mandatory OS-level focus lock | Not active. The pilot uses the web app with a focus flag (BR-FOC-01) |

The backend MUST be client-agnostic: the same API serves the web student app now and a native
app later.

### 1.8 Stack

**Hard requirements (DECIDED):**

- EU hosting for all personal data, including backups and logs.
- A relational, Postgres-shaped data model.
- A realtime channel for live sessions (server-to-client push; polling is not enough for the
  timer and the console).
- The server is the authority for every timer.
- Clients can queue answers offline and sync later; the server accepts them (BR-NET-01).
- Encryption in transit and at rest.

**Recommendation (owner, 2026-10-18; the final choice is the backend developer's):**

- Frontend: Next.js with TypeScript (already used by the student app).
- Backend API: Laravel.
- Database: PostgreSQL.
- Realtime: a managed realtime layer. In Laravel this is broadcasting through Laravel Reverb
  (self-hosted WebSocket server) or a managed service such as Pusher or Ably.
- AI: the September architecture note recommended Anthropic Claude for Macedonian lesson
  generation, with prompt caching of the system prompt and curriculum excerpt. Not re-confirmed;
  see OPEN-17.

### 1.9 Glossary

| Term | Meaning |
|---|---|
| гимназија | General secondary school (ages 15–18), years I to IV |
| стручно училиште | Vocational or technical secondary school |
| е-учебник (e-textbook) | National electronic textbook, published under МОН |
| e-Дневник | The national electronic school register (official attendance and grades) |
| МОН | Ministry of Education and Science |
| БРО | Bureau for the Development of Education (curricula) |
| ЗЗЛП | Law on Personal Data Protection (2020), GDPR-aligned |
| ДПА | Data processing agreement (school = controller, TeacherAid = processor) |
| ДПИ | State Education Inspectorate |
| Class (одделение) | A group of students, for example "II-3" (year II, group 3) |
| Course | INTERPRETATION: one teacher teaching one subject to one class. See section 3 |
| Lesson | A reusable authored object: opener, pop-up questions, timers, review summary |
| Session | One live run of a lesson for one course |
| Class view | The projected window: QR, question, bars, reveal, leaderboard. No private data |
| Console | The teacher's own screen during a session. Holds named data. Never projected |
| Pop-up | A question the teacher fires during teaching |
| Focus flag | A marker on the console that a student left the app during a question |

---

## 2. Actors and permissions

### 2.1 Actors

| Actor | Account | Created by |
|---|---|---|
| Student | School-issued email account (DECIDED). Login method OPEN-01 | Roster import or entry by the school admin |
| Teacher | Email and password (DECIDED). No magic links, no SSO | Invited by the school admin |
| School admin | Email and password (INTERPRETATION: same method as teachers) | TeacherAid staff at onboarding |
| Headmaster | No account. An email address that receives the report | Set at onboarding (INTERPRETATION) |
| TeacherAid staff | Internal account | TeacherAid |
| System | Scheduled jobs: monthly report, retention, token rotation | — |
| AI provider | External, receives no personal data | — |

### 2.2 Permission matrix

`own` = only their own records. `school` = their school only. `—` = no access.

| Data or action | Student | Teacher | School admin | Staff |
|---|---|---|---|---|
| Join a session they are enrolled in | yes | — | — | — |
| Their own answers, points, rank, badges, history, practice | own | — | — | grant |
| Another student's answers, reasons, results | — | own courses | — | grant |
| Who sent an anonymous question | — | own sessions | — | grant |
| One-line reasons | without names, on the class view | with names | — | grant |
| Per-topic correctness | own | own courses | — | grant |
| Focus events and authorization state | — | own courses | — | grant |
| Lessons | — | own; school library read and copy | — | grant |
| Create, edit, run lessons and sessions | — | own | — | — |
| Classes, rosters, moving students | — | read own | school | grant |
| Invite and remove teachers | — | — | school | yes |
| Distraction list | — | — | school | yes |
| Usage overview (who teaches, how often) | — | — | school | yes |
| Resend the headmaster report | — | — | school | yes |
| Staff-access log | — | — | school | yes |
| Badges of classmates | yes | own courses | — | grant |
| Class leaderboard | own class | own courses | — | grant |

`grant` = only through a time-limited, audited support grant visible to the school admin
(BR-SEC-03). Staff have no standing access to school data.

A teacher MUST NOT see another teacher's classes, students or results, or anything school-wide
except the school library. A teacher MUST NOT change rosters.

---

## 3. Domain model

INTERPRETATION, to confirm (OPEN-02): a Macedonian class (II-3) has a different teacher per
subject, and the spec says a teacher sees "their own classes". This file models the teaching
unit as a **course** = one class + one subject + one teacher. Rosters belong to the class; a
course inherits the class roster.

### 3.1 Entities

Field lists are minimum fields, not final schemas. Every table has an id, created and updated
timestamps, and a `school_id` where it holds school data (tenant isolation, BR-SEC-02).

| Entity | Key fields | Notes |
|---|---|---|
| School | name, type (гимназија, стручно, уметничко), municipality, headmaster_email, report_token, settings (leaderboard name mode) | One tenant |
| User | email, password_hash, role (teacher, admin, staff), name, first_run_done | Teachers and admins belong to one school |
| Student | school_id, full_name, school_email, class_id, display_name (nullable), avatar, joined_class_at, removed_at, nickname | Identity is the school email |
| StudentAuthSession | student_id, device info, created_at, revoked_at | One active per student (BR-AUTH-03) |
| PushToken | student_id, token, platform | The only device identifier stored |
| Class | school_id, name (for example "II-3"), year (I–IV), school_year | Roster lives here |
| ClassMembership | student_id, class_id, from, to | History follows a moved student (BR-ROS-03) |
| Subject | name, has_loaded_textbook | Pilot: history, electronics, physics |
| Course | class_id, subject_id, teacher_id, speed_bonus_on (default true) | INTERPRETATION, see above |
| Textbook / TextbookUnit | subject_id, year, unit number, title, chunked content | Loaded by staff (FR-LES-02). Blocked by a legal gate (section 13.4) |
| Lesson | owner_teacher_id, school_id, subject_id, year, unit_id (nullable), emphasis, source (textbook, typed), copied_from_lesson_id, current_version_id | Reusable |
| LessonVersion | lesson_id, number, content (opener, questions, review summary), created_at | Immutable snapshot; a session runs one version (BR-LES-03) |
| Question | lesson_version_id, order, kind, prompt, media, options, correct, tolerance, explanation, timer_seconds, topic | See BR-Q-* |
| Session | course_id, lesson_version_id, state, started_at, ended_at, opener_mode | One live run |
| JoinToken | session_id, token, short_code, valid_from, valid_to | Rotates (BR-JOIN-02) |
| SessionQuestion | session_id, question_id or ad-hoc content, fired_at, closes_at, closed_at, revealed_at, skipped_at | Server timer lives here |
| Attendance | session_id, student_id, status, joined_at, no_device_reason, marked_by | Statuses in 3.2 |
| Answer | session_question_id, student_id, response, correct, client_answered_at, received_at, response_ms, via_offline_queue, idempotency_key | One per student per question |
| Reason | session_id, student_id, text, correct_vote (nullable) | Opener reason |
| AnonQuestion | session_id, student_id, text, created_at, answered_at | Sender stored (BR-ANON-01) |
| FocusEvent | session_id, session_question_id, student_id, type (lost, back), at | Pilot web flag |
| FocusAuthorization | student_id, platform, state, updated_at | Native lock, later. Model it now |
| PointsEntry | student_id, session_id, course_id, season_id, source, amount, created_at | Append-only ledger (BR-PTS-07) |
| Badge / BadgeAward | badge code, title; student_id, awarded_at, points | Badge list OPEN-09 |
| Season | school_id, school_year, semester (1, 2), starts_on, ends_on | Dates OPEN-11 |
| PracticeSet / PracticeAttempt | session_id, questions; student_id, answers, score, completed_at | From missed questions |
| Notification | student_id, kind, payload, created_at, read_at | Student Alerts tab |
| MonthlyReport | school_id, month, pdf_location, page1_metrics, page2_comparison, sent_at | Kept in aggregate |
| DistractionList | school_id, entries | School-wide (BR-FOC-04) |
| StaffAccessGrant | staff_user_id, school_id, incident_ref, reason, starts_at, expires_at | Auto-expires |
| AuditLog | actor, action, target, at | Covers every staff access |
| DataSubjectRequest | school_id, student_id, kind (export, delete), received_at, due_at, completed_at, handled_by | Manual, durable tracking |
| SupportRequest | from_user_id, school_id, message, urgency, created_at | One-tap human channel |

### 3.2 Enumerations

- `session.state`: `lobby`, `opener`, `teaching`, `review`, `ended`
- `attendance.status`: `present`, `late`, `absent`, `present_no_device`
- `no_device_reason`: `no_phone`, `battery`, `no_data`, `no_focus_authorization`
- `question.kind`: `multiple_choice`, `true_false`, `numeric`, `opener_prediction`,
  `opener_debate` (the one-line reason is attached to the opener, BR-Q-01)
- `points.source`: `opener_correct`, `opener_reason`, `popup_correct`, `speed_bonus`,
  `practice_correct`, `badge`
- `focus_event.type`: `lost`, `back`
- `refinement`: `easier`, `harder`, `more_questions`, `opener_to_debate`,
  `opener_to_prediction`, `shorter_timers`
- `avatar` (from the student app): `initials`, `atom`, `rocket`, `telescope`, `zap`, `music`,
  `earth`, `palette`
- `console.student_state` per open question: `answered`, `not_answered`, `offline`

---

## 4. State machines

### 4.1 Session

    lobby -> opener -> teaching -> review -> ended

- Only the teacher advances the state (FR-SES-03). No timer advances it. There is no auto-close.
- `lobby`: the join token rotates; students join.
- `opener`: the opener runs (4.3).
- `teaching`: pop-ups may be fired (4.2); anonymous questions accepted.
- `review`: most-missed and anonymous questions shown.
- `ended`: set only by End (FR-END-01). Terminal. Joins and new answers rejected, except queued
  offline answers recorded before End (BR-NET-01).
- The spec does not say whether a teacher may go back a state, or fire pop-ups during the
  opener or review. Allow forward moves only, and End from any state (INTERPRETATION).
- Students may join in any state before `ended`; a join after `lobby` is late (OPEN-05).

### 4.2 Pop-up question instance

    ready -> open -> closed -> revealed
    (ready | open | closed) -> skipped

- `open`: set when the teacher fires it. The server sets `fired_at` and
  `closes_at = fired_at + timer_seconds` and broadcasts both with the server clock.
- `closed`: when `closes_at` passes. The student demo also closes a question as soon as every
  joined student has answered; PROPOSED, see BR-TIME-04.
- `revealed`: the correct answer and bars go to the class view.
- `skipped`: the teacher removed it; nobody scores on it, answers already given are discarded
  from scoring, and the class view says it does not count (BR-SKIP-01).
- At most one question open at a time (INTERPRETATION).

### 4.3 Opener

    voting -> reasoning -> revealed

- Prediction (default): 3–4 options with a correct one; students vote, then type a reason.
- Debate: a claim; students pick a side and give a reason. Scoring of the side is OPEN-07.
- Timers: PROPOSED 30 s for the vote, then 45 s for the reason (BR-TIME-01).

### 4.4 What the student app renders

The student demo already has these states (2026-10-13 student scope). The API must give a
student everything needed to render each one, with a single snapshot call after any reconnect.

| Student state | Server condition |
|---|---|
| Joined, waiting for start | session `lobby` |
| Answering | a question `open`, this student has not answered |
| Waiting for others | a question `open`, this student has answered |
| Right / Wrong | the question `closed` or `revealed`; the result for this student |
| Waiting for the next question | `teaching`, no question open |
| Ended | session `ended` |

---

## 5. User stories

Format: as a role, I want, so that. Linked requirement IDs in brackets.

### Teacher

- US-T01 As a teacher, I log in with my email and password and can reset the password quickly
  on my phone, so a forgotten password at 08:05 does not stop the class. [FR-AUTH-01..03]
- US-T02 As a teacher, I pick subject, year and unit from the e-textbook and add one optional
  sentence of emphasis, so I get a full lesson in two taps. [FR-LES-01..05]
- US-T03 As a teacher, I can type three sentences instead when there is no loaded book or I want
  something off-book. [FR-LES-06]
- US-T04 As a teacher, I edit any generated field and apply one-tap refinements, so the lesson
  fits my class without a conversation. [FR-LES-07..08]
- US-T05 As a teacher, I reuse one lesson for several classes, each run with its own results.
  [FR-LES-09, BR-LES-03]
- US-T06 As a teacher, I browse lessons other teachers in my school saved and copy one into my
  own account. [FR-LIB-01..03]
- US-T07 As a teacher, I start a session and open the class view in a second window with one
  click, so I can project it while keeping names on my screen. [FR-SES-01..02]
- US-T08 As a teacher, I see who joined, who is late, who is offline and who has not answered.
  [FR-CON-01..03]
- US-T09 As a teacher, I fire a pop-up with one tap, fire an ad-hoc question, and skip a bad one.
  [FR-POP-01..05]
- US-T10 As a teacher, I get a quiet nudge if I have not fired a question for a while.
  [FR-POP-06]
- US-T11 As a teacher, I read anonymous questions with the sender's name on my console only.
  [FR-ANON-01..04]
- US-T12 As a teacher, I mark a student without a working phone as present in one tap.
  [FR-ATT-04]
- US-T13 As a teacher, I press End and get only the absent names, ready to copy into e-Дневник.
  [FR-END-01..03]
- US-T14 As a teacher, I see the most-missed questions and the anonymous questions in the review.
  [FR-REV-01..02]
- US-T15 As a teacher, I send an optional practice set built from what the class got wrong, and
  see who did it. [FR-PRA-01..04]
- US-T16 As a teacher, I see per class: attendance, points, focus flags, per-topic correctness,
  practice, lesson history. [FR-DASH-01]
- US-T17 As a teacher, I can turn the speed bonus off for a class. [BR-PTS-04]
- US-T18 As a teacher, I can see that a session is still open and end it from anywhere.
  [FR-END-04]
- US-T19 As a teacher, I reach a human with one tap when something breaks in front of the class.
  [FR-SUP-02]

### Student

- US-S01 As a student, I sign in with my school email once, so my attendance and points are mine
  and nobody can pretend to be me. [FR-AUTH-04..06]
- US-S02 As a student, I scan the class QR (with the app's scanner or my phone's camera) and I am
  in the lesson and marked present. [FR-JOIN-01..07]
- US-S03 As a student, I vote on the opener and type one reason, and I get points for the reason
  even if I was wrong. [FR-OPN-01..04]
- US-S04 As a student, I answer pop-ups before the timer runs out and see right or wrong and my
  points. [FR-ANS-01..06]
- US-S05 As a student, if my connection drops while answering, my answer still counts with the
  time I gave it. [FR-ANS-05, BR-NET-01]
- US-S06 As a student, I send a question no classmate will see is mine. [FR-ANON-01..03]
- US-S07 As a student, I see my points, rank, badges, attendance and results history.
  [FR-STU-01..05]
- US-S08 As a student, I do an optional practice set and earn points. [FR-PRA-03]
- US-S09 As a student, I change my display name and avatar. [FR-STU-06]
- US-S10 As a student, I get alerts when results or practice are ready. [FR-NOT-01..03]

### School admin

- US-A01 As a school admin, I create classes and rosters, add, remove and move students, and
  invite and remove teachers. [FR-ADM-01..05]
- US-A02 As a school admin, I see which teachers run lessons and which have not started.
  [FR-ADM-07]
- US-A03 As a school admin, I adjust the school's distraction list once for everyone.
  [FR-ADM-06]
- US-A04 As a school admin, I resend the headmaster report. [FR-ADM-08]
- US-A05 As a school admin, I see every time TeacherAid staff accessed our data. [FR-ADM-09]

### Headmaster

- US-H01 As a headmaster, I receive a monthly report I can forward without editing, and keep a
  permanent private link to all past reports. [FR-REP-01..05]

### TeacherAid staff

- US-X01 As staff, I onboard a school and create its admin. [FR-STF-01]
- US-X02 As staff, I load and chunk e-textbooks per unit. [FR-LES-02]
- US-X03 As staff, I get time-limited, logged access to a school's data for one support incident.
  [FR-STF-02, BR-SEC-03]
- US-X04 As staff, I export or delete one student's data on the school's written request, and
  track it. [FR-STF-03]

---

## 6. User flows

Each flow lists the steps and what the backend does. Endpoint names refer to the proposed API
in section 11.

### F-01 Teacher sign-in and password reset

1. Teacher enters email and password. Server verifies and starts a session.
2. Forgot password: teacher enters email; server sends a reset link; the link opens a mobile
   friendly form; the new password signs the teacher in.
3. First sign-in: `first_run_done = false`, the client starts the guided first lesson (F-03).

### F-02 Student sign-in (method OPEN-01)

1. Student opens the app (or arrives from a QR link while signed out).
2. Student signs in with the school email account. Server matches the email to a Student on a
   roster. An unknown email is refused with a clear message (EC-AUTH-02).
3. Server ends any other active session of that student (BR-AUTH-03).
4. If the student arrived from a QR link, the client continues the join with the same token
   (F-07, EC-JOIN-05).

### F-03 Create a lesson from the e-textbook

1. Teacher picks subject, then year, then unit (FR-LES-01).
2. Optional: one sentence of emphasis.
3. Server calls the AI with the unit content and the sentence only, never roster data. Output is
   structured and validated (FR-LES-04).
4. The draft returns: opener, 5–8 pop-ups with answers and explanations, a suggested timer per
   question, the review summary. In Macedonian.
5. Teacher edits inline or taps a refinement (F-04), then saves. Saving creates a LessonVersion.

Fallback (FR-LES-06): no book loaded for the subject, or off-book: the teacher types three
sentences; steps 3 to 5 are the same without curriculum grounding.

### F-04 Refine a lesson

1. Teacher taps one of: Easier, Harder, More questions, Make the opener a debate (or a
   prediction), Shorter timers.
2. Server regenerates or adjusts and returns a new draft; the teacher reviews before saving.
   No free text.

### F-05 Copy from the school library

1. Teacher browses lessons saved by teachers in the same school.
2. Copy creates a new Lesson owned by the teacher with `copied_from_lesson_id`. The original is
   never modified.

### F-06 Run a live session (teacher)

1. Teacher chooses a lesson and a course and starts a session: state `lobby`. Server begins
   rotating the join token (BR-JOIN-02).
2. Teacher opens the class view in a second window (one click). The class view subscribes to
   the public channel; the console subscribes to the private teacher channel.
3. Students join (F-07). Console shows each join, with late marks after `lobby`.
4. Teacher starts the opener: vote, then reason, then reveal. Class view shows live bars,
   then the reveal, then reasons without names. Console shows reasons with names.
5. Teacher moves to `teaching` and teaches. One tap fires a pop-up: server opens it with
   `closes_at`. Console shows per student: answered, not answered, offline.
6. When it closes, the teacher reveals it. Class view shows bars, the correct answer and the
   leaderboard.
7. Optional: ad-hoc question (FR-POP-04) or skip (FR-POP-05).
8. Anonymous questions arrive on the console with sender names at any time.
9. If several minutes pass with no pop-up, the console shows a quiet nudge (FR-POP-06).
10. Teacher moves to `review`: class view shows the most-missed questions, worst first, and the
    anonymous questions (text only).
11. Teacher presses End (F-10).

### F-07 Student joins by QR

1. Student scans the QR on the class view, with the in-app scanner or the phone's own camera.
   The QR is a link to the student app containing the current join token.
2. Signed out: sign in first (F-02), then continue with the same token.
3. Server checks: token valid (BR-JOIN-02, BR-JOIN-03), session not ended, student enrolled in
   the course's class (EC-JOIN-02).
4. Server records Attendance `present` with `joined_at`, or `late` after `lobby` (OPEN-05).
5. A repeat join is idempotent: same attendance row, same state (EC-JOIN-04).
6. Response: session snapshot and the server time, so the client can sync its clock.
7. Checking in earns no points (BR-PTS-01).

The short code shown beside the QR can be used instead of scanning (FR-JOIN-04).

### F-08 Student answers a question

1. `question.opened` arrives with the question, `fired_at`, `closes_at` and server time.
2. Student answers. The client sends the answer with an idempotency key and the answer time in
   server-clock terms.
3. Server validates: question open, or closed but the answer was given in time (BR-NET-01);
   not skipped; first answer for this student (BR-ANS-02).
4. Server stores it, updates the console state to `answered`, and when the question closes
   sends the student their result: right or wrong, points, rank in this session.

Offline: the client queues the answer locally, retries on reconnect, and the server credits it
with the original answer time (BR-NET-01).

### F-09 Student sends an anonymous question

1. Any time before `ended`, the student types a question.
2. Server enforces the per-lesson limit (BR-ANON-02), stores the sender, pushes it to the
   console with the sender's name. Nothing reaches classmates.

### F-10 End a session

1. Teacher presses End, from the console or from anywhere in the app if a session is open
   (FR-END-04).
2. Server: settles points, writes attendance (enrolled students with no attendance become
   `absent`), archives the session, produces the absentee list.
3. Teacher sees only the absent names, formatted for copying into e-Дневник (OPEN-06).
4. Students get the ended state and their results; notifications are created (FR-NOT-01).

### F-11 Student without a working phone

1. Teacher taps the student on the console and picks the reason: no phone, battery, no data,
   no focus-lock authorization.
2. Server records `present_no_device` with the reason. No points for the lesson. The dashboard
   shows the reason.

### F-12 Practice set

1. After End, the teacher chooses to send a practice set to the class.
2. Server assembles it from the session's missed questions (selection rule OPEN-08). No AI call.
3. Students get a notification and can do it voluntarily; correct answers score
   (BR-PTS-03).
4. The teacher sees who completed it and their scores.

### F-13 School admin roster work

- Create a class; add students (name and school email); invite teachers by email; assign
  courses (OPEN-02); remove a teacher.
- Add a student mid-year: starts at zero points, marked as joined partway (BR-ROS-01).
- Remove a student: data retained until deletion is requested (BR-ROS-02).
- Move a student between classes: history moves with them (BR-ROS-03).

### F-14 Monthly headmaster report

1. A scheduled job builds each school's report for the previous month (day OPEN-12).
2. It renders a PDF: page one forwardable, page two headmaster only (FR-REP-02, FR-REP-03).
3. It emails the headmaster with the PDF and the permanent private link (FR-REP-04).
4. The link lists every past report, no account needed.

### F-15 Staff support access

1. A school reports an incident. Staff create a grant for that school with the incident
   reference and an expiry.
2. While the grant is active, staff can read that school's data. Every read is audit-logged.
3. The grant expires automatically. The school admin sees every grant and access.

### F-16 Data subject request

1. The school sends a written request to export or delete everything held on one student.
2. Staff log it with a due date, perform it with internal tools, and mark it done.

### F-17 Student edits profile

1. Student changes display name and avatar. Name rules: BR-STU-01.
2. Class and school come from the school account and cannot be edited.
3. Where the display name appears instead of the real name is OPEN-10.

---

## 7. Functional requirements

### 7.1 Authentication and accounts

- FR-AUTH-01 MUST: teachers and admins log in with email and password. No magic links, no SSO.
- FR-AUTH-02 MUST: password reset by email link, usable on a phone, fast.
- FR-AUTH-03 MUST: secure password storage, rate limiting on login and reset, lockout or delay
  after repeated failures.
- FR-AUTH-04 MUST: students authenticate with their school-issued email account (method
  OPEN-01). The student must already be on a roster.
- FR-AUTH-05 MUST: one active session per student account; signing in on a second device ends
  the first (BR-AUTH-03).
- FR-AUTH-06 MUST: the student client can tell "your session ended because you signed in
  elsewhere" from other errors.
- FR-AUTH-07 MUST: role-based access exactly as in section 2.

### 7.2 Lessons

- FR-LES-01 MUST: list subjects, years and units from loaded e-textbooks.
- FR-LES-02 MUST: staff-only ingestion that loads, chunks per unit and stores e-textbook content
  for the pilot subjects. Teachers never upload books. Live use is gated by section 13.4.
- FR-LES-03 MUST: generate a lesson from a unit plus an optional sentence of emphasis.
- FR-LES-04 MUST: generation returns structured output: opener, 5–8 pop-up questions with
  correct answers and explanations, a suggested timer per question, a review summary, in
  Macedonian. Validate against a schema; never save invalid output.
- FR-LES-05 MUST: the generator suggests a timer per question by its shape (a recall question
  is not a calculation); the teacher can override any timer before running.
- FR-LES-06 MUST: fallback generation from three typed sentences when no book is loaded or the
  teacher wants something off-book.
- FR-LES-07 MUST: every generated field is editable before saving.
- FR-LES-08 MUST: the six refinements in section 3.2 `refinement`. No free-text chat.
- FR-LES-09 MUST: a lesson is reusable; each save creates an immutable LessonVersion; a session
  references the version it ran.
- FR-LES-10 MUST: questions support images (in the question, the options and the reveal) and
  formulas (maths and chemical notation). Formula format OPEN-13.
- FR-LES-11 MUST: images are stored with compressed, phone-sized variants for low bandwidth.
- FR-LES-12 SHOULD: measure AI cost per school (tokens per generation) from day one; the cost
  model depends on it.

### 7.3 School library

- FR-LIB-01 MUST: every saved lesson is visible to other teachers in the same school.
- FR-LIB-02 MUST: a teacher copies a library lesson into their own account and edits the copy.
- FR-LIB-03 MUST: the original is never modified by a copy. No cross-school or public sharing.

### 7.4 Live session

- FR-SES-01 MUST: a teacher starts a session from a lesson for one of their courses; state
  `lobby`.
- FR-SES-02 MUST: two views of the session: the class view (public projection, no private data)
  and the console (full teacher data). The class view is a separate window the client opens with
  one click; the backend provides a class-view snapshot and channel that contain no personal data
  beyond what BR-PRIV-* allows.
- FR-SES-03 MUST: the teacher advances the state; transitions per section 4.1.
- FR-SES-04 MUST: realtime push of every state change to the class view, the console and each
  student.
- FR-SES-05 MUST: a snapshot endpoint per audience (class view, console, student) for reconnects.
- FR-SES-06 MUST: every snapshot and timer event carries the server time.
- FR-SES-07 MUST: no live text editing of questions during a session.
- FR-SES-08 MUST: a session survives the teacher's console or class view window closing and
  reopening.

### 7.5 Joining and attendance

- FR-JOIN-01 MUST: the class view shows a QR and a short code for the session.
- FR-JOIN-02 MUST: the QR encodes a link into the student app carrying the current token, so the
  phone's own camera app works as well as the in-app scanner (the demo uses
  `/app/checkin/<code>`; keep that path shape).
- FR-JOIN-03 MUST: the token and short code rotate every 30 seconds (BR-JOIN-02).
- FR-JOIN-04 MUST: the join endpoint accepts either the QR token or the short code.
- FR-JOIN-05 MUST: joining requires an authenticated student enrolled in the course's class.
- FR-JOIN-06 MUST: joining records attendance with a timestamp; late joins are marked late.
- FR-JOIN-07 MUST: joining is idempotent per student and session.
- FR-ATT-01 MUST: on End, enrolled students with no attendance become `absent`.
- FR-ATT-02 MUST: TeacherAid attendance is its own record. Never write to e-Дневник.
- FR-ATT-03 MUST: attendance history per student and per course for dashboards and the student
  app.
- FR-ATT-04 MUST: the teacher marks a student `present_no_device` in one tap with a reason.

### 7.6 Opener

- FR-OPN-01 MUST: two modes, prediction (default) and debate.
- FR-OPN-02 MUST: vote, then a one-line reason, then the reveal; live vote counts on the class
  view.
- FR-OPN-03 MUST: reasons on the class view without names; on the console with names.
- FR-OPN-04 MUST: scoring per BR-PTS-02. Not configurable by teachers.

### 7.7 Pop-up questions and answers

- FR-POP-01 MUST: the teacher fires a prepared pop-up with one tap; it reaches every joined
  student with its timer running.
- FR-POP-02 MUST: pop-ups are teacher-triggered only, never scheduled.
- FR-POP-03 MUST: the server opens and closes each question by its own clock.
- FR-POP-04 MUST: the teacher can fire an ad-hoc question without preparation ("quick: yes or
  no?"). Its type and scoring are OPEN-14.
- FR-POP-05 MUST: the teacher can skip a question during the session (BR-SKIP-01).
- FR-POP-06 MUST: if no question has been fired for a while during `teaching`, the console gets a
  quiet nudge event. Not a modal, not a sound. Interval OPEN-16.
- FR-ANS-01 MUST: answer types per BR-Q-01, auto-scored.
- FR-ANS-02 MUST: numeric answers are correct within the author's tolerance; accept a comma or a
  dot as the decimal separator (Macedonian writes 0,2).
- FR-ANS-03 MUST: one answer per student per question; it is final (BR-ANS-02).
- FR-ANS-04 MUST: idempotent submission (client idempotency key).
- FR-ANS-05 MUST: accept answers queued offline and credit them with their original answer time
  (BR-NET-01).
- FR-ANS-06 MUST: after a question closes, each student gets their own result: right or wrong,
  points gained, running points and rank in this session.

### 7.8 Anonymous questions

- FR-ANON-01 MUST: a student can send a question any time before End.
- FR-ANON-02 MUST: stored with the sender; the teacher sees the sender; no classmate ever does.
- FR-ANON-03 MUST: limit per student per lesson (BR-ANON-02).
- FR-ANON-04 MUST: the inbox lives on the console only; the class view shows the text without
  names during review.

### 7.9 Console

- FR-CON-01 MUST: per student: joined or not, late, answer state for the open question
  (`answered`, `not_answered`, `offline`), focus flags, no-device reason.
- FR-CON-02 MUST: distinguish offline from not answered using realtime presence.
- FR-CON-03 MUST: the anonymous inbox with sender names.
- FR-CON-04 MUST: controls: advance state, fire, reveal, skip, ad-hoc, mark present without a
  device, End.

### 7.10 Focus

- FR-FOC-01 MUST (pilot): the student client reports `lost` and `back` events when the student
  leaves the app during an open question. The server stores them and pushes a flag to the
  console. Nobody is blocked (BR-FOC-01).
- FR-FOC-02 MUST: focus events queued offline are accepted with their original times.
- FR-FOC-03 SHOULD: model FocusAuthorization now so the native lock can be added without a
  schema change (BR-FOC-02).
- FR-FOC-04 MUST: the school admin can edit the school-wide distraction list; TeacherAid ships a
  default (social, games, messaging). In the web pilot the list has no technical effect; it is
  needed for the native lock.

### 7.11 Ending and review

- FR-END-01 MUST: only the teacher ends a session. No auto-close.
- FR-END-02 MUST: End settles points, writes attendance, archives the session, returns the
  absentee list.
- FR-END-03 MUST: the absentee list contains only absent students' names, formatted for copying
  into e-Дневник (format OPEN-06).
- FR-END-04 MUST: an endpoint that reports any open session for the teacher, and End works from
  anywhere in the app.
- FR-REV-01 MUST: the review shows the most-missed questions ranked worst first.
- FR-REV-02 MUST: the review shows the anonymous questions for the teacher to answer aloud. No AI
  explanations.

### 7.12 Practice sets

- FR-PRA-01 MUST: after a session the teacher can send an optional practice set to the class.
- FR-PRA-02 MUST: assembled automatically from that session's missed questions. No new
  generation, no AI cost.
- FR-PRA-03 MUST: voluntary; correct answers score (BR-PTS-03).
- FR-PRA-04 MUST: the teacher sees who completed it and how they scored.

### 7.13 Student data

- FR-STU-01 MUST: the student's points this season and rank in the class leaderboard.
- FR-STU-02 MUST: attendance percentage and log (course, date, status).
- FR-STU-03 MUST: results history: per session, right and wrong per question.
- FR-STU-04 MUST: badges earned.
- FR-STU-05 MUST: per-topic correctness over time, own only.
- FR-STU-06 MUST: edit display name and avatar (BR-STU-01).
- FR-STU-07 MUST: settings: notifications on or off; language if OPEN-15 keeps English.

### 7.14 Notifications

- FR-NOT-01 MUST: an in-app notification feed for students with read state and mark-all-read.
  The student app has an Alerts tab. Triggers PROPOSED: results ready after End, practice set
  sent, marked late, badge earned.
- FR-NOT-02 SHOULD: web push to students who allowed it; store the push token.
- FR-NOT-03 MUST: respect the student's notifications switch.

### 7.15 Dashboards

- FR-DASH-01 MUST: teacher dashboard, one screen per course: attendance per lesson and per
  student; points and participation over time; focus flags with reasons; per-topic correctness,
  class and per student; the anonymous inbox with senders; practice completion and scores;
  lesson history, every lesson reusable. Nothing to configure.
- FR-DASH-02 SHOULD: compare two classes on the same question when they ran the same lesson.
- FR-DASH-03 MUST: show a student's no-device reason so a zero is not read as disengagement.
- FR-DASH-04 MUST: flag students who joined the class mid-year.

### 7.16 School admin

- FR-ADM-01 MUST: create and edit classes.
- FR-ADM-02 MUST: manage rosters: add, remove, move students; bulk import SHOULD exist
  (format OPEN).
- FR-ADM-03 MUST: invite and remove teachers.
- FR-ADM-04 MUST: assign courses (OPEN-02).
- FR-ADM-05 MUST: no access to individual answers, reasons, anonymous questions or per-topic
  results.
- FR-ADM-06 MUST: edit the distraction list.
- FR-ADM-07 MUST: usage overview: which teachers run lessons, how often, which have not started.
- FR-ADM-08 MUST: resend the latest headmaster report.
- FR-ADM-09 MUST: view the staff-access log.

### 7.17 Headmaster report

- FR-REP-01 MUST: monthly, automatic, emailed as a PDF. No login.
- FR-REP-02 MUST: page one, forwardable: lessons run, teachers active, average attendance,
  average engagement (answers per pop-up, definition to confirm), top classes, top teachers, a
  two-line plain-language summary, month-on-month trend.
- FR-REP-03 MUST: page two, headmaster only: comparison against other schools using TeacherAid.
- FR-REP-04 MUST: the email carries a permanent, unguessable private link to every past report.
- FR-REP-05 MUST: reports are kept in aggregate, without student names.

### 7.18 Support and onboarding

- FR-SUP-01 SHOULD: searchable in-app help content (the frontend may host it statically).
- FR-SUP-02 MUST: one-tap direct channel to a human: create a support request and alert staff
  immediately (delivery channel OPEN).
- FR-SUP-03 MUST: a first-run flag per teacher for the guided first lesson.

### 7.19 TeacherAid staff

- FR-STF-01 MUST: onboard a school: create the school, its admin and the headmaster email.
- FR-STF-02 MUST: create time-limited support grants; all access audit-logged and visible to the
  school admin.
- FR-STF-03 MUST: export or delete everything on one student; track each request durably.
- FR-STF-04 MUST: load e-textbooks (FR-LES-02).

### 7.20 Data lifecycle

- FR-DATA-01 MUST: retention jobs per BR-RET-*.
- FR-DATA-02 MUST: deletion is complete across tables, files and search indexes, and backups
  expire on a known schedule.

---

## 8. Business rules

| ID | Rule | Value | Status |
|---|---|---|---|
| BR-AUTH-01 | Teacher login | Email and password; no magic links, no SSO | DECIDED |
| BR-AUTH-02 | Student identity | School-issued email account | DECIDED (pilot, 2026-10-15) |
| BR-AUTH-03 | Student sessions | One active per account; a new sign-in ends the old one | DECIDED |
| BR-JOIN-01 | Join is attendance | Joining marks present with a timestamp | DECIDED |
| BR-JOIN-02 | QR rotation | Token and short code rotate every 30 s | DECIDED |
| BR-JOIN-03 | Token grace | Accept the current and the previous token | PROPOSED |
| BR-JOIN-04 | Late | A join after the session leaves `lobby` is late | OPEN-05 (suggested default) |
| BR-JOIN-05 | Who can join | Only students enrolled in the course's class | DECIDED |
| BR-Q-01 | Question types | Multiple choice (2–4 options); true/false; numeric with tolerance; opener vote plus one-line reason | DECIDED |
| BR-Q-02 | Questions per lesson | 5–8 pop-ups plus one opener | DECIDED |
| BR-Q-03 | Reason length | One line; 80 characters in the prototype | PROPOSED |
| BR-TIME-01 | Timer defaults | Recall and true/false 15 s; multiple choice 25 s; numeric 45 s; opener vote 30 s then reason 45 s | PROPOSED |
| BR-TIME-02 | Timer override | Teacher may change any timer before the lesson runs | DECIDED |
| BR-TIME-03 | Timer authority | Server | DECIDED |
| BR-TIME-04 | Early close | Close when every joined, online student has answered (student demo behaviour) | PROPOSED |
| BR-ANS-01 | Auto-scoring | All four types auto-scored | DECIDED |
| BR-ANS-02 | Final answer | One answer per question; final once sent (the student app locks it) | DECIDED in the frontend; confirm |
| BR-PTS-01 | Point sources | Only answering (opener, pop-ups, practice) and badges. Checking in earns nothing | DECIDED (2026-10-13) |
| BR-PTS-02 | Live points | Opener correct 10; opener incorrect 0; reason written 8; pop-up correct 10; speed bonus up to 5 | PROPOSED |
| BR-PTS-03 | Practice points | 5 per correct answer | PROPOSED |
| BR-PTS-04 | Speed bonus | round(5 × time remaining ÷ duration), correct answers only; the teacher may switch it off per course | Formula PROPOSED (demo); switch DECIDED |
| BR-PTS-05 | Same everywhere | Values identical in every school, not teacher-configurable | DECIDED |
| BR-PTS-06 | Reason ratio | A reasoned wrong answer scores close to a silent right one | DECIDED (the mechanism) |
| BR-PTS-07 | Ledger | Points are an append-only ledger; totals are derived | INTERPRETATION |
| BR-PTS-08 | Badge points | 20 per badge | PROPOSED |
| BR-PTS-09 | Session rank | Everyone starts each session at 0; rank and score shown in the quiz are per session; session points also add to the season total | DECIDED (2026-10-13) |
| BR-PTS-10 | Settlement | Points settle on End | DECIDED |
| BR-SKIP-01 | Skipped question | Removed from the session; nobody scores; the class view says it does not count | DECIDED |
| BR-ANON-01 | Anonymity | Anonymous to classmates; the teacher sees the sender | DECIDED |
| BR-ANON-02 | Anonymous limit | 3 per student per lesson | DECIDED (default) |
| BR-NET-01 | Offline answers | Queued answers are credited with their original answer time if it falls inside the question's window and they arrive before End | DECIDED (rule); window check PROPOSED |
| BR-NET-02 | Offline teacher | The lesson is cached on the teacher's device; if everything fails it runs aloud and points pause | DECIDED |
| BR-FOC-01 | Pilot focus | Web focus flag: leaving the app during an open question is flagged on the console; nobody is blocked | DECIDED (pilot exception, 2026-10-15) |
| BR-FOC-02 | Product focus | Native lock mandatory: no authorization, no join; that student is handled as present without a device, never absent | DECIDED (target after the pilot) |
| BR-FOC-03 | Lock privacy | The lock never reads activity in other apps, only whether TeacherAid is in the foreground | DECIDED |
| BR-FOC-04 | Distraction list | School-wide, edited by the admin; default social, games, messaging | DECIDED |
| BR-ATT-01 | Never falsely absent | Phone, battery, data or lock problems give `present_no_device` with no points, never `absent` | DECIDED |
| BR-LES-01 | Primary input | The e-textbook unit; three typed sentences is the fallback | DECIDED |
| BR-LES-02 | Refinements | Six fixed one-tap actions; no chat | DECIDED |
| BR-LES-03 | Versioning | Editing a lesson affects later runs only, never past session records | DECIDED |
| BR-LES-04 | Review before students | No AI content reaches students before the teacher reviews it | DECIDED |
| BR-LIB-01 | Library scope | Same school only; copies never change the original | DECIDED |
| BR-LB-01 | Class leaderboard | Real names | DECIDED |
| BR-LB-02 | School leaderboard | Nicknames by default; the school may choose otherwise | DECIDED |
| BR-LB-03 | Seasons | Per semester, matching the Macedonian school calendar | DECIDED |
| BR-LB-04 | No national board | — | DECIDED |
| BR-LB-05 | Mid-year joiner | Starts at zero; the leaderboard shows they joined partway | DECIDED |
| BR-ROS-01 | Add mid-year | Zero points, flagged | DECIDED |
| BR-ROS-02 | Remove | Data retained under BR-RET until deletion is requested | DECIDED |
| BR-ROS-03 | Move | History moves with the student | DECIDED |
| BR-STU-01 | Display name | 2–24 characters; letters of any script with single spaces, hyphens, apostrophes and full stops between them; empty means the school account name | DECIDED in the frontend |
| BR-PRIV-01 | Class view | Shows the class leaderboard with real names, reasons and anonymous questions without names; never the console's named data | DECIDED |
| BR-RET-01 | Session data | Current school year plus one, then aggregated and deleted | DECIDED |
| BR-RET-02 | Reports | Kept in aggregate, without names | DECIDED |
| BR-RET-03 | Leaver | Deleted within 30 days of the school's request | DECIDED |
| BR-SEC-01 | Hosting | EU; encryption in transit and at rest | DECIDED |
| BR-SEC-02 | Tenancy | Every query scoped to one school | INTERPRETATION (required by section 2) |
| BR-SEC-03 | Staff access | No standing access; per-incident grant, time-limited, auto-expiring, audit-logged, visible to the school admin | DECIDED |
| BR-SEC-04 | AI data | Student names and roster never sent to the AI; generation uses the unit and the teacher's sentence | DECIDED |
| BR-SEC-05 | Not stored | Grades; student contact details beyond the school email; location; device identifiers beyond a push token; anything about other apps | DECIDED |

---

## 9. Edge cases and exceptions

| ID | Situation | Required behaviour |
|---|---|---|
| EC-JOIN-01 | A photo of the QR is sent to a friend at home | The token is expired within about 30–60 s; reject with "code expired, scan the board" |
| EC-JOIN-02 | A student scans another class's code | Reject: not enrolled in this class (the demo shows a clear message for this) |
| EC-JOIN-03 | The QR is not a TeacherAid code | The client handles it; the server never receives it |
| EC-JOIN-04 | Double scan or a page refresh | Idempotent: one attendance row, same state returned |
| EC-JOIN-05 | Signed out when scanning | Sign in, then join with the same token. If signing in outlasts the token, OPEN-04 |
| EC-JOIN-06 | Joining late | Attendance `late` with the timestamp; can still answer open and future questions |
| EC-JOIN-07 | Joining after End | Reject: the session has ended |
| EC-JOIN-08 | The camera is denied or missing | The student can use the short code if the frontend offers typing (OPEN-03); otherwise the teacher marks them present without a device |
| EC-AUTH-01 | Signing in on a second device | The first device's session ends; it receives a clear event; answers it queued before the switch are still accepted (PROPOSED) |
| EC-AUTH-02 | School email not on any roster | Refuse sign-in with a clear message; tell the student to ask the school |
| EC-AUTH-03 | The school does not issue student mailboxes | No student can sign in (assumption A, section 13). No workaround in v1 |
| EC-AUTH-04 | A teacher forgets the password at 08:05 | Reset by email from a phone in under a minute |
| EC-NET-01 | The student's network drops during a question | Answer queued; credited with the original time on reconnect |
| EC-NET-02 | A queued answer arrives after the question closed | Accept if the answer time is inside the window and End has not happened; otherwise reject |
| EC-NET-03 | A queued answer arrives for a skipped question | Discard |
| EC-NET-04 | Offline versus not answered | Console shows `offline` from presence, not `not_answered`, so no unfair focus flag |
| EC-NET-05 | The teacher's network dies | The session stays in its state; the teacher runs questions aloud; on reconnect the console resumes from a snapshot; points pause in between |
| EC-NET-06 | Answer retried several times | Idempotency key: stored once |
| EC-NET-07 | Student clock is wrong | The client uses the server time from snapshots; the server clamps answer times to the question window |
| EC-TIME-01 | The timer hits zero while an answer is in flight | Server decides by answer time inside the window, with a small network grace (PROPOSED) |
| EC-TIME-02 | Everyone answered early | Close early (BR-TIME-04, PROPOSED) |
| EC-TIME-03 | A slow image | Never delay `fired_at` or the timer for an image; the client shows it when loaded |
| EC-SES-01 | The teacher forgets End | Session stays open; points unsettled; the open-session indicator shows it; End from anywhere |
| EC-SES-02 | Starting a new session while one is open | PROPOSED: at most one open session per course; ask the teacher to end the open one first |
| EC-SES-03 | A bad question mid-lesson | Skip it; no live text edit; fix the lesson afterwards |
| EC-SES-04 | Console or class view window closed and reopened | Session continues; the window restores from a snapshot |
| EC-SES-05 | The class view is projected by mistake in place of the console | Frontend risk; the backend still keeps named data off the class-view channel |
| EC-SES-06 | No pop-up fired for a long time | Quiet nudge to the console (interval OPEN-16) |
| EC-ANON-01 | A fourth anonymous question in one lesson | Reject with a clear limit message |
| EC-ANON-02 | An abusive anonymous question | The teacher sees the sender; that is the safety mechanism |
| EC-ATT-01 | No phone, dead battery, no data, no lock authorization | Teacher marks `present_no_device` with the reason; no points; reason on the dashboard |
| EC-FOC-01 | The student leaves the app during a question (pilot) | Focus flag on the console with the time; the student is not blocked |
| EC-FOC-02 | The student leaves between questions | No flag (the pilot rule covers open questions only) |
| EC-FOC-03 | A phone call or the screen locks during a question | Flagged like any exit; the teacher judges it |
| EC-FOC-04 | Focus events recorded while offline | Queued and accepted with their times |
| EC-Q-01 | Numeric answer with a comma ("0,2") | Parse comma and dot |
| EC-Q-02 | Empty or overlong reason | Reject empty; trim and cap at the limit (BR-Q-03) |
| EC-Q-03 | Debate opener, no right side | Reason points apply; vote scoring OPEN-07 |
| EC-LES-01 | No loaded e-textbook for the subject | Fallback path from three sentences |
| EC-LES-02 | Generation fails or returns invalid output | Retry once (PROPOSED), then a clear error; never save a broken lesson |
| EC-LES-03 | A lesson is edited between runs | Earlier session records keep the version they ran |
| EC-LES-04 | The original of a copied lesson is deleted | Copies survive (INTERPRETATION) |
| EC-PRA-01 | No question was missed | Nothing to send; say so |
| EC-ROS-01 | A student joins the school mid-year | Zero points; flagged on the leaderboard |
| EC-ROS-02 | A student moves class | History moves; the old class's leaderboard drops them for the rest of the season (INTERPRETATION) |
| EC-ROS-03 | A student leaves the school | Removed from rosters; data kept until the school requests deletion; deleted within 30 days of the request |
| EC-ROS-04 | A teacher goes on long leave or leaves | No supported path in v1 (known limitation). Do not build reassignment |
| EC-REP-01 | A month with no lessons (holidays) | OPEN-12 |
| EC-REP-02 | Few schools for page two | Show what exists; never reveal other schools' names unless decided (INTERPRETATION) |
| EC-SEC-01 | A staff grant expires mid-task | Access stops at expiry, no exceptions |
| EC-LB-01 | Tied points | Tie rule OPEN (the demo ranks the viewing student first) |
| EC-NAME-01 | Cyrillic and Latin names | Store as given. The English UI transliterates on the client |

---

## 10. Acceptance criteria

Given / When / Then. Each criterion is a test. IDs map to features.

### Authentication

- AC-AUTH-01 Given a teacher with a valid account, when they submit the right email and password,
  then they are signed in; with a wrong password they are not, and repeated failures are slowed.
- AC-AUTH-02 Given a teacher who forgot the password, when they request a reset, then an email
  link arrives and lets them set a new password on a phone.
- AC-AUTH-03 Given a student signed in on phone A, when they sign in on phone B, then phone A's
  session ends and phone A receives a "signed in elsewhere" event.
- AC-AUTH-04 Given a school email not on any roster, when it signs in, then sign-in is refused
  with a message to contact the school.

### Joining and attendance

- AC-JOIN-01 Given a session in `lobby`, when an enrolled student submits the current token, then
  attendance is `present` with `joined_at` and the console shows the join in real time.
- AC-JOIN-02 Given a token older than the grace window, when it is submitted, then the join is
  rejected as expired.
- AC-JOIN-03 Given a student not enrolled in the class, when they submit a valid token, then the
  join is rejected as not their class.
- AC-JOIN-04 Given a student already joined, when they join again, then no second attendance row
  exists and the same state returns.
- AC-JOIN-05 Given a session past `lobby`, when a student joins, then attendance is `late`
  (subject to OPEN-05).
- AC-JOIN-06 Given a valid short code, when it is submitted in place of the token, then the join
  succeeds like a scan.
- AC-JOIN-07 Given a join, then the student's points do not change.
- AC-ATT-01 Given an enrolled student who never joined, when the teacher presses End, then their
  attendance is `absent` and their name is in the absentee list.
- AC-ATT-02 Given a student marked `present_no_device` with reason `battery`, when End is
  pressed, then they are not absent, have no points for the session, and the dashboard shows
  "battery".

### Live session

- AC-SES-01 Given a session, when the teacher advances it, then the class view, console and
  students all receive the new state within the realtime layer's normal latency.
- AC-SES-02 Given any class-view payload, then it contains no student names except the class
  leaderboard, and never an anonymous question's sender or a reason's author.
- AC-SES-03 Given the console window is closed and reopened mid-session, then it restores the
  full current state from a snapshot.
- AC-SES-04 Given no pop-up fired during `teaching` for the nudge interval, then the console
  receives one nudge event.

### Questions and answers

- AC-ANS-01 Given a fired pop-up with a 25 s timer, then every joined student receives the
  question with `fired_at`, `closes_at` and the server time, and the question closes at
  `closes_at` by the server's clock.
- AC-ANS-02 Given a correct multiple-choice answer with half the time left and the speed bonus on,
  then the student gets 10 plus round(5 × 0.5) = 13 points (PROPOSED values).
- AC-ANS-03 Given the speed bonus switched off for the course, then the same answer gets 10.
- AC-ANS-04 Given a numeric question 2601 ± 10, then 2595 and 2611 are correct, 2590 is wrong,
  and "2601,0" parses as 2601.
- AC-ANS-05 Given a student answered, when they send a second answer, then it is rejected and the
  first stands.
- AC-ANS-06 Given a student who answered at second 8 while offline and reconnected at second 40,
  when the queued answer arrives before End, then it is credited with the second-8 time.
- AC-ANS-07 Given the same answer sent three times with one idempotency key, then one answer is
  stored.
- AC-ANS-08 Given a student whose connection dropped, then the console shows `offline` for them,
  not `not_answered`.
- AC-SKIP-01 Given a question some students answered, when the teacher skips it, then nobody gets
  points for it, the class view shows that it does not count, and later queued answers to it are
  discarded.

### Opener

- AC-OPN-01 Given a prediction opener, when a student votes wrong and writes a reason, then they
  get 0 + 8 points; a right vote without a reason gets 10 (PROPOSED values).
- AC-OPN-02 Given reasons on the class view, then no names appear; on the console names appear.

### Anonymous questions

- AC-ANON-01 Given a student sends an anonymous question, then the console shows it with the
  sender's name and no student endpoint ever returns the sender.
- AC-ANON-02 Given a student has sent 3 in this lesson, when they send a 4th, then it is rejected.

### Focus (pilot)

- AC-FOC-01 Given a question is open, when the student app reports `lost`, then the console shows
  a focus flag for that student within the realtime latency, and the student can still answer.
- AC-FOC-02 Given no question is open, when the app reports `lost`, then no flag is raised.

### End and review

- AC-END-01 Given a session in any state, when the teacher presses End, then points settle,
  attendance is written, and the response holds only the absent names.
- AC-END-02 Given a session still open from earlier, when the teacher opens the app anywhere,
  then the open session is reported and End works in one action.
- AC-END-03 Given an ended session, when a student tries to join or answer live, then it is
  rejected.
- AC-REV-01 Given an ended or reviewing session, then the most-missed list is ordered by the share
  of wrong answers, worst first.

### Lessons and library

- AC-LES-01 Given a unit and an emphasis sentence, when generation runs, then the result has one
  opener, 5–8 pop-ups each with a correct answer, explanation and timer, and a review summary,
  all in Macedonian, and passes schema validation.
- AC-LES-02 Given any generation request, then the payload sent to the AI contains no student
  names, emails or roster data.
- AC-LES-03 Given a lesson run for II-1 and then edited, when it runs for II-2, then II-1's
  session still shows the original questions.
- AC-LES-04 Given each refinement, then it returns a new draft that the teacher must save before
  it is used.
- AC-LIB-01 Given a teacher copies a colleague's lesson and edits the copy, then the original is
  unchanged; a teacher from another school cannot see either.

### Practice

- AC-PRA-01 Given a session with missed questions, when the teacher sends a practice set, then it
  contains only questions from that session, no AI call is made, and students are notified.
- AC-PRA-02 Given a student completes it, then they get 5 points per correct answer (PROPOSED)
  and the teacher sees completion and score.

### Admin, privacy and security

- AC-ADM-01 Given a school admin, when they request any student answer, reason, anonymous
  question or per-topic result, then access is denied.
- AC-ADM-02 Given a student moved from II-1 to II-2, then their full history is visible to the
  II-2 teacher's views of that student going forward, and nothing is split.
- AC-ADM-03 Given a student added mid-year, then they start at zero and the leaderboard marks
  them as joined partway.
- AC-SEC-01 Given a teacher, when they request another teacher's course, then access is denied.
- AC-SEC-02 Given any request, then it can only read data of the caller's school.
- AC-SEC-03 Given a staff grant, then every access during it is in the audit log, the school
  admin can see it, and access stops at expiry.
- AC-RET-01 Given session data older than the current school year plus one, when the retention
  job runs, then it is aggregated and personal data is deleted.

### Report

- AC-REP-01 Given a school with lessons last month, when the monthly job runs, then the headmaster
  receives an email with a two-page PDF (page one forwardable metrics with month-on-month trend,
  page two comparison) and the permanent link.
- AC-REP-02 Given the permanent link, then it lists every past report without an account, and a
  guessed or altered link returns nothing.

---

## 11. Proposed API surface

PROPOSED. The backend developer owns the final contract. Agree changes with the frontend before
implementing them; once agreed, record them here and bump the version.

Conventions: JSON over HTTPS, versioned under `/api/v1`, ISO 8601 UTC times, every snapshot and
timer event includes `server_now`. Errors carry a stable machine code (for example
`token_expired`, `not_enrolled`, `session_ended`, `already_answered`, `limit_reached`,
`signed_in_elsewhere`) that the client maps to Macedonian copy.

### 11.1 REST

Auth

    POST   /auth/login                     teacher, admin, staff
    POST   /auth/password/forgot
    POST   /auth/password/reset
    POST   /auth/student/...               method depends on OPEN-01
    POST   /auth/logout
    GET    /me

Student

    PATCH  /me/profile                     display_name (or null), avatar
    PATCH  /me/settings                    notifications_on, locale (if OPEN-15 keeps English)
    PUT    /me/push-token
    GET    /me/courses
    GET    /me/attendance                  percentage and log
    GET    /me/results                     per session; ?course=
    GET    /me/topics                      per-topic correctness over time
    GET    /me/leaderboard                 ?scope=class|school
    GET    /me/badges
    GET    /me/notifications
    POST   /me/notifications/read          ids or all
    GET    /me/practice-sets
    POST   /practice-sets/{id}/answers

Live session, student side

    POST   /sessions/join                  { token } or { short_code }
    GET    /sessions/{id}/student-state    snapshot for reconnect
    POST   /sessions/{id}/answers          { session_question_id, response, answered_at, idempotency_key }
    POST   /sessions/{id}/opener/vote
    POST   /sessions/{id}/opener/reason
    POST   /sessions/{id}/anon-questions   { text }
    POST   /sessions/{id}/focus-events     [ { type, at, session_question_id } ]

Teacher

    GET    /teacher/courses
    PATCH  /courses/{id}                   speed_bonus_on
    GET    /courses/{id}/dashboard
    GET    /textbooks                      ?subject=&year=
    GET    /textbooks/{id}/units
    POST   /lessons/generate               { unit_id, emphasis } or { subject_id, year, sentences }
    POST   /lessons/drafts/{id}/refine     { refinement }
    POST   /lessons                        save a draft as a lesson (version 1)
    GET    /lessons
    GET    /lessons/{id}
    PUT    /lessons/{id}                   new version
    GET    /library
    POST   /library/{lesson_id}/copy
    POST   /sessions                       { lesson_id, course_id }
    GET    /sessions/open
    GET    /sessions/{id}/console          snapshot
    GET    /sessions/{id}/class-view       public snapshot
    POST   /sessions/{id}/advance          { to }
    POST   /sessions/{id}/questions/{sqid}/fire
    POST   /sessions/{id}/questions/{sqid}/reveal
    POST   /sessions/{id}/questions/{sqid}/skip
    POST   /sessions/{id}/adhoc            OPEN-14
    POST   /sessions/{id}/attendance/{student_id}/no-device   { reason }
    POST   /sessions/{id}/end              returns absentees
    GET    /sessions/{id}/review
    POST   /sessions/{id}/practice-set
    POST   /support                        one-tap human channel

School admin

    GET|POST|PATCH        /admin/classes
    GET|POST|DELETE       /admin/classes/{id}/students
    POST                  /admin/students/{id}/move     { to_class_id }
    GET|POST|DELETE       /admin/teachers
    GET|POST|PATCH        /admin/courses
    GET|PUT               /admin/distraction-list
    GET                   /admin/usage
    GET                   /admin/reports
    POST                  /admin/reports/{id}/resend
    GET                   /admin/staff-access-log

Staff (internal)

    /staff/schools, /staff/admins, /staff/textbooks, /staff/access-grants, /staff/data-requests

Public

    GET    /reports/{report_token}         headmaster's permanent private link

### 11.2 Realtime channels and events

| Channel | Who subscribes | Events |
|---|---|---|
| `session.{id}.public` | Class view (and students) | `state.changed`, `join_token.rotated` (class view only), `joined.count`, `opener.counts`, `opener.reasons` (no names), `question.opened` (no correct answer), `question.closed`, `question.revealed` (counts, correct, explanation), `question.skipped`, `leaderboard.updated`, `review.ready` |
| `session.{id}.console` | The teacher only | Everything public plus `student.joined`, `student.answer_state`, `student.presence`, `reason.received` (with name), `anon_question.received` (with sender), `focus.flag`, `nudge` |
| `student.{id}` | One student | `question.result` (right or wrong, points, session rank), `session.ended`, `signed_in_elsewhere`, `notification.created` |

Presence on the session channel drives `offline` versus `not_answered`. Never put the correct
answer in `question.opened`.

---

## 12. What the existing student frontend expects

The student web app at `/app` (repository `teacher-aid-app`) is a clickable demo on mock data
held in the browser (localStorage). It is the reference for behaviour, not for data. When it is
wired to the API:

| Screen | Data it needs from the API |
|---|---|
| Home | Student name and avatar; rank and season points in the class; attendance percentage; the latest result summary; the student's courses; search |
| Scan and check-in | `POST /sessions/join`; error codes for expired token, not your class, ended |
| Quiz play | Student state per section 4.4; questions; results; session rank |
| Alerts | Notification feed, read state, mark all read |
| Profile | Name, avatar, class, school; edit display name and avatar; attendance and summary tiles |
| Attendance | Percentage and log: course, date, status (`present`, `late`, `absent`) |
| Summary | Latest session result: right and wrong per question |
| Leaderboard | Class leaderboard for the season, own row marked |
| Subject (course) | Teacher name, attendance in that course, recent results |
| Achievements | Badges with earned state |
| Settings | Notifications switch, language, version |
| FAQ, Welcome, QR kit | Static or demo only; no API |
| Documents | Teacher handouts. Not in the spec. No API until OPEN-18 is decided |
| Schedule, next-lesson card, scheduled pop quiz, streak badge | No API (owner decision 2026-10-18). To be removed or reworked |

Current demo behaviour worth keeping in the contract:

- A class QR is a link ending `/app/checkin/<code>`. With rotation, `<code>` becomes the rotating
  token. The phone's own camera opening that link must work.
- A link opened while signed out goes through sign-in (`/app/welcome?next=...`) and then checks
  in.
- Joining a quiz records attendance; the quiz opens at `/app/play/<code>`.
- Quiz stages in the demo: joined, lobby, question, reveal, between. Kinds built: four-option
  multiple choice and true/false. Numeric, opener vote and reason are not built yet.
- Demo scoring: 10 for a right answer plus up to 5 for speed; demo timers shortened to 20 s
  (multiple choice) and 15 s (true/false).
- Display name 2–24 characters; avatar from the enum in section 3.2.
- The app works offline after one visit (service worker). Answer queuing for live sessions is
  not built yet; the API must support it.
- Typing the code was removed from the scanner on 2026-10-13 (OPEN-03).

---

## 13. Non-functional requirements, assumptions and launch gates

### 13.1 Scale (estimates from the September architecture note)

- Up to about 35 students per session.
- A school of 50 teachers may run about 25 sessions at once at 09:00.
- National scale: a few thousand concurrent sessions. Any managed realtime service handles this.

### 13.2 Network

Students are often on mobile data inside concrete buildings. Keep payloads small, images
compressed and sized for phones, and never let an image delay a question.

### 13.3 Quality

- Automated tests for every acceptance criterion in section 10.
- Structured logs without personal data where possible; logs in the EU.
- Monitoring of realtime delivery and timer accuracy during school hours.

### 13.4 Launch gates (business, not code — but the code must respect them)

- **e-textbook rights.** Pre-loading national e-textbooks into a commercial product needs a
  written answer from МОН/БРО before the feature ships. Build ingestion behind a switch, off in
  production until cleared.
- **ДПА clauses.** Two new clauses are needed before launch: per-topic assessment data on minors,
  and anonymous questions stored with the sender's identity.
- **Age of digital consent** under ЗЗЛП: verify before launch (GDPR default 16; states may lower
  to 13).

### 13.5 Open assumptions (from the spec)

| # | Assumption | What breaks if wrong |
|---|---|---|
| A | Every pilot school issues student mailboxes | Student sign-in; nothing else works. Not yet checked |
| B | e-textbook rights can be cleared with МОН/БРО | The primary lesson input |
| C | A class gives up 45 minutes for a setup lesson | Adoption |
| D | The mandatory native lock does not trigger parent complaints | The native phase |
| E | Teachers and parents accept per-topic data being stored | The ДПА |
| F | Apple grants the Family Controls entitlement in time | iOS students in the native phase |

---

## 14. Open decisions

Ask before implementing any of these. Who decides: the product owner unless stated.

| ID | Question | Notes |
|---|---|---|
| OPEN-01 | How do students sign in with the school email: Google Workspace or Microsoft 365 sign-in, a one-time code emailed to the mailbox, or a password? | Depends on assumption A and the pilot school's mail provider |
| OPEN-02 | Is the teaching unit a course (class + subject + teacher), and who assigns teachers to classes and subjects: the admin? | This file assumes yes and yes |
| OPEN-03 | Can students type the short code? The spec shows one; the student app removed typing on 2026-10-13 | Without it, a student with a blocked camera cannot join |
| OPEN-04 | If signing in takes longer than the token's life, is the scanned token still honoured? | Trade-off: convenience against the rotation's purpose |
| OPEN-05 | What makes a join late: after the session leaves `lobby`, or a number of minutes after start? | Suggested default: after `lobby` |
| OPEN-06 | Absentee list format for copying into e-Дневник | Order, surname first or not, separators |
| OPEN-07 | Debate opener: does picking a side score? | Reason points apply either way |
| OPEN-08 | Practice set selection: one set for the class with every missed question, a threshold, or per student? Retakes? A deadline? | — |
| OPEN-09 | The badge list | The demo has First answer and Perfect quiz; the spec's examples: a reason on a wrong answer, a comeback, a good question. Streaks are out |
| OPEN-10 | Where does a student's edited display name appear, given the class leaderboard uses real names? | The demo shows the display name to classmates |
| OPEN-11 | Semester dates: national calendar or set per school? | Seasons follow the semester |
| OPEN-12 | Report send day, and what to send for a month with no lessons | — |
| OPEN-13 | Formula format: LaTeX with mhchem for chemistry, rendered on the client? | Recommendation, not decided |
| OPEN-14 | Ad-hoc question: which types, is there a correct answer, does it score? | — |
| OPEN-15 | Does v1 ship the English interface? The spec says Macedonian only; the demo has English | Backend impact small: a locale setting |
| OPEN-16 | Nudge interval ("several minutes") | — |
| OPEN-17 | AI provider and model | The September note recommended Anthropic Claude |
| OPEN-18 | Documents (teacher handouts) in the student app: build or drop? | Not in the spec |
| OPEN-19 | "Average engagement (answers per pop-up)" exact formula | For the report |
| OPEN-20 | Leaderboard ties | Shared rank or a tie-breaker |
| OPEN-21 | Delivery of the one-tap human channel: email, phone, chat? | — |
| OPEN-22 | Roster bulk import format | — |

---

## 15. Changelog

| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-10-18 | First version. Compiled from feature-spec-v1 (13 Oct), the pilot decisions (15 Oct) and the owner's answers (18 Oct): whole v1 in scope; the spec wins over the demo's timetable, next-lesson card, scheduled quiz and streak; stack recommendation Next.js with TypeScript, Laravel, PostgreSQL and a managed realtime layer, final choice with the backend developer |
