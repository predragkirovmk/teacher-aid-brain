---
created: 2026-09-16
type: area
status: living — lifted from the Startup Weekend Q&A prep, now written for headmasters
---

# Objections and how they are answered

The six questions that get asked every time, and the answer that holds up. These started as
judge prep for Startup Weekend; they survived the weekend because a headmaster, a parent and
the ДПИ ask the same things in different words.

Format: repeat the question in five words, answer in three sentences, stop. "We don't know
yet, here's how we'd find out" is a full answer. Never say "great question".

## 1. "How can you block apps on a student's own phone?"

It is possible with the student's one-time permission. On iOS it is Apple's Screen Time API,
the same mechanism focus apps like Opal use; on Android it is usage access plus an overlay,
like AppBlock. **Authorizing it is how you join the lesson** — a student who does not is
marked present and sits the game out, exactly like a student with a dead battery, and nobody
is ever marked absent over a phone setting.

The fallback is what makes this acceptable to a parent, so say it in the same breath. See
[[feature-spec-v1]] 6.1 and 6.4.

## 2. "Students are minors. What about the data?"

The school is the data controller — it already processes attendance — and TeacherAid is the
processor under a ДПА, exactly like any school software. We store what the lesson produces:
name, class, attendance, points, and which topics each student keeps getting wrong. Nothing
from the rest of the phone, EU hosting, deleted when the student leaves, and nobody at
TeacherAid can open a school's data without a logged, time-limited grant the school can see.

Full detail in [[data-and-privacy]].

## 3. "Do points count toward grades? Is that allowed?"

The app never grades — the teacher decides if and how the numbers inform a participation
grade, the same discretion they have today with a raised hand, and nothing changes in МОН's
grading rules. What the app does do is show the teacher which topics a student keeps getting
wrong, which is information a good teacher already tries to hold in their head.

**Volunteer the second half.** Someone who discovers the per-topic data after hearing a flat
"we never grade" will hear the whole answer as evasion.

## 4. "Why not just use ChatGPT and Kahoot for free?"

They do — that is five tabs and a paper attendance list, and none of them talk to the
headmaster. TeacherAid is the whole lesson in one flow: plan, opener, pop-ups, attendance,
focus, the questions students will not ask out loud, and a report the school actually pays
for. The school pays, not the teacher; free tools never get a headmaster's signature.

## 5. "Curipod already does this."

Curipod is the closest and it is a good product — a slide tool sold to teachers, in English,
with no attendance, no focus lock and no headmaster report. If they localized tomorrow they
would still be selling to the wrong person. The moat is school relationships, daily teacher
habit, and the school-versus-school leaderboard that improves with every school added.

See [[competitors]] for the honest view of where TeacherAid is weaker.

## 6. "Phones are being banned in schools across Europe."

As of May 2026 the Minister said the teacher decides — those words exactly — and that a ban
in secondary schools would only be discussed. Any ban will carve out teacher-sanctioned
educational use, which is what TeacherAid is. That is also why the wedge is secondary rather
than primary, where a restriction is more likely.

Quote and source in [[macedonian-education-system#Phone policy]]. Keep it for the answer, do
not lead with it.

## Rules that travel with these answers

- If two people ask at once, answer the harder one.
- Numbers said out loud must match whatever is on the current price sheet — check
  [[revenue-and-pricing]] before any meeting, because the price changed once already.
- If someone is wrong about a fact, agree with the concern first, then give the fact.

## Related

[[messaging]] · [[feature-spec-v1]] · [[data-and-privacy]] · [[competitors]] ·
[[macedonian-education-system]] · [[revenue-and-pricing]]
