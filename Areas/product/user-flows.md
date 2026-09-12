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
1. Receives an invite link from the school admin (or from the champion teacher).
2. Creates an account: name, subject(s), classes taught. Language: Macedonian.
3. Watches the 90-second "your first lesson" video, or skips it.
4. Adds one class: name (e.g. II-3), student count. Students are added by scanning, not by
   typing a list.

### Planning a lesson (5 minutes, the night before or in the staff room)
1. "New lesson" → picks the class and date.
2. Types three sentences, or uploads the plan and picks the unit.
3. Reads the generated opener and pop-ups. Edits inline, or asks the chatbot to change them.
4. Saves. The lesson shows in "Today".

### Running a lesson (45 minutes)
1. Taps "Start" → the QR appears on the projector (or on the teacher's phone, held up).
2. Watches the join counter. Taps "Open" when most are in — attendance is done.
3. Opener runs: the teacher moderates, reads a reason or two aloud, taps "Reveal".
4. Teaches. Taps "Pop-up" whenever it fits. Sees answers land in real time.
5. Glances at the anonymous inbox before the last five minutes.
6. Taps "Review": the app lists the most-missed questions and the anonymous ones.
7. Taps "End": points close, students see their rank, the lesson is archived.

### Afterwards (2 minutes, optional)
- Opens the class dashboard: attendance, who was flagged, points trend.

## Student

### First time (2 minutes)
1. Scans the QR on the board. The link opens the app store page or the installed app.
2. Installs the app (native, required for focus lock). Enters name and class once.
3. Authorizes focus mode once (iOS Screen Time / Android usage access). The screen explains
   in one sentence: "During a live lesson, apps on the distraction list are paused. You can
   turn this off any time — your teacher will see that you did."

### Every lesson
1. Scans the QR → joined, present, focus lock on.
2. Opener: taps an option, types one line, sends. Sees live bars and the reveal.
3. Pop-ups arrive with a timer. Taps an answer. Sees whether it was right and the points.
4. Anytime: "Ask the teacher anonymously" — types a question, sends.
5. End: sees points for the lesson, rank in class, rank in school, any reward unlocked.

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
