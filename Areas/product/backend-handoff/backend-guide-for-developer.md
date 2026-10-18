---
created: 2026-10-18
updated: 2026-10-18
type: area
version: 1.0
status: current — backend handoff for the developer
audience: the backend developer
companion: backend-spec-for-agents.md (the full version, for coding agents)
---

# TeacherAid backend — developer guide

Version 1.0 · updated 2026-10-18

## Start here

This is the short version of what the TeacherAid backend has to do. It covers the seven things
you need to start: the product, the user stories, the user flows, what to build, the rules, the
edge cases and how we will know it works.

There is a longer companion file, `backend-spec-for-agents.md`. It has the same content in full
detail (data model, every rule with an ID, every acceptance test, a proposed endpoint list) and
is written for coding agents. Put it in your repository and point your agents at it. When the
two files disagree, the agent file is the precise one; tell us, because it means one of them
needs fixing.

Three labels appear below:

- **Decided** — build it as written.
- **Proposed** — a starting value nobody signed off. Keep it in config so it can change.
- **Open** — not decided yet. Don't guess; ask us. The list is at the end.

What is already built: a student app demo (mobile web app, installable, works offline) at
https://teacher-aid-bitola.vercel.app/app. It runs on fake data in the browser. It shows how the
student side should behave, but its data is invented, so don't copy its numbers, subjects or
classmates. Nothing on the teacher side is built yet.

---

## 1. The product and the business

**TeacherAid is an AI classroom system for secondary schools in North Macedonia.** It is built
around one 45-minute lesson:

1. The teacher prepares a lesson in minutes: picks a unit from the national e-textbook, and AI
   writes an opening question, 5 to 8 quiz questions and a short review.
2. In class the teacher projects a QR code. Students scan it with their own phones. Scanning
   is the attendance.
3. The lesson opens with a prediction or debate question. Students vote and write one line of
   reasoning. Being wrong but giving a reason still earns points: that is the core idea.
4. While teaching, the teacher taps once to send a quick question to every phone, with a timer.
5. Students can send a question the class never sees as theirs. The teacher sees who sent it.
6. At the end the teacher gets only the names of absent students (to copy into e-Дневник, the
   official register) and the questions the class got wrong most.
7. Once a month the headmaster gets a PDF report by email. The headmaster never logs in.

**Who it is for.** Secondary schools (гимназија, vocational and arts schools). The pilot
subjects are history, electronics and physics. The interface is Macedonian.

**Who pays.** The school. Pricing is still being worked out, so there is no billing, no plans
and no payments to build.

**The people in the system.**

| Who | How they get in | What they do |
|---|---|---|
| Student | School email account | Joins lessons, answers, sees own points and results |
| Teacher | Email and password | Prepares and runs lessons, sees own classes only |
| School admin | Email and password | Sets up classes, students and teachers; sees usage, not answers |
| Headmaster | No account | Gets the monthly report by email |
| TeacherAid staff | Internal | Onboards schools, loads e-textbooks, handles support |

**What we are not building in v1.** Universities, lab or practical lessons, a national
leaderboard, streaks, a teacher chatbot, e-Дневник sync, an Albanian interface, timetables or
schedules, a parent app, extra time per student, self-serve data export, moving a class to
another teacher, co-teaching, and billing.

The student demo shows a timetable, a next-lesson card, a scheduled quiz and a streak badge.
**These are not in the product** (decided 18 Oct 2026). Don't build an API for them; those
screens will change when we connect the app to your API.

**Stack.** Your choice. What we need, whatever you pick:

- Hosting in the EU, for all personal data, backups and logs.
- A relational database (Postgres-style).
- Realtime push for live lessons (polling is not enough for timers and the teacher's screen).
- The server runs every timer, not the phones.
- Phones can send answers they saved while offline, and the server accepts them.
- Encryption in transit and at rest.

Our recommendation: Next.js with TypeScript on the frontend (the student app already uses it),
a **Laravel** API, **PostgreSQL**, and a managed realtime layer (Laravel Reverb, or a service
like Pusher or Ably).

---

## 2. User stories

The main ones. The full list, with links to requirements, is in the agent file.

**Teacher**

- I log in with email and password, and can reset my password fast from my phone.
- I pick subject, year and unit, add one optional sentence, and get a full lesson I can edit.
- With no e-textbook for my subject, I type three sentences instead.
- I adjust the lesson with one tap: easier, harder, more questions, debate instead of
  prediction, shorter timers.
- I reuse one lesson for several classes, and copy lessons other teachers in my school made.
- During the lesson I see who joined, who is late, who is offline and who hasn't answered.
- I send a quick question with one tap, send an unprepared one, or skip a bad one.
- I read anonymous questions with the sender's name, on my screen only.
- I mark a student without a working phone as present in one tap.
- I press End and get only the absent names.
- I send an optional practice set made from what the class got wrong.
- I see each class's attendance, points, focus flags and weak topics.

**Student**

- I sign in with my school email once, so nobody can pretend to be me.
- I scan the QR and I'm in the lesson and marked present.
- I vote, write one reason, and get points for the reason even if I'm wrong.
- If my signal drops while answering, my answer still counts.
- I send a question nobody in class knows is mine.
- I see my points, rank, badges, attendance and results.
- I change my display name and avatar.

**School admin**

- I set up classes and students, add, remove and move students, and invite teachers.
- I see which teachers use TeacherAid and which don't.
- I see every time TeacherAid staff looked at our data.

**Headmaster**

- I get a monthly report I can forward to parents or the municipality without editing.

---

## 3. User flows

### Preparing a lesson

Teacher picks subject → year → unit → optional sentence → AI writes the lesson (Macedonian) →
teacher edits or taps a refinement → saves. Saving creates a version; past lessons keep the
version they ran.

The AI never receives student names or any class list.

### Running a lesson (the main flow)

1. Teacher starts a session from a lesson for one class. It opens in the **lobby**.
2. Teacher opens the **class view** in a second window and projects it. It shows the QR, a short
   code, the questions, live bars and the leaderboard. It never shows private data.
3. The teacher keeps the **console** on the laptop screen: names, who answered, anonymous
   questions with senders, focus flags, all controls. It is never projected.
4. Students scan and join. The QR changes every 30 seconds, so a photo sent home stops working.
5. **Opener**: students vote, then write a reason, then the answer is revealed.
6. **Teaching**: the teacher fires quick questions. Each lands on every phone with a timer the
   server runs. When time is up the teacher reveals the answer.
7. **Review**: the most-missed questions, worst first, and the anonymous questions.
8. **End**: only the teacher can end it. Points settle, attendance is saved, absent names appear.

Session states: `lobby → opener → teaching → review → ended`. The teacher moves it forward.

### A student joining

Scan the QR (in-app scanner or the phone's own camera; the QR is a link) → sign in if needed →
the server checks the code is current, the lesson hasn't ended and the student belongs to that
class → marked present (or late) → in the lesson. Scanning twice changes nothing. Checking in
earns no points.

### A student answering

Question arrives with its end time → student answers → the server checks it's on time and the
first answer → stores it → when the question closes, the student sees right or wrong, points and
rank. If the signal drops, the phone keeps the answer and sends it later; it counts with the time
the student actually answered.

### No working phone

Teacher taps the student and picks a reason (no phone, battery, no data, no focus permission) →
present, no points, the reason shows on the dashboard. **Never absent.**

### After the lesson

Optional practice set built from the lesson's missed questions (no AI) → students get an alert →
voluntary, earns points → the teacher sees who did it.

### Monthly report

Automatic job → two-page PDF → email to the headmaster with a permanent private link to all past
reports.

---

## 4. What to build

**Accounts**

- Teacher and admin login with email and password; fast password reset by email.
- Student sign-in with the school email account (exact method is open).
- One active session per student: signing in on a new phone signs out the old one.
- Permissions as in the table in section 1: teachers see only their own classes; admins never
  see answers.

**Lessons**

- Load and split the e-textbooks per unit (staff tool; teachers never upload them).
- AI generation from a unit plus one sentence, or from three typed sentences.
- Structured output, checked before saving: opener, 5–8 questions with answers, explanations
  and timers, and a review summary.
- Six one-tap refinements. No chat.
- Versions: editing a lesson never changes past results.
- Images (with small versions for phones) and formulas in questions.
- A school library: teachers copy each other's lessons inside the same school.

**Live lesson**

- Sessions with the five states, moved by the teacher.
- Realtime channels for the class view (no private data), the console (everything) and each
  student (their own results).
- Rotating QR token and short code, every 30 seconds.
- Join, which is attendance (present or late).
- Opener votes and reasons; quick questions; unprepared questions; skip.
- Answers for four question types: multiple choice, true/false, a number within a tolerance,
  and the one-line reason after the opener vote.
- Offline answers accepted with their real time; no duplicates.
- Online, offline and not-answered shown separately on the console.
- Anonymous questions, 3 per student per lesson.
- A quiet nudge on the console if the teacher hasn't sent a question for a while.
- End, the absent list and the review.
- A way to find and end a lesson someone forgot to end.

**Focus**

- For the pilot: the student app reports when a student leaves the app during a question; the
  console shows a flag. Nobody is blocked.
- Later, a native app will lock other apps during class. Plan the data for it now.
- A school-wide list of apps that count as distractions, editable by the admin.

**Student side**

- Points, rank, badges, attendance, results, weak topics.
- Profile: display name and avatar.
- An alerts feed (results ready, practice sent, marked late, badge earned), plus web push.

**Teacher dashboard** — per class: attendance, points over time, focus flags, weak topics per
student, anonymous questions, practice results, lesson history.

**School admin** — classes, rosters, teachers, the distraction list, a usage overview, resend
the report, the log of staff access.

**Headmaster report** — monthly PDF by email plus a permanent private link.

**Support** — a one-tap way for a teacher to reach a human.

**TeacherAid staff** — onboarding schools, loading books, time-limited support access, handling
export and deletion requests.

---

## 5. The rules

| Rule | Value | Status |
|---|---|---|
| QR code changes | Every 30 seconds | Decided |
| Who can join | Only students of that class, signed in with the school email | Decided |
| Points for checking in | None | Decided |
| Where points come from | Answers (opener, quick questions, practice) and badges | Decided |
| Opener: right vote / wrong vote / reason written | 10 / 0 / 8 | Proposed |
| Quick question right | 10, plus up to 5 for speed | Proposed |
| Speed bonus | Bigger the faster you answer; the teacher can turn it off for a class | Formula proposed; switch decided |
| Practice question right | 5 | Proposed |
| Badge | 20 points | Proposed |
| Point values | The same in every school; teachers can't change them | Decided |
| A wrong answer with a reason | Must score close to a right answer without one | Decided |
| Rank in a lesson | Everyone starts each lesson at 0; lesson points add to the semester total | Decided |
| Timers | Recall and true/false 15 s; multiple choice 25 s; numbers 45 s; opener vote 30 s then reason 45 s | Proposed |
| Who runs the timer | The server | Decided |
| Answers | One per question, final once sent | Decided |
| Skipped question | Nobody scores; the class sees it doesn't count | Decided |
| Anonymous questions | The class never sees who; the teacher always does; 3 per student per lesson | Decided |
| Ending a lesson | Only the teacher; never automatic | Decided |
| Phone, battery, data or permission problem | Present, no points, reason shown; never absent | Decided |
| Class leaderboard | Real names | Decided |
| School leaderboard | Nicknames by default | Decided |
| Leaderboard season | One semester | Decided |
| Student joining mid-year | Starts at 0, marked as joined partway | Decided |
| Student moving class | History moves with them | Decided |
| Lesson edits | Only affect future runs | Decided |
| Lesson library | Same school only; copies never change the original | Decided |
| AI content | The teacher reviews everything before students see it | Decided |
| Data kept | Current school year plus one, then aggregated and deleted | Decided |
| Student who leaves | Deleted within 30 days of the school's request | Decided |
| TeacherAid staff access | Only per support case, time-limited, logged, visible to the school | Decided |
| Sent to the AI | Never student names or class lists | Decided |
| Not stored | Grades, student contact details beyond the school email, location, device IDs beyond a push token, anything about other apps | Decided |
| Hosting | EU | Decided |

---

## 6. Edge cases

- **A photo of the QR sent home.** The code is already expired. Reject it.
- **Scanning another class's code.** Reject: "not your class".
- **Scanning twice or refreshing.** Nothing changes; still one attendance.
- **Signed out when scanning.** Sign in, then continue joining with the same code.
- **Joining after End.** Reject.
- **Signal drops mid-question.** The phone keeps the answer and sends it later; it counts with
  the real answer time, as long as it was in time and arrives before End.
- **The same answer sent three times.** Stored once.
- **Offline is not the same as not answering.** The console must tell them apart, so nobody gets
  an unfair focus flag.
- **The teacher's internet dies.** The lesson keeps its place; the teacher reads questions aloud;
  points pause; the console catches up on reconnect.
- **Wrong phone clock.** Use the server's time everywhere.
- **Timer runs out while an answer is on its way.** The server decides by when it was answered.
- **A slow image.** Never delay the question or the timer for it.
- **The teacher forgets to press End.** The lesson stays open; show it, and let them end it from
  anywhere.
- **A bad question.** Skip it. No editing during class; fix it afterwards.
- **The teacher closes the console window.** The lesson continues; reopening restores it.
- **A fourth anonymous question.** Reject with a clear message.
- **A student leaves the app during a question.** Flag it on the console; don't block them.
- **A phone call during a question.** Flagged like any exit; the teacher judges.
- **A number typed with a comma (0,2).** Accept commas and dots.
- **No e-textbook for the subject.** Use the three-sentence path.
- **AI returns something broken.** Don't save it; show a clear error.
- **Signing in on a second phone.** The first phone is signed out and told why.
- **A school email that isn't on any class list.** Refuse and say "ask your school".
- **A school without student email accounts.** Nobody can sign in. This is our biggest unchecked
  assumption.
- **A teacher goes on long leave.** No solution in v1. Don't build one.
- **Nothing was missed in a lesson.** No practice set to send.

---

## 7. How we'll know it works

These are the checks we will run. Each one is written in full, as a test, in the agent file.

- A teacher can log in, and reset a password from a phone.
- A student signed in on a second phone signs out the first.
- Scanning the current code marks the student present, and the teacher's screen shows it at once.
- An expired code, another class's code and a code after End are all rejected.
- Scanning twice gives one attendance record and no points.
- A student who never joined is in the absent list after End; a student marked "battery" is not.
- The class view never shows the sender of an anonymous question or the author of a reason.
- Each quick question closes at the time the server set, the same for everyone.
- A right answer with half the time left scores 13 (10 plus 3); with the speed bonus off, 10.
- A number within the tolerance is right; one outside it is wrong; "2601,0" reads as 2601.
- A second answer to the same question is refused.
- An answer saved offline at second 8 and sent at second 40 counts with second 8.
- A skipped question gives nobody points.
- A wrong opener vote with a reason scores 8; a right vote without one scores 10.
- A fourth anonymous question is refused.
- Leaving the app during a question raises a flag; leaving between questions doesn't.
- End settles points, saves attendance and returns only absent names.
- Generation returns one opener and 5–8 valid questions in Macedonian, and nothing sent to the AI
  contains a student's name.
- Editing a lesson after running it doesn't change the earlier class's results.
- A copied lesson can be edited without touching the original; other schools can't see it.
- A school admin can't read any student's answers.
- A teacher can't open another teacher's class.
- Every staff access is logged, visible to the school, and stops at expiry.
- The headmaster gets the two-page PDF and a link to all past reports, and a changed link shows
  nothing.

---

## Open questions

We still have to decide these. Ask us before building them; the agent file has more detail.

1. **How students sign in with the school email**: Google or Microsoft sign-in, a one-time code
   by email, or a password. Depends on what the pilot school uses.
2. **Class, subject and teacher**: we assume a "course" is one class with one subject and one
   teacher, and that the admin assigns them.
3. **Typing the short code**: the spec shows one; the student app currently has no way to type it.
4. **A slow sign-in after scanning**: does the scanned code still work if signing in takes longer
   than 30 seconds?
5. **What counts as late.**
6. **The absent list format** for copying into e-Дневник.
7. **Debate opener**: does picking a side score?
8. **Practice sets**: which questions, retakes, a deadline.
9. **The list of badges.**
10. **Display names**: where an edited name shows, since the class leaderboard uses real names.
11. **Semester dates**: national or per school.
12. **Report day**, and what to send for a month with no lessons.
13. **Formula format** (we suggest LaTeX).
14. **Unprepared questions**: types and scoring.
15. **English interface**: the spec says Macedonian only; the demo has English.
16. **Nudge timing**: how long without a question before the console nudges the teacher.
17. **Which AI provider and model.**
18. **Documents** (teacher handouts) in the student app: keep or drop.
19. **The engagement formula** in the report.
20. **Ties** on the leaderboard.
21. **How the "reach a human" button reaches us.**
22. **The format** for importing class lists.

Business items that block launch, not your code: permission from МОН/БРО to load the
e-textbooks, two new clauses in the data agreement with schools, and checking the age of digital
consent in Macedonian law. Build the e-textbook loading behind an on/off switch, off until we
have the permission.

---

## How this guide stays current

Both files change whenever a product decision changes, in the same update. The version and date
are at the top; the history is below. If something here looks wrong or out of date, tell us
before building on it.

| Version | Date | Change |
|---|---|---|
| 1.0 | 2026-10-18 | First version |
