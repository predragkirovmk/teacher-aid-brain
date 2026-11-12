---
created: 2026-11-12
type: area
status: direction approved 2026-11-12 (light look); step 1 of 7 in progress
---

# Teacher app redesign

The teacher app (`/teacher`) made simpler and calmer, with every feature kept. This note
holds the decision and where the work stands. The plan itself and the code are in the app
repo: `docs/teacher-redesign-plan.md`, pull request 8 of
github.com/predragkirovmk/teacher-aid-app.

## Why

The owner, 2026-11-12: the app feels crowded, complex and confusing; it is for teachers, so
it has to be simple; the feel wanted is Apple luxury. Keep the features, change the UI and
UX, drastically if needed. Plan first, and approve or deny every major change through a
link.

Measured the same day on the demo school (`main` at 3dc6a64, mock data, a 1280x800 window;
the full table is in the plan):

- On the live lesson with a question open, 82% of the text is bold (weight 600 or more);
  on Lessons, 79%. Nothing leads when everything is heavy.
- Lessons lists 13 lessons with a button and a link each: 26 controls for 13 rows.
- The live lesson with a question open is 4.2 screens long, and "Заврши го часот" sits
  3,187px down the page.
- A class's results are 6.9 screens long, the lesson editor 5.7.
- Sidebar, ground and cards are three close dark teals, held apart by white outlines.

## Decision (2026-11-12)

Approved by the owner on the review page at 10:10 UTC, with the light look.

Five rules:

1. One job per screen. What the teacher does now; the rest one labelled click away.
2. Lists, not stacks of boxes. Rows on one surface; the whole row opens; one button a row.
3. One accent. Deep teal for the main action; yellow only while a lesson is live.
4. Type carries the hierarchy. One large title, regular text, bold only for names and
   numbers. Nothing under 16px.
5. The live lesson fits one screen. The question and the next step stay put; the class, the
   questions and End sit beside them.

Earlier rules of the teacher app that this replaces:

| Before | Now |
|---|---|
| Dark teal only, no light mode | Light by default; the projected class view stays dark teal |
| One yellow button per screen | The main button is deep teal; yellow marks a live lesson |
| Buttons 56px, small ones 48px | 48px main, 44px others |
| A button and a link on every card | The whole row opens; one button a row |
| Body text 18px | 17px; the 16px floor stays |
| The sidebar on every screen | It steps aside during a live lesson |

Also decided the same day: UI libraries are allowed in the teacher app, where one carries
behaviour that is hard to get right by hand (tabs, segmented controls, menus): headless,
accessible parts, styled with the app's own tokens.

What does not change: every feature and flow, the wording (Macedonian first, English
second), right and wrong shown as an icon and a word, red for End and warnings only, the
API contract (no screen needs new data, so the backend handoff documents are untouched),
the student app and the website.

## How each step is approved

Review page, private to the owner: https://claude.ai/artifact/GaAL6hpukNw9EnvFB3KvdJ

Seven steps after the direction. Each is built on one branch, gets a preview link, and is
shown on the review page with before and after pictures. The next step starts only when the
owner approves the one before.

1. Foundation and frame: colours, type, buttons, fields, windows, the sidebar.
2. Today, Lessons, Classes, Library.
3. Preparing a lesson: the form, the wait, the editor, the lesson page.
4. The live lesson.
5. After the lesson: the end page, a class's results.
6. Settings, Help, sign-in, first run, and every loading, empty and error state.
7. The projected view and the final checks.

## Status

- 2026-11-12: direction approved (light look). Step 1 in progress.

## Related

[[prototype]] · [[features]] · [[user-flows]] · [[2026-11-12]]
