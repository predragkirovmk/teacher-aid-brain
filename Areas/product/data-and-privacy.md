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
| Anonymous questions | Review block | Teacher only, stored without student id |
| Focus events (left app / returned) | Focus flag | Teacher only |
| Teacher lesson plans, uploads | Planning | The teacher; school admin |

Not stored: grades, contact details of students, location, device identifiers beyond a
push token, anything from other apps on the phone. Focus lock never reads what the student
does in other apps — it only knows "TeacherAid is not in the foreground".

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
- The **leaderboard** is the sensitive bit: public ranking of minors. Default to nicknames
  on school and national boards; real names only inside the class view. Let the school
  choose.

## Retention

- Session-level data: current school year + 1, then aggregated and deleted.
- Monthly reports: kept (aggregate, no student names).
- A student who leaves the school: personal data deleted within 30 days on the school's
  request.

## Security basics

- Hosting in the EU. Encryption in transit and at rest. Role-based access: a teacher sees
  only their classes; a school admin sees the school; nobody at TeacherAid sees student data
  without a support ticket and logging.
- AI provider: student names are **not sent** to the AI — generation uses the topic and the
  plan, never the roster. Answers are aggregated before any analysis.

## e-Дневник

Official attendance and grades live in e-Дневник (МОН). TeacherAid does not replace it and
does not write to it in v1. A teacher records attendance there as before; TeacherAid gives
them the QR list to copy from. Integration is a roadmap item that depends on МОН — do not
promise it on stage.

## The parent question, in one breath

"The school owns the data, we process it under contract, we store the minimum — name,
attendance, points — nothing from the rest of the phone, and we delete it when the student
leaves. The teacher decides when phones are used, exactly as the Minister said in May."

## Related

[[features#Focus lock]] · [[technical-architecture]] · [[macedonian-education-system]] · [[qa-prep]]
