---
created: 2026-09-12
type: area
status: draft
---

# Data and privacy

Students are minors. This page is the answer to "what about the data?" — a question a judge,
a headmaster and a parent will each ask in their own words.

## What is stored (decided: minimum viable)

| Data | Why | Who sees it |
|---|---|---|
| Student name + class | Attendance, leaderboard | Teacher, the student, classmates (leaderboard shows names or nicknames — school chooses) |
| Attendance per session (joined_at) | Dashboard, monthly report | Teacher; aggregated for headmaster |
| Answers, points, reasons | The game, review block | Teacher; the student sees their own |
| **Per-topic correctness per student, over time** | The teacher sees who is repeatedly wrong on a topic | Teacher; the student sees their own |
| **Practice set completion and scores** | Voluntary practice after a lesson | Teacher; the student sees their own |
| Anonymous questions | Review block | Teacher, **with the sender's identity** — see below |
| Focus events, and whether focus lock is authorized | Focus flag; joining requires authorization | Teacher only |
| Teacher lesson plans, uploads | Planning | The teacher; other teachers in the same school via the lesson library |

Not stored: grades, contact details of students beyond the school email address, location,
device identifiers beyond a push token, anything from other apps on the phone. Focus lock
never reads what the student does in other apps — it only knows "TeacherAid is not in the
foreground".

### Two things this note used to say, and no longer does

- **Anonymous questions are anonymous to classmates, not to the teacher.** The sender's
  identity is stored and shown to their teacher. The fear the feature is designed around is
  peer judgement — the student who will not raise a hand because thirty people are watching.
  A side effect is that there is no anonymous channel for abuse. This must be stated in the
  ДПА and in the parent notice; it must not be described to students as anonymous full stop.
- **Per-topic correctness is stored per student.** This is, in substance, assessment data
  about a minor. TeacherAid still does not grade anyone — the teacher decides what counts —
  but the app now holds the data a grade could be built from, and that distinction has to be
  made deliberately rather than assumed. It needs its own ДПА clause and its own sentence in
  the parent notice.

## Legal structure (the standard edtech setup)

- The **school is the data controller** — it already processes attendance and participation
  under its legal mandate. **TeacherAid is the processor**, under a data processing agreement
  (ДПА) with each school. This is the structure that lets a school adopt the tool without a
  parental consent campaign for the core data.
- Law: Закон за заштита на личните податоци (ЗЗЛП, 2020), aligned with GDPR. Applies as GDPR
  would: lawful basis, minimization, retention limits, breach notification, DPO where
  required. **Verify** the exact age of consent for digital services in ЗЗЛП before launch
  (GDPR default is 16; member states may lower to 13).
- Parents are informed by the school through the usual channel (a one-page notice). Where a
  school wants explicit consent — private schools often will — a consent form template is
  provided.
- The **leaderboard** is the sensitive bit: ranking of minors. Default to nicknames on the
  school board; real names only inside the class view. Let the school choose. There is no
  national board in v1.

## Retention

- Session-level data: current school year + 1, then aggregated and deleted.
- Monthly reports: kept (aggregate, no student names).
- A student who leaves the school: personal data deleted within 30 days on the school's
  request.

## Security basics

- Hosting in the EU. Encryption in transit and at rest. Role-based access: a teacher sees
  only their classes; a school admin sees setup and school-wide usage, never an individual
  student's answers.
- **TeacherAid staff have no standing access to any school's data.** Access is granted per
  support incident, is time-limited and expires automatically, is written to an audit log,
  and **the school admin can see every occurrence**. This is the mechanism behind the
  promise; it is also the most convincing single thing to show a headmaster on the data
  question.
- **Data subject requests are handled manually**, by support, on the school's written
  request — export everything held on one student, or delete it. There is no self-serve
  path for the admin in v1, which means the ЗЗЛП response clock depends on a person being
  available. Track every request somewhere durable from the first one.
- AI provider: student names are **not sent** to the AI — generation uses the topic and the
  plan, never the roster. Answers are aggregated before any analysis.

## e-Дневник

Official attendance and grades live in e-Дневник (МОН). TeacherAid does not replace it and
does not write to it in v1. A teacher records attendance there as before; TeacherAid gives
them the QR list to copy from. Integration is a roadmap item that depends on МОН — do not
promise it on stage.

## The parent question, in one breath

"The school owns the data, we process it under contract, and we store what the lesson
produces — who was there, what they answered, which topics they are getting wrong — and
nothing from the rest of the phone. Questions a student sends go to their teacher, not to
the class. We delete it when the student leaves. The teacher decides when phones are used,
exactly as the Minister said in May."

This is longer than the old version because the old version was no longer true. Per-topic
data and teacher-visible questions both have to be said out loud rather than discovered by
a parent later.

## The part that needs a lawyer before the pilot

1. The ДПА clause for **per-student assessment data** (above).
2. The ДПА clause and parent-notice wording for **teacher-visible "anonymous" questions**.
3. **Rights to the e-учебници.** Pre-loading МОН-published textbooks into a commercial
   product is a separate question from personal data, and it is blocking — see
   [[feature-spec-v1]] section 4.1.

## Related

[[feature-spec-v1]] · [[features#Focus lock]] · [[technical-architecture]] ·
[[macedonian-education-system]] · [[qa-prep]]
