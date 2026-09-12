---
created: 2026-09-12
type: area
status: draft
---

# Product overview — the 45-minute class

TeacherAid is a classroom system for secondary schools (гимназија, 15–18) in North
Macedonia. Three roles, one lesson:

- **Teacher** plans the lesson with AI in minutes and runs it from a phone or laptop.
- **Students** join by QR on their own phones and play the lesson: predict, answer, earn points.
- **Headmaster** does nothing inside the app and receives a monthly one-page report.

The unit of the product is the **lesson period** — 45 minutes in secondary school. Everything
is designed around that clock, not around "content" or "courses".

## The class, minute by minute

| Minute | What happens | Who acts | Feature |
|---|---|---|---|
| 0 | Teacher projects a QR. Students scan. | Students | [[features#QR join and attendance]] |
| 0–1 | Attendance is complete without a roll call. Focus lock starts. | App | [[features#Focus lock]] |
| 1–5 | **Opener.** A prediction or debate question generated from today's topic. Students vote and type a one-line reason. Results appear live. Being wrong is expected and rewarded with points for reasoning. | Students, teacher moderates | [[features#The opener]] |
| 5–40 | **Teaching.** The teacher teaches normally. Any time they want, one tap sends a pop-up question to every phone: 10–30 seconds to answer, points for correct and for fast. | Teacher triggers, students answer | [[features#Pop-up questions]] |
| 5–40 | Students can send an **anonymous question** the teacher alone sees. | Students | [[features#Anonymous questions]] |
| 40–45 | **Review.** The teacher goes through what the class got wrong and answers the anonymous questions. Points close. Rewards unlock. | Teacher | [[features#Points, rewards, leaderboards]] |
| after | Points roll into class, school and national leaderboards. Dashboard updates. | App | [[features#Teacher dashboard]] |

## Before the class — the teacher's five minutes

1. Open TeacherAid. Type three sentences: subject, topic, what matters today. Or upload the
   existing годишен/тематски план and pick the unit.
2. The AI returns: the opener question, 5–8 pop-up questions with answers, a suggested timing,
   and a short summary for the review block. All in Macedonian. All editable.
3. Optional: ask the teacher chatbot to adjust — "make it easier", "add a question about X",
   "make the opener a debate instead of a prediction".
4. Save. The lesson is ready to run.

## After the class — what everyone gets

- **Teacher:** attendance, per-student points and participation, who left the app (focus
  flags), the anonymous questions, and the lesson saved for reuse next year.
- **Student:** points, rank in class and school, in-app rewards.
- **Headmaster:** nothing until the end of the month, then the one-page report — lessons run,
  average attendance, engagement, top classes, top teachers. Written so it can be forwarded to
  parents, the municipality, or the local press without editing.

## Why this shape

- **Guess first, learn second.** Predicting before instruction improves later retention even
  when the guess is wrong. See [[learning-from-mistakes-research]]. The opener is not a warm-up;
  it is the pedagogical core.
- **Attention needs interruption.** Attention lapses in lectures start within minutes and
  recur; they drop during active tasks. Pop-ups at unpredictable moments keep the phone useful
  instead of hostile.
- **The phone is already in the room.** The teacher decides when phones are used
  ([[macedonian-education-system#Phone policy]]). TeacherAid is the sanctioned use.
- **The buyer never has to log in.** Headmasters buy outcomes and reputation, not software.
  A report they can forward is worth more to them than a dashboard they have to learn.

## What TeacherAid is not

- Not a learning management system. No homework, no grades, no content library (yet).
- Not a replacement for e-Дневник. Attendance stays in the official system; TeacherAid
  attendance is for the dashboard and the report ([[data-and-privacy]]).
- Not a student tutor. The chatbot is for teachers only.

## Related

- [[features]] · [[user-flows]] · [[technical-architecture]] · [[data-and-privacy]]
- Business side: [[business-model-canvas]] · [[value-propositions]]
