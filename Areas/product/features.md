---
created: 2026-09-12
type: area
status: overview — the build detail lives in [[feature-spec-v1]]
---

# Features

The ten things TeacherAid does, one paragraph each. This note is the map.

**The build detail is in [[feature-spec-v1]]** — screens, states, rules, edge cases and
failure behaviour, written for the developer. Where this note and the spec disagree, the
spec is right.

## QR join and attendance

Teacher starts the lesson; the class view shows a QR plus a short code. One scan joins the
session and marks the student present, timestamped; late scans are marked late. The QR
rotates every 30 seconds so a photographed code cannot be used from home. Attendance is
separate from e-Дневник — the teacher still records official attendance there, and after
the lesson TeacherAid shows a copy-ready list of who was absent.

## Focus lock

During a live session the student app blocks apps on a distraction list and shows the
teacher who left the app. **Authorization is mandatory: a student who has not authorized
cannot join the session** — they are marked present with no points, and the reason is shown
on the dashboard. The distraction list ships as a TeacherAid default that the school admin
can adjust. Mechanism: iOS Screen Time APIs, Android usage access plus an overlay. The
student app must therefore be native; the teacher side is web.

## The opener

Minutes 1–5. One question, in prediction mode (default) or debate mode. Students vote and
type one line of reasoning. Live bars on the class view, then the reveal. Being right earns
points; **giving a reason earns points whether right or wrong**, because the pedagogy is
productive failure and the pretesting effect ([[learning-from-mistakes-research]]) and the
incentive has to reward committing to a guess. Reasons are shown to the class without names.

## Pop-up questions

Minutes 5–40. The teacher taps once; a question lands on every phone with a timer. Teacher
triggered, never scheduled — unpredictability is the point — with a quiet nudge on the
console if a long gap passes with none fired. Question types: multiple choice, numeric with
a tolerance, true/false. Images and formulas are supported. Wrong answers are collected for
the review block.

## Anonymous questions

Any time during the lesson a student can send a question that **no classmate sees. The
teacher does see who sent it.** The fear being designed around is peer judgement, not the
teacher's — and it means there is no anonymous channel for abuse. Rate-limited to three per
student per lesson. Answered in the review block.

## Points, badges, leaderboards

Fixed scoring, identical in every school so boards compare like with like; the one thing a
teacher can change is switching the speed bonus off for a class. Badges for things points
cannot reward. Leaderboards at class level (real names) and school level (nicknames by
default), in per-semester seasons. **At the teacher's discretion, points inform the
participation grade** — the app shows numbers, the teacher decides how they count.

## Teacher planning with AI

The teacher **picks today's unit from the e-учебник**, which TeacherAid pre-loads and chunks
for the pilot subjects, and optionally adds one sentence of emphasis. Typing three sentences
is the fallback for subjects with no loaded book. Output: opener, 5–8 pop-ups with answers,
a suggested timer per question, and the review summary — in Macedonian, editable. Refinement
is a fixed row of one-tap actions (easier, harder, more questions, make it a debate, shorter
timers), not an open chatbot. One lesson can be run for several classes with results kept
separate, and every lesson is visible to other teachers in the same school.

## Teacher dashboard

Attendance per lesson and per student; points over time; focus flags with their reason;
per-topic correctness for the class and per student; the anonymous inbox with names;
practice completion; lesson history. One screen per class, nothing to configure.

## Headmaster report

Monthly, automatic, by email as a PDF, with a permanent private link to past reports. No
login and nothing to configure. Page one is written to be forwarded — lessons run, teachers
active, attendance, engagement, top classes, top teachers, a two-line summary, and the
month-on-month trend. Page two, for the headmaster only, carries the comparison against
other schools.

## Onboarding and support

Each class spends one period on setup before its first real lesson: install, sign in with
the school email, authorize the lock. Teachers get a guided first lesson inside the product
— they finish holding a real lesson, not having watched a video. Support is in-app help plus
a direct channel to a human. The free live session in a school remains the sales stunt.

## Not in v1

Universities; practical and лабораториска настава; a national leaderboard; streaks; an
open-ended chatbot; e-Дневник sync; Albanian-language UI; any timetable; a parent app;
per-student extra time; self-serve data export and deletion; reassigning a class to another
teacher; co-teaching; in-app billing. Optional practice sets after class **are** in v1 — see
[[feature-spec-v1]] section 8.3 and the positioning consequence it carries.

## Related

[[feature-spec-v1]] · [[product-overview]] · [[user-flows]] · [[technical-architecture]] ·
[[data-and-privacy]]
