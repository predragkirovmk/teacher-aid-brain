---
created: 2026-11-12
type: area
status: direction approved 2026-11-12 (light look); step 1 approved 2026-11-13; step 2 of 7 shown 2026-11-13, waiting for the owner
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
- 2026-11-13: step 1 (foundation and frame) built and shown on the review page; it waits
  for the owner's decision. Steps 2 to 7 have not started.
- 2026-11-13, 10:09 UTC: the owner approved step 1 on the review page, with no note.
- 2026-11-13: step 2 (Today, Lessons, Classes and the Library as rows) built and shown on
  the review page; it waits for the owner's decision. Steps 3 to 7 have not started.
- 2026-11-14, 00:53 UTC: the owner approved step 2 on the review page, with no note.
- 2026-11-14: step 3 (preparing a lesson: the form, the wait, the editor, the lesson page,
  the Library's preview) built and shown on the review page; it waits for the owner's
  decision. Steps 4 to 7 have not started.

## Step 1 as built (2026-11-13)

Every teacher screen changed skin at once. No layout, flow, wording or feature changed, and
nothing new is asked of the API.

- Light palette and one scale for type and sizes: white panels on a pale ground, dark teal
  ink, Golos Text everywhere, 17px for reading, nothing under 16px.
- One main button a screen, deep teal and 48px. Every other button is quiet and 44px. End
  is white with a red edge. Yellow is kept for a live lesson: the band on Today, the mark in
  the sidebar, the open question.
- The sidebar is a floating white panel with who is signed in at its foot; the name left
  the top right of every page. Back is a quiet link above the title.
- The browser's own colour, the installed app and the offline page are light too.
- The projected class view is unchanged, pixel for pixel.

Left for the later steps on purpose: the layouts (rows instead of stacks of boxes, one
button a row, the live lesson on one screen).

Detail and the test changes: `docs/teacher-redesign-plan.md` in the app repo, "Step 1 as
built".

## Step 2 as built (2026-11-13)

The four lists stopped being stacks of boxes. Every feature is where it was or one press
away, and nothing new is asked of the API.

- A list is one white panel with a hairline between its rows. A row is a title and one line
  under it (two in the Library), with a chevron at its right end.
- The row opens: its title is the link and the link covers the whole row. The separate
  "Отвори", "Прегледај" and "Види резултати" are gone.
- One button a row at the most. A lesson, prepared or held, has "Започни час". A class, a
  Library lesson and a draft have none; a draft's one link is the words "Продолжи" or
  "Отвори".
- "Подготви лекција" is in the header, at the right of the title, on Today and on Lessons.
- Throwing a draft away moved to the end of the draft's own page ("Фрли ја лекцијата", or
  "Фрли ги измените" for changes to a saved lesson). It asks first, as before.
- The subject filter is drawn as a segmented control over the same radio buttons.
- The Lessons page of the demo teacher went from 2051px to 1287px tall at 1440 wide
  (measured on the two builds, 2026-11-13).
- The lesson page, the editor's questions, the class results, the live lesson and the
  projector are untouched: steps 3 to 7.

Detail and the test changes: `docs/teacher-redesign-plan.md` in the app repo, "Step 2 as
built".

## Step 3 as built (2026-11-14)

Preparing a lesson reads as one calm page at each stop. Every feature is where it was, and
nothing new is asked of the API.

- Questions are rows of one white panel wherever a lesson is read (the lesson page, the
  Library's preview, the editor): the number in a small disc, the question, one quiet line
  with its kind and timer, the answers, the explanation as one line. Each question was a
  box of its own with every answer in a box inside it.
- Answers stand two side by side where there is room. The right one has a tint under it, a
  tick and the words "Точен одговор", so it is never colour alone.
- In the editor "Уреди" is at the right end of the question's first line. The open question
  is the same row with a teal edge.
- "Промени ги сите прашања" (Полесни, Потешки, Повеќе прашања, Пократко време) stays after
  the questions. Putting it above them was tried and undone the same day: an earlier design
  decision, held by the tests, is that nothing but the lesson's name stands in front of the
  first question.
- The save bar says where saving stands at its left and has the one main button at its
  right.
- The prepare form is one narrow panel: year, material, emphasis, then the button with its
  line of reassurance. The wait keeps the same width.
- The editor of the demo's six-question lesson went from 3921px to 2611px tall, and
  the Library's preview from 2733px to 1818px, at 1280 wide (measured on the two builds,
  2026-11-14).
- The question's own form, the live lesson, the class results and the projector are
  untouched: steps 4 to 7.

Detail and the test changes: `docs/teacher-redesign-plan.md` in the app repo, "Step 3 as
built".

## Related

[[prototype]] · [[features]] · [[user-flows]] · [[2026-11-12]] · [[2026-11-13]] · [[2026-11-14]]
