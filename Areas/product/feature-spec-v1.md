---
created: 2026-09-15
type: area
status: draft — not yet integrated into the vault
---

# Feature specification — v1

## 1. What this note is

The build specification for TeacherAid version one, written for the developer who will
build it. It says what exists, how it behaves, and what happens when it fails.

**These decisions are current.** Where this note and an older product note disagree, this
note is right. The older notes — [[features]], [[product-overview]], [[user-flows]],
[[technical-architecture]], [[data-and-privacy]] and the handbook — were written in
36 hours for a pitch and describe a product shaped for a five-minute stage slot. Section 15
lists every place they now need updating.

[[features]] stays as the short ten-heading overview. This note is the depth behind it.

Two conventions used throughout:

- **Cost of this decision** — one line where a choice carries a price worth remembering.
  Not an argument against it; a note so nobody rediscovers it during a pilot.
- **(verify)** — a figure or fact nobody has checked yet.

---

## 2. Scope of v1

**In:** secondary schools in North Macedonia — гимназија, стручно and уметничко.

**Pilot subjects:** history, electronics, physics. These three have their curriculum and
e-учебници loaded properly. Any other subject works from the teacher's own input with no
curriculum grounding.

**Interface language:** Macedonian only.

**Out of v1, explicitly:**

| Not in v1 | Why |
|---|---|
| Universities | No headmaster, no attendance mandate, lectures of 90+ minutes, halls far beyond the 35-student session assumption, and students who are legal adults — so mandatory focus lock has no institutional mandate behind it. A second product shape. |
| Practical / лабораториска настава | A 20-second timed question does not fit a student with a soldering iron in both hands. Theory periods only. |
| National leaderboard | Meaningless across three pilot schools. |
| Streaks | Not proven with a real class yet. |
| Open-ended teacher chatbot | Replaced by fixed one-tap refinements (section 4.6). |
| e-Дневник sync | Depends on МОН. Never promise it. |
| Albanian-language interface | After the Macedonian corpus is proven. |
| Timetable / periods | Deliberately rejected. See 5.1. |
| Parent app or parent login | Parents are reached by the school, on paper. |
| Per-student extra time | See 10.4, recorded as a known limitation. |
| Self-serve data export and deletion | Manual, by support. See 11.5. |
| Reassigning a class to another teacher | No answer in v1 for maternity leave, illness or a teacher leaving. |
| A second teacher on one class | No co-teaching, no substitutes. |
| In-app billing | Pricing is being reconsidered. Out of scope entirely. |

> **Cost of this decision — practical lessons.** In a technical school a large share of
> periods are практична настава. Excluding them structurally halves lessons-per-teacher,
> which is the retention metric [[channels-and-relationships]] says to intervene on below
> two per week. Expect pilot usage numbers to look worse than the product deserves, and
> measure per *theory* period rather than per period.

> **Cost of this decision — pilot subjects.** History, electronics and physics point at a
> стручно or technical school. Every business note in this vault assumes a private
> гимназија beachhead. One of the two is out of date; that is a business decision, not a
> product one, but the build should not assume гимназија.

---

## 3. Roles and accounts

Three accounts and one non-account.

### 3.1 Teacher — email and password

Logs in with an email address and a password. No magic links, no SSO.

> **Cost of this decision.** [[channels-and-relationships]] identifies 08:05 as the moment
> that matters and prescribes magic links so there is nothing to forget. Passwords
> guarantee that support call. Build password reset properly and make it fast on a phone.

Can: create and edit lessons, run sessions, see their own classes only, view their
dashboard, send practice sets, read the anonymous inbox for their own sessions.

Cannot: see another teacher's classes, students or results; see anything school-wide;
change rosters (see 3.3).

### 3.2 Student — school email account

Logs in with the school-issued email account. This is the identity for attendance, points,
focus flags and per-topic results.

The reason is impersonation: a student cannot claim to be another student. This replaces
the "type your name and class once" design in [[user-flows]], which had nothing stopping a
fake name, a duplicate, or joining the wrong class — and attendance built on that could not
be trusted or forwarded.

One active session per account at a time. A second device signing in ends the first.

**This is the single largest unverified assumption in the spec.** If a school does not
issue student mailboxes, no student can log in and nothing else works. See section 14.

Can: join a session they are enrolled in, answer, send an anonymous question, see their own
points, rank, badges, answer history and practice sets.

Cannot: see another student's answers, reasons, or results; see who sent an anonymous
question; see the teacher dashboard.

### 3.3 School admin — a real role with screens

The school secretary or IT person. Created by TeacherAid at onboarding.

Can: create and edit classes and rosters, invite and remove teachers, move a student
between classes, add or remove a student mid-year, adjust the distraction list (6.3), see
the school usage overview (9.2), resend the headmaster report, see the staff-access log
(11.4).

Cannot: see individual student answers, reasons, anonymous questions, or per-topic results.
The admin sees *whether* teaching is happening, not *what* any child answered.

### 3.4 Headmaster — no account

Receives the monthly report by email. Has no login, nothing to configure and nothing to
learn. This is decided and stays decided.

---

## 4. The lesson

A lesson is a reusable object. It is authored once and can be run many times.

### 4.1 Creating a lesson — the e-учебник path

The primary input is **the unit from the e-учебник**, not three typed sentences.

1. Teacher picks subject, year and unit from the pre-loaded e-учебник contents. Two taps.
2. Optionally adds one sentence of emphasis — "focus on the exam question", "they struggled
   with this last year".
3. Generation returns: the opener, 5–8 pop-up questions with answers and explanations, a
   suggested timer per question, and the review summary. In Macedonian. All editable.
4. Teacher reviews, edits inline or uses a refinement (4.6), saves.

TeacherAid pre-loads and chunks the e-учебници for the three pilot subjects, per unit.
Teachers do not upload them.

**"Type three sentences" is now the fallback path**, used when there is no loaded book for
that subject, or the teacher wants something off-book. It still works. It is no longer the
headline.

> **Blocking legal item.** The national e-учебници are published under МОН and written by
> named authors. Pre-loading them centrally into a commercial product is a rights question
> that must be resolved with МОН/БРО **before this feature ships** — alongside the ДПА and
> the privacy notice already budgeted for a lawyer in [[cost-structure]]. This is not a
> launch-and-see item. It is the kind of thing that becomes a headline if discovered later,
> and legitimacy with МОН is the main defence against Curipod in [[competitors]].

### 4.2 Question types

Four types. All auto-scored.

| Type | Student does | Scoring |
|---|---|---|
| Multiple choice | Taps one of 2–4 options | Correct, plus speed bonus |
| One-line reason | Types a short free-text reason after an opener vote | Points for writing one, right or wrong |
| Numeric | Types a number | Correct within a tolerance the author sets, e.g. `2601 ± 10`. Plus speed bonus |
| True / false | Two taps | Correct, plus speed bonus |

Numeric and true/false exist because of the pilot subjects. Physics and electronics
questions cannot honestly be expressed as four options.

The one-line reason is the pedagogically load-bearing part, not decoration: writing a reason
raises commitment, and commitment is what drives the hypercorrection effect described in
[[learning-from-mistakes-research]]. It scores whether the answer was right or wrong.

Not in v1: ordering, matching, tap-the-map, drawing.

### 4.3 Media in questions

- **Images** — in the question, the options and the reveal. Needed for history.
- **Formulas** — rendered maths and chemical notation. Needed for physics and electronics;
  without it those teachers cannot write their questions at all.

Images must have a low-bandwidth path: students are frequently on mobile data in a concrete
building ([[technical-architecture]]). Serve compressed, sized for a phone, and never block
a question's timer on an image that has not loaded.

### 4.4 Timers

The generator suggests a duration per question based on what it wrote — a recall question is
not a calculation. The teacher can override any of them while reviewing, before the lesson
runs.

**Proposed starting defaults — not yet decided, needs sign-off:**

| Question shape | Proposed |
|---|---|
| Recall, true/false | 15 s |
| Multiple choice, reasoning | 25 s |
| Numeric, calculation | 45 s |
| Opener vote | 30 s, then 45 s for the reason |

### 4.5 Running one lesson for several classes

One lesson object, many sessions. The teacher runs the same lesson for II-1, II-2 and II-3;
each run is its own session with its own attendance, answers, points and results. Editing
the lesson between runs affects later runs, not the record of earlier ones.

This is what makes the ~400 generations per teacher per year in [[cost-structure]] real
rather than aspirational, and it lets a teacher compare how two classes did on the same
question.

### 4.6 Refinements — one tap, not a conversation

A fixed row of actions on the generated lesson:

- Easier
- Harder
- More questions
- Make the opener a debate (or a prediction)
- Shorter timers

No free-text chat. These cover the structural changes inline editing cannot do — turning a
prediction opener into a debate is not an edit to a field — while staying inside the
curriculum by construction, costing a fraction of a conversation, and giving nothing for a
prompt to wander into.

This replaces the teacher chatbot described in the handbook, the canvas and [[features]].

### 4.7 The school library

Every lesson a teacher saves is visible to other teachers **in the same school**, who can
copy it into their own account and edit their copy. The original is never modified by a
copy.

This is what makes a whole-staff licence pay off immediately: one keen teacher's work seeds
the building instead of fifty-five people each paying the same setup cost alone. It is also
the first safe step toward the national corpus named as the moat in [[competitors]].

No national library in v1. No cross-school sharing. No public lessons.

---

## 5. The live session

### 5.1 There is no timetable

The app has no concept of periods, schedules or a school timetable. A teacher prepares a
lesson whenever they like and runs it whenever they like. "Today" is not driven by a
schedule; it is what the teacher has prepared and not yet run.

This is a deliberate rejection, not an omission. A timetable is setup work that goes stale
every time the school reshuffles, in exchange for ordering a list.

### 5.2 Two screens, not one mirrored screen

The teacher runs the lesson from a laptop. There are **two browser windows**:

| Window | Where | Shows |
|---|---|---|
| Class view | Projected | QR and join code, joined counter, the question, live bars, the reveal, the leaderboard |
| Console | Teacher's laptop screen | Everything above in miniature, plus the anonymous inbox with sender names, per-student answer state, the pop-up trigger, the skip control |

The console must never be projected. Named student data and the anonymous inbox live only
there. Setting up extended display is a real friction point for a teacher who has thirty
students waiting — the onboarding (10.2) has to cover it explicitly, and the class view must
be openable as a separate window with one click, not by dragging.

### 5.3 Session states

    lobby -> opener -> teaching -> review -> ended

The teacher advances the state. The server is the timer authority for every countdown;
clients render it.

### 5.4 Joining — QR, rotating

The class view shows a QR and a short code. A student scans, is authenticated by their
school email session, and joins.

**The QR rotates every 30 seconds.** A photographed QR sent to a friend at home stops
working almost immediately. This closes the gap [[qa-prep]] #17 currently concedes to
judges.

Joining marks the student present, with a timestamp. Late joins are timestamped as late.

Attendance here is TeacherAid's own. Official attendance stays in e-Дневник — see 8.1.

### 5.5 The opener

Two modes. **Prediction is the default.**

- **Prediction** — "What will happen if…?", 3–4 options. Students vote, then type one line
  of reasoning. Live bars on the class view, then the reveal.
- **Debate** — a claim; students pick a side and give one reason. The teacher reads two
  reasons aloud and lets the class argue.

Points for being right. Points for giving a reason, **right or wrong**. That ratio is a
product decision and must not be configurable by a teacher — it is the mechanism, not a
preference.

Reasons are shown to the class without names. The teacher's console shows names.

### 5.6 Pop-up questions

The teacher taps once; the question lands on every phone with its timer running. Teacher
triggered, never scheduled — unpredictability is the point.

**The nudge:** if several minutes pass with no question fired, the console shows a quiet
suggestion. Not a modal, not a sound. It protects the teacher who got absorbed in teaching
and fired none, which is the failure mode that produces an empty dashboard and a school
that looks inactive.

The teacher can also fire an ad-hoc question — "quick: yes or no?" — without preparation.

### 5.7 Anonymous questions — anonymous to classmates, not to the teacher

A student can send a question at any time during the lesson. **The teacher sees who sent
it.** No classmate ever does.

This is the corrected definition. The fear being designed around is peer judgement — the
student who will not raise a hand because thirty people are watching, not because the
teacher is. The teacher knowing is not the problem; it is what makes the feature safe to
run at all, because there is no anonymous channel for abuse.

Rate limit: 3 per student per lesson (default).

The teacher answers them in the review block. The inbox lives on the console only.

> **Cost of this decision.** [[data-and-privacy]] currently promises these are stored
> "without student id". That is now false and the note must be corrected before any ДПА or
> parent notice is issued on the old wording.

### 5.8 A bad question mid-lesson

The teacher can **skip** a question during the session: it is removed from the session, and
nobody is scored on it. The class view says so plainly so students know it does not count.

No live text editing. Changing a question's wording while thirty phones are rendering it is
where live sessions break. The teacher fixes the lesson afterwards, in the copy they reuse.

### 5.9 Bad network

Phones **queue answers locally and sync on reconnect**. A student who answers during a
20-second dropout is still credited, with their original answer time.

The console distinguishes **offline** from **has not answered**. This matters: from the
front of the room a dropped connection and a disengaged student look identical, and one of
them is about to get a focus flag they did not earn.

The teacher's lesson is cached on their device. If everything fails, the teacher still has
the questions and can run them out loud; points pause.

### 5.10 Ending

**The teacher presses End.** There is no auto-close. Points settle, attendance is written,
the absentee list appears, the lesson is archived.

> **Cost of this decision.** A forgotten tap leaves a session open, points unsettled and
> attendance unwritten, and the class stays at the top of the teacher's list until someone
> notices. Build a visible open-session indicator and make ending an already-open session
> from anywhere in the app a one-tap action.

---

## 6. Focus lock

### 6.1 Mandatory

**A student who has not authorized focus lock cannot join a session.**

This overrides the "not blocked, only flagged" behaviour in [[features]],
[[technical-architecture]] and the handbook.

> **Cost of this decision.** [[qa-prep]] #2 currently answers judges and parents with:
> "a student can revoke it — and then the teacher sees a focus flag. It's social and
> technical, not a prison, and that's why parents accept it." That answer is no longer true
> and must be rewritten before it is given again. The new answer has to justify a hard
> requirement on a personal phone, which is a harder sentence to say to a parent. It also
> means the student app is a hard dependency for every single participant — there is no
> web fallback and no gradual adoption.

### 6.2 Mechanism

The student app is native, because the lock needs operating-system APIs a web page cannot
reach.

- **iOS** — Screen Time: `FamilyControls` for authorization, `ManagedSettings` to shield
  apps, `DeviceActivity` for the time window. Individual authorization from the device
  owner, no parent pairing. Requires Apple's Family Controls entitlement, which takes weeks
  to obtain — start the request early.
- **Android** — `PACKAGE_USAGE_STATS` to detect a foreground distraction app plus a system
  overlay, or an `AccessibilityService`. Declare the focus/productivity use case for Play
  review.

The lock never reads what the student does in other apps. It knows only that TeacherAid is
not in the foreground.

### 6.3 The distraction list

TeacherAid ships a sensible default — social, games, messaging. **The school admin can
adjust it**, once, for the whole school.

School-wide rather than per-teacher on purpose: students compare rules between teachers, and
inconsistent ones invite argument in class.

### 6.4 A student who cannot or will not authorize

They are treated exactly like a student with no phone (7.1): **marked present, no points,
and the reason shown on the dashboard.**

Never marked absent. A false absence over a phone setting is the incident that reaches a
parent, then the headmaster, and ends a pilot.

---

## 7. Students without a working phone

### 7.1 No phone, dead battery, no data, or no focus-lock authorization

Marked present by the teacher in one tap. No points for the lesson. The dashboard shows the
reason, so a zero is not read as disengagement.

This is one code path serving four causes. It will be used in most lessons.

### 7.2 Not in v1

No pairing with a classmate's device. No teacher entering answers on a student's behalf.

---

## 8. After the lesson

### 8.1 The absentee list

On End, the teacher sees **only the names of students who were absent**, formatted for
copying into e-Дневник.

The teacher retypes four names instead of checking thirty. This is the smallest feature in
the spec and probably the moment a teacher first feels the product saved them something.

TeacherAid does not write to e-Дневник and does not replace it.

### 8.2 The review block

The class view shows:

- The most-missed questions, ranked worst first
- The anonymous questions, for the teacher to answer aloud

The teacher supplies the explanation. No AI re-explanation, no generated remedial content —
nothing unreviewed reaches students.

### 8.3 Practice sets

After a lesson the teacher can send an optional practice set to the class.

The set is **auto-assembled from that session's own missed questions** — no new generation,
no cost, no unreviewed content. It is retrieval practice on exactly what this class got
wrong, which is what [[learning-from-mistakes-research]] supports.

Doing it is voluntary and scores points. The teacher sees who completed it and how they
scored.

> **Cost of this decision.** [[product-overview]], [[features]] and the handbook state four
> separate times that TeacherAid has no homework and is not a learning management system.
> That line is now crossed. It is a good feature; the positioning sentence — "everything
> else is a tool for the teacher or a game for the students, TeacherAid is the lesson" —
> needs rewriting to survive it, and parents will read optional practice as homework.

### 8.4 Per-student results

TeacherAid stores, per student: attendance, points, answers, correctness, response time,
and **per-topic correctness over time**.

The teacher sees which student is repeatedly wrong on a specific topic.

> **Cost of this decision.** This is an assessment record on a minor. It needs its own
> clause in the ДПА, an answer for parents, and a rewrite of the "TeacherAid never grades
> anyone" framing in [[features]], the handbook and [[qa-prep]] #9 — which is currently the
> answer to whether points may inform a grade. The app still does not grade; it now holds
> the data a grade could be built from, and that distinction has to be stated deliberately
> rather than assumed.

---

## 9. Dashboards and the report

### 9.1 Teacher dashboard

One screen per class:

- Attendance per lesson and per student
- Points and participation over time
- Focus flags, with the reason where there is one
- Per-topic correctness, class and per student
- The anonymous inbox, with sender names
- Practice completion and scores
- Lesson history, every lesson reusable

Nothing to configure.

### 9.2 School admin overview

Setup (classes, rosters, staff, distraction list) plus a **usage overview**: which teachers
are running lessons, how often, and which have not started.

That number is the early warning for a school where only the champion teacher is active —
the signal [[channels-and-relationships]] names — and somebody inside the building needs to
see it before renewal month, not after.

The admin does not see student answers, reasons, anonymous questions or per-topic results.

### 9.3 The headmaster report

Monthly, automatic, by email as a PDF. No login, nothing to configure.

**Page one — written to be forwarded** to parents, the municipality or the local press
without editing:

- Lessons run
- Teachers active
- Average attendance
- Average engagement (answers per pop-up)
- Top classes
- Top teachers
- A two-line plain-language summary
- **Month-on-month trend**

**Page two — headmaster only, not for forwarding:**

- Comparison against other schools using TeacherAid

The split exists because a school ranked ninth of twelve will not forward a page that says
so, and page one has to stay forwardable. In a twelve-school pilot most schools are outside
the top few.

The email carries a **permanent private link** to every past report, so a headmaster who
needs last month's figures for the municipality does not have to ask anyone. One
unguessable URL, no account.

---

## 10. Onboarding and support

### 10.1 The setup lesson

Because focus lock is mandatory and needs a native app plus an operating-system permission,
**each class spends one period on setup before its first real lesson**: install, sign in
with the school email, authorize the lock, with the teacher present to fix the five phones
that resist.

Forty-five minutes given up once per class. The alternative is spending that time anyway,
unplanned, during the first opener — the moment the product is supposed to prove itself.

This is an unverified assumption. See section 14.

### 10.2 Teacher first run — a guided first lesson

Not a video. The app walks the teacher through building one real lesson end to end: pick the
unit from the e-учебник, review the questions, run it. They finish holding a lesson ready
for tomorrow.

This has to work fifty-five times in one week without a founder in the room, because the
whole staff arrives at once rather than one volunteer at a time.

It must cover opening the class view as a second window (5.2) — that is the step most likely
to go wrong in front of a class.

### 10.3 Support

- In-app help, searchable, covering the common failures
- **A direct channel to a human**, one tap, for when it is urgent

A help article is no use to a teacher standing in front of thirty students at 08:05.

### 10.4 Accessibility

Baseline only in v1: adequate contrast, legible at the back of the room and on a cheap
screen, and never colour alone to signal right or wrong.

> **Cost of this decision.** No per-student extra time, no committed screen-reader support,
> no text scaling guarantee. The core loop is "read fast, answer in twenty seconds", which
> is hardest on exactly the students who most need an accommodation — and a class of 28 will
> contain several. Recorded as a known limitation, not an oversight. Expect it to be raised
> by a parent, a school psychologist or the ДПИ.

---

## 11. Data, privacy and security

### 11.1 What is stored

| Data | Who sees it |
|---|---|
| Student name, class, school email | Teacher, admin, the student |
| Attendance per session with timestamp | Teacher; aggregated for the headmaster |
| Answers, correctness, response time, points | Teacher; the student sees their own |
| Per-topic correctness over time | Teacher; the student sees their own |
| One-line reasons | Teacher with names; class without names |
| Anonymous questions | Teacher, with the sender's name |
| Focus events and authorization state | Teacher |
| Practice completion and scores | Teacher; the student sees their own |
| Badges | Teacher, the student, classmates |
| Lessons and generated content | The authoring teacher; other teachers in the school via the library |

Not stored: grades, student contact details beyond the school email, location, device
identifiers beyond a push token, anything about other apps on the phone.

### 11.2 Legal structure

The school is the data controller; TeacherAid is the processor under a ДПА with each school.
Law: ЗЗЛП (2020), GDPR-aligned.

Two clauses the old ДПА draft does not cover and now must:

1. **Per-topic assessment data on minors** (8.4)
2. **Anonymous questions stored with the sender's identity** (5.7)

Verify the age of digital consent in ЗЗЛП before launch (GDPR default is 16, states may
lower to 13) — **(verify)**.

### 11.3 Leaderboards and names

Class leaderboard: real names. School leaderboard: nicknames by default, the school may
choose otherwise.

### 11.4 TeacherAid staff access

**No standing access to any school's data.** Access is granted per support incident, is
time-limited and expires automatically, is written to an audit log, and **the school admin
can see every occurrence**.

This puts a mechanism behind a promise [[data-and-privacy]] already makes, and it is the
most convincing single thing to show a headmaster on the data question.

### 11.5 Data subject requests

Handled manually by support on the school's written request: export everything held on one
student, or delete it.

> **Cost of this decision.** No self-serve export or deletion for the admin. The ЗЗЛП
> response clock is a legal one and it now depends on one person being available and not on
> holiday. With per-student assessment data in scope, the stakes are higher than when the
> privacy note was written. Track requests somewhere durable from the first one.

### 11.6 Retention

Session data: current school year plus one, then aggregated and deleted. Monthly reports
kept in aggregate, without names. A student who leaves: deleted within 30 days on the
school's request.

### 11.7 Security

EU hosting. Encryption in transit and at rest. Role-based access as in section 3. Student
names are never sent to the AI provider — generation uses the unit and the teacher's
sentence, never the roster.

---

## 12. Roster lifecycle

In v1, performed by the school admin:

- **Add a student mid-year.** They start at zero points. The class leaderboard must show
  they joined partway through, or it looks broken.
- **Remove a student.** Their data is retained under 11.6 until deletion is requested.
- **Move a student between classes.** Their history moves with them. A split record is both
  useless to a teacher and a problem when the school asks for everything held on that
  student.

Not in v1: reassigning a class to a different teacher, or a second teacher on a class. A
teacher on extended leave has no supported path, and their classes are stranded in an
account nobody is using. This will happen within the first school year.

---

## 13. Points, badges and leaderboards

### 13.1 Scoring

Fixed values, identical in every school, so class and school leaderboards compare like with
like and no teacher can inflate their class.

**One exception:** the teacher can switch the **speed bonus** off for a class — for a group
where it causes rushing, or a subject where it rewards the wrong instinct.

**Proposed starting values — not yet decided, needs sign-off:**

| Event | Proposed points |
|---|---|
| Opener vote, correct | 10 |
| Opener vote, incorrect | 0 |
| Opener reason written | 8 |
| Pop-up correct | 10 |
| Speed bonus | up to 5, scaled by time remaining |
| Practice question correct | 5 |

The ratio that matters: **a reasoned wrong answer must score close to a silent right one.**
That is the productive-failure mechanism, and it is why reason points are 8 against 10 in
the proposal above rather than a token 2.

### 13.2 Badges

Named achievements for things points cannot reward — reasoning on a wrong answer, a
comeback, asking a good question. Design work still to do. Empty badges are worse than none.

No streaks in v1: they punish the student who was ill for a week, and streak pressure is the
part parents notice.

### 13.3 Leaderboards

- **Class** — real names
- **School** — nicknames by default

Seasons run **per semester**, matching the Macedonian school calendar: a season closes when
marks are being decided and the teacher wants participation figures anyway, and a student
who fell behind in October is not written off until May.

No national leaderboard in v1.

---

## 14. Open assumptions

Unverified facts this specification rests on. These move into [[risks-and-assumptions]] as
numbered rows on integration.

| # | Assumption | What breaks if wrong | Evidence needed |
|---|---|---|---|
| A | Every target school issues student mailboxes | The entire login design. No student can join | Confirm with the pilot school before any build **(verify)** |
| B | e-учебник rights can be cleared with МОН/БРО | The primary planning input, and the legitimacy argument | A written answer before the feature ships |
| C | A class will give up 45 minutes for a setup lesson before seeing any value | Adoption; the first impression is a support session | Ask the pilot teacher directly |
| D | Mandatory focus lock does not trigger the parent complaint the old design avoided | The pilot, and the parent-facing answer | Pilot; parent notice reviewed before week one |
| E | Teachers and parents accept per-student assessment data being stored | The ДПА, and the "never grades anyone" position | Ask the advising teacher and a parent representative |
| F | Apple grants the Family Controls entitlement in time | No iOS students at all | Start the request immediately; it takes weeks |

---

## 15. What to update in the vault

The integration work order. Nothing here is done yet.

| # | Note | Now out of date | Becomes |
|---|---|---|---|
| 1 | [[features]], [[technical-architecture]], handbook 2.4, [[qa-prep]] #2 | "not blocked, only flagged"; "not prison-grade — that's why parents accept it" | Focus lock mandatory; no authorization, no join. The parent and judge answer needs rewriting |
| 2 | [[data-and-privacy]] | Anonymous questions "stored without student id" | Anonymous to classmates; the teacher sees the sender. Add a ДПА clause |
| 3 | [[features]], handbook 2.4, [[qa-prep]] #9 | "TeacherAid never grades anyone" | Per-topic correctness per student is stored. Add a ДПА clause and a parent answer |
| 4 | [[product-overview]], [[features]], handbook 2.5 | "no homework", "not an LMS" | Optional practice sets after class. Rewrite the positioning line |
| 5 | Handbook 2.4, [[business-model-canvas]] block 2, [[features]] | Teacher chatbot | One-tap refinements, no open chat |
| 6 | [[user-flows]] | "name and class typed once"; "added by scanning, not by typing a list" | School-email login; admin-managed rosters |
| 7 | [[user-flows]] | A "Today" screen implying a schedule | No timetable at all |
| 8 | [[features]], [[technical-architecture]], [[product-overview]] | "Type three sentences" as the headline input | e-учебник unit is primary; three sentences is the fallback. Add the rights item |
| 9 | Every business note | "Private гимназија beachhead" | Pilot subjects history, electronics and physics imply a стручно or technical school |

Also on integration:

- Add assumptions A–F to [[risks-and-assumptions]].
- Trim [[features]] to the short overview and link here.
- Remove the university line from v1 scope wherever it implies a build commitment; the
  15 institutions stay in the market sizing, not in the product.

## Related

[[features]] · [[product-overview]] · [[user-flows]] · [[technical-architecture]] ·
[[data-and-privacy]] · [[prototype]] · [[risks-and-assumptions]] ·
[[learning-from-mistakes-research]]
