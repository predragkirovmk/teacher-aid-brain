---
created: 2026-09-12
type: area
status: draft
---

# Technical architecture

What the real product needs, as opposed to the demo ([[prototype]]). Written so a developer
joining after the weekend can start without a meeting.

## Shape

Three clients, one backend, one AI provider.

| Client | Form | Why |
|---|---|---|
| Student app | **Native** (Expo / React Native, or Capacitor around web views) | Focus lock needs OS APIs a web app cannot touch. QR deep link opens the app. Authorization is mandatory to join, so there is no web fallback and no gradual adoption — every participant installs. |
| Teacher app | Web (PWA, installable), run from a laptop in class | Planning at home, running the class from the laptop. No store review cycle for teacher features. |
| Headmaster | Email (PDF report) + a permanent private link to past reports | No login by design. |
| Class view | A **second browser window** the teacher opens onto the projector | The QR, the live bars, the reveal, the leaderboard. Separate from the console so the anonymous inbox and named data are never projected. Opening it must be one click, not a drag. |

Backend: one Node/TypeScript service (Next.js API or a small Fastify app), Postgres, a
realtime layer for the live session (managed WebSockets — e.g. Supabase Realtime, Ably, or
a small Socket.IO process), object storage for uploaded plans.

## Focus lock — the honest technical basis

This is the claim most likely to be challenged. The answer:

- **iOS:** Screen Time API — `FamilyControls` (authorization), `ManagedSettings` (shield
  apps/categories), `DeviceActivity` (time windows). Since iOS 16, apps can request
  *individual* authorization from the device owner (no parent/child pairing needed). Apple
  grants the Family Controls entitlement on request; consumer focus apps (Opal, one sec,
  Jomo) ship on exactly this.
- **Android:** `PACKAGE_USAGE_STATS` permission (usage access) to detect a foreground
  distraction app, plus a system overlay (`SYSTEM_ALERT_WINDOW`) to cover it, or an
  `AccessibilityService` to intercept. App blockers (AppBlock, Forest, StayFree) use this.
  Google Play policy requires declaring the use case; "productivity/focus" is accepted.
- **Authorization is mandatory.** A student who has not authorized cannot join a session.
  Revocation is detected and surfaces as a **focus flag**. A student who will not or cannot
  authorize takes the same path as a student with a dead battery — marked present, no points,
  reason shown on the dashboard. **Never marked absent:** a false absence over a phone
  setting is the incident that reaches a parent and ends a pilot.
- This replaces the earlier "not blocked, only flagged" design. It is a harder thing to
  justify to a parent, and the fallback above is what makes it justifiable.
- **School-owned devices** (if a school has them): Android Enterprise lock-task (kiosk) mode
  or iOS Guided Access / MDM make it absolute. Not the primary path (BYOD is).

## Live session

- Session = one lesson: `session_id`, class, teacher, state machine
  (`lobby → opener → teaching → review → ended`).
- Events: `join`, `vote`, `reason`, `popup_open`, `answer`, `question_anon`, `focus_lost`,
  `focus_back`, `reveal`, `end`.
- Scale per session is tiny (≤ 35 students). Scale across sessions: a school of 50 teachers
  runs ~25 sessions concurrently at 09:00. A country: a few thousand. Any managed realtime
  service handles this.
- Timer authority is the server (10–30 s pop-ups); clients render the countdown.

## AI

- **Recommendation: Claude Sonnet 5** for lesson generation (Macedonian quality, instruction
  following) with **prompt caching** of the system prompt + curriculum excerpt; **Claude
  Haiku 4.5** for cheap ad-hoc pop-ups and chatbot small talk. Cost math in
  [[cost-structure#AI inference]].
- Generation is one structured call: input = system prompt (role, format, Macedonian, the
  45-minute frame) + БРО curriculum excerpt for the subject/grade + teacher's text or the
  uploaded plan's relevant pages; output = JSON with opener, pop-ups (question, options,
  correct, explanation), timing, review summary. Structured output, validated before saving.
- Curriculum: the БРО programs are public documents per subject and year. Load once, chunk
  per unit, retrieve the unit the teacher picked.
- **e-учебници.** The national digital textbooks are pre-loaded and chunked per unit for the
  pilot subjects (history, electronics, physics), and picking a unit from one is the primary
  planning input — better grounded than three typed sentences. **Rights to use them are a
  blocking legal item** to settle with МОН/БРО before the feature ships; see
  [[feature-spec-v1]] 4.1.
- Together these are the moat-in-progress: a corpus of MK lessons and questions that improves
  with every edit teachers make, seeded per school by the shared lesson library.
- Safety: teacher-only exposure; generated content is always reviewed by the teacher before
  students see it. No student-facing generation in v1.

## Data model (minimum)

`schools`, `users` (teacher/admin), `classes`, `students` (name, class), `lessons` (plan JSON,
date, teacher, class), `sessions`, `attendance` (student, session, joined_at),
`answers` (student, question, choice, correct, ms), `reasons`, `anon_questions`,
`focus_events`, `points_ledger`, `reports` (monthly PDF per school).

## Offline and bad network

- Students on **mobile data** when school wifi fails (decided). The student app is light:
  events are small JSON messages.
- **Phones queue answers locally and sync on reconnect**, preserving the original answer
  time, so a student who answers during a 20-second dropout is still credited. The console
  must distinguish **offline** from **has not answered** — from the front of the room those
  look identical, and one of them is about to earn a focus flag nobody deserved.
- Teacher plan is cached on device; if the network dies mid-lesson the teacher still has the
  questions and can run them out loud. The session degrades to verbal, points pause.
- Rural coverage gaps are real; the private-school beachhead sidesteps them for the pilot.

## PWA and the house stack

The teacher web app follows the house defaults: Next.js, Tailwind, Postgres, PWA-first
(installable, offline shell), CSP and security headers, mobile-first. The student app is the
one deliberate exception to "web everywhere" — because of focus lock.

## Not decided

- Expo vs Capacitor for the student app (Expo recommended: better native module story for
  Screen Time / usage access).
- Realtime vendor.
- Where the monthly report is rendered (server-side PDF from HTML is enough).

## Related

[[feature-spec-v1]] — the v1 build spec, authoritative where it disagrees with this note ·
[[features]] · [[prototype]] · [[data-and-privacy]] · [[cost-structure]]
