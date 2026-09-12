---
created: 2026-09-12
type: area
status: draft
---

# Features

Each feature: what it does, why it exists, what is decided, what is still open. Open items are
also collected in [[risks-and-assumptions]].

## QR join and attendance

- Teacher starts the lesson; the app shows a QR (projector or teacher's phone) plus a short
  code for students whose camera will not scan.
- One scan = joined today's session + marked present. No roll call.
- Late students scan late; the timestamp is recorded.
- **Decided:** one QR per lesson; join and attendance are the same action.
- **Open:** whether the QR should rotate every 30 s to stop students scanning a photo from
  home. Cheap to add; decide after the pilot.
- Attendance is **separate from e-Дневник** — the teacher still records official attendance
  there. TeacherAid attendance feeds the dashboard and the headmaster report. Sync is a
  roadmap item, not a promise. See [[data-and-privacy]].

## Focus lock

- During a live session the student app blocks apps on a distraction list (social media,
  games, messaging) and shows the teacher who left the app.
- **Decided:** this is a real feature, not a slogan.
- **How it is technically possible on personal phones** (the answer for any judge who asks):
  - iOS: the Screen Time API (FamilyControls + ManagedSettings + DeviceActivity). Since iOS 16
    a third-party app can request *individual* authorization from the device owner and then
    shield chosen apps for a time window. This is the mechanism focus apps such as Opal and
    one sec use. Requires Apple's entitlement approval.
  - Android: usage-access permission plus an overlay, or an accessibility service — the
    mechanism app blockers such as AppBlock and Forest use.
  - Both require the student to authorize once at install. A student who revokes it is not
    blocked — but the teacher sees a **focus flag** on the dashboard. Social pressure and
    points do the rest.
  - Consequence: the **student app must be native** (or a native shell around web views).
    The teacher side can be web. See [[technical-architecture]].
- **Open:** exact distraction list; whether the school can customize it.

## The opener

- Minutes 1–5. One question generated from today's topic, in one of two modes:
  - **Prediction:** "What will happen if…?" with 3–4 options. Students vote, then type one
    line of reasoning.
  - **Debate:** a claim; students pick a side and give one reason. The teacher reads two
    reasons out loud and lets the class argue for two minutes.
- Live results on the projector: bars, then the reveal.
- Points: being right earns points; **giving a reason earns points whether right or wrong**.
  This is deliberate — the pedagogy is productive failure and the pretesting effect
  ([[learning-from-mistakes-research]]); the incentive must reward committing to a guess.
- **Decided:** vote + one-line reason on phones.
- **Open:** whether reasons are shown anonymously to the class (default: only the teacher
  sees names).

## Pop-up questions

- Minutes 5–40. The teacher taps once; a question lands on every phone with a 10–30 s timer.
- Generated with the lesson plan (5–8 per lesson), editable, and the teacher can also fire an
  ad-hoc one ("quick: yes or no?") without preparation.
- Points: correct + speed bonus. Wrong answers are collected for the review block.
- **Decided:** teacher-triggered, not on a fixed schedule — unpredictability is the point.

## Anonymous questions

- Any time during the lesson a student can send a question only the teacher sees.
- The teacher answers them in the last five minutes, or later.
- Why: the questions nobody asks out loud are usually the ones half the class has.
- **Open:** rate limit per student to prevent spam (default: 3 per lesson).

## Points, rewards, leaderboards

- Points per lesson: opener (right / reasoned), pop-ups (right / fast), participation.
- Rewards: in-app (badges, streaks, rank). **At the teacher's discretion, points inform the
  participation grade** — the app shows the numbers, the teacher decides how they count.
  This framing matters for parents and for МОН: TeacherAid never grades anyone.
- Leaderboards: class, school, national. School vs school is the retention hook and the
  network effect — every new school makes the game bigger for the existing ones.
- **Open:** season length (semester?) and whether national boards are per subject.

## Teacher planning with AI

- Input: three typed sentences and/or an uploaded plan (годишен/тематски план, PDF/DOCX).
- Output: opener, pop-up questions with answers, timing suggestion, review summary — in
  Macedonian, editable.
- The **teacher chatbot** refines the plan conversationally. It is for teachers only; there is
  no student-facing AI.
- Curriculum: the БРО programs for secondary subjects are loaded as reference so the AI
  stays inside what is actually taught. See [[technical-architecture#AI]].

## Teacher dashboard

- Attendance per lesson and per student.
- Per-student points and participation over time.
- Focus flags (who left the app and when).
- Anonymous questions inbox.
- Lesson history: every generated plan, reusable next year.
- Simple by design: one screen per class, nothing to configure.

## Headmaster report

- Monthly, automatic, one page, PDF and email.
- Lessons run, teachers active, average attendance, average engagement (answers per pop-up),
  top classes, top teachers, a two-line summary in plain language.
- Written to be forwarded: to parents, to the municipality, to the local press. This *is*
  the headmaster's product.
- **Decided:** the headmaster has no login and nothing to configure.

## Onboarding and support

- In-app tutorials and short videos; a "first lesson in 10 minutes" path for teachers.
- In-app help plus the teacher chatbot as first-line support.
- No in-person onboarding in the base offer (scales), but the **free live session** in a
  school is the sales stunt — the team runs one real class to show it works.

## Not in v1

- Homework, grading, content library, parent app, Albanian-language UI, e-Дневник sync,
  primary-school mode. All are roadmap; none are promised in the pitch.

## Related

[[product-overview]] · [[user-flows]] · [[technical-architecture]] · [[data-and-privacy]]
