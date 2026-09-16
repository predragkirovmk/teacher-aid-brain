---
created: 2026-09-12
type: area
status: draft
---

# User flows

Three actors, three flows. Each step names the screen and the decision the user makes.
Where a step is a demo simplification, it says so.

## Teacher

### First time (10 minutes)
1. Receives an invite from the school admin, who has already created the classes and rosters.
2. Creates an account: email and password, name, subject(s). Language: Macedonian.
3. Is walked through building one real lesson end to end, inside the product — they finish
   holding a lesson ready for tomorrow, not having watched a video.

### Planning a lesson (5 minutes, whenever suits — there is no timetable)
1. "New lesson" → picks the class.
2. Picks the unit from the e-учебник; optionally adds one sentence.
3. Reads the generated opener and pop-ups. Edits inline, or uses a one-tap refinement.
4. Saves. The lesson waits until the teacher chooses to run it, for one class or several.

### Running a lesson (45 minutes)
1. Opens two windows on the laptop: the **class view** onto the projector, the **console** on
   the laptop screen. The console holds the anonymous inbox and named data and is never
   projected.
2. Taps "Start" → the QR appears on the class view, rotating every 30 seconds.
3. Watches the join counter. Taps "Open" when most are in — attendance is done.
4. Opener runs: the teacher moderates, reads a reason or two aloud, taps "Reveal".
5. Teaches. Taps "Pop-up" whenever it fits. Sees answers land in real time; the console
   nudges if a long gap passes with none fired. A bad question can be skipped outright.
6. Glances at the anonymous inbox before the last five minutes.
7. Taps "Review": the app lists the most-missed questions and the students' questions.
8. Taps "End" — nothing closes by itself. Points close, students see their rank, the app
   shows a copy-ready list of who was absent, the lesson is archived.
9. Optionally sends a practice set built from what this class got wrong.

### Afterwards (2 minutes, optional)
- Opens the class dashboard: attendance, who was flagged, points trend.

## Student

### First time — a setup lesson, one period, whole class together
1. Installs the app (native, required for focus lock).
2. **Signs in with their school email account.** No typed names: this is what stops a student
   claiming to be someone else, and it is what makes attendance trustworthy enough to
   forward.
3. Authorizes focus mode (iOS Screen Time / Android usage access). The screen explains in one
   sentence: "During a live lesson, apps on the distraction list are paused. Authorizing this
   is how you join — without it you can sit in the lesson, but you can't play or score."
4. The teacher is in the room for all of it, fixing the handful of phones that resist.

This costs one period per class, once. The alternative is spending the same time unplanned,
during the first opener, which is the moment the product is supposed to prove itself.

### Every lesson
1. Scans the QR → joined, present, focus lock on. (No authorization, no join — see above.)
2. Opener: taps an option, types one line, sends. Sees live bars and the reveal.
3. Pop-ups arrive with a timer — multiple choice, a number, or true/false. Sees whether it
   was right and the points. An answer given during a dropout is queued and still counts.
4. Anytime: "Ask the teacher" — types a question, sends. No classmate sees it; the teacher
   does, with their name.
5. End: sees points for the lesson, rank in class, rank in school, any badge unlocked.

### Between lessons
Opens the app to see their points, rank, badges, their own answer history — including what
they got wrong and why — and any practice set the teacher has sent.

### Demo simplification (Startup Weekend)
No install, no account: the judges scan a QR that opens a **web page**, get an auto-generated
nickname, tap Join, vote, type a reason, see the reveal and a mini leaderboard. Focus lock is
explained, not demonstrated. See [[prototype]].

## Headmaster

### Buying (private school, target: one meeting)
1. A teacher who has seen or run a free live session brings TeacherAid to the headmaster.
2. 20-minute meeting: the class flow, the monthly report sample, the price per teacher, the
   free first month.
3. Headmaster says yes; the school admin receives the invite links. The headmaster's
   involvement ends here.

### Every month
1. Receives the one-page report by email as PDF.
2. Forwards it. That is the whole flow.

### Renewal (month 12)
- The renewal email contains the year in numbers. The decision is made on the reports and on
  whether teachers ask to keep it.

## Related

[[product-overview]] · [[features]] · [[prototype]] · [[customer-segments]]
