---
created: 2026-10-13
type: area
status: live — demo for testing flows, not the product
---

# Student app demo

A mobile-first PWA of the student app, built 12 and 13 Oct 2026 from five screens the team
designed (home, drawer, language, notifications, profile). Its job is to test the **user flows
between the screens** and the **QR check-in**, not to be the final app. The real app is still
Flutter ([[technical-architecture]]); this is the clickable reference for how it should behave.
It is separate from the Startup Weekend opener in [[prototype]].

## Where to open it

- Team link, no login: https://teacher-aid-bitola.vercel.app/app
- Test QR codes: `/app/qr-kit` on the same address (open on a second screen and scan with the
  in-app scanner).
- Source: `~/Desktop/teacher-aid-app`, route group `app/(app)/`, components in
  `components/app/`, logic and copy in `lib/app/`. Pushes to `main` deploy to Vercel.
- The camera needs https or localhost. Over plain `http://localhost` a production build looks
  unstyled because the site sends `upgrade-insecure-requests`; that only affects local
  production builds, not the deployed site.

## What it does

- **Sections:** Home, Alerts, Profile (swipe sideways between them, or tap the tab bar).
- **Scan (the main feature):** a yellow button beside the tab bar opens a camera scanner that
  reads a class QR (native BarcodeDetector first, jsQR fallback for iPhone Safari); photo upload
  is the fallback (typing the code was removed on 13 Oct 2026). A class QR opens the live quiz
  for that class at `/app/play/<CODE>`: joining records attendance (87% to 88%) and adds a
  notification, but earns no points. A code for a class that is not yours, or any other QR,
  shows a clear message instead.
- **Points and achievements (13 Oct 2026):** points come only from quizzes and achievements
  (20 each, proposed). First answer (answer a question in a quiz, right or wrong) and Perfect
  quiz are earned in the quiz; the class leaderboard and the Home rank include them.
- **A class QR is a link** ending `/app/checkin/<CODE>` (MATH-7B, PHYS-7B, MKD-7B, ENG-7B), so
  a phone's own camera app also checks you in.
- **Screens:** home with a next-lesson card, schedule, attendance, summary (4 of 5), leaderboard,
  subject pages, notifications and detail, profile, settings, documents, achievements, FAQ,
  welcome (after log out), the test QR codes.
- **Language:** Macedonian by default, English second, one dictionary, switched from Profile or
  Settings (bottom sheet).
- State lives in the browser (localStorage); Settings has "Reset demo data".

## Design and motion decisions

Look and rules are in [[design-system]] (version 2). On top of that, an Apple HIG pass on
13 Oct changed the structure:

- Tab bar has **three tabs plus a detached Scan button**; no drawer and no More tab. The
  drawer's items moved to grouped rows on Profile. (The drawer was in the original screens;
  the HIG treats a tab bar action and an overflow tab as the wrong pattern.)
- Lists are inset grouped cards; tab screens have large titles; the logo is on Home only;
  system font for body text, Golos Text for titles and numbers; one yellow control per screen.
- Motion is limited to moments that show a change: the check-in lock-and-draw, the attendance
  count-up, the card-to-subject morph, physical sheets, the sliding tab indicator, the swipe
  pager, the notification badge. Nothing loops in the background.
- Accessibility: 44px targets, reduced motion, reduced transparency and increased contrast
  respected, a one-time camera pre-prompt. Text reflows up to 200% (fixed and measured at 320
  to 430px on 13 Oct 2026: grids drop to one column, labels wrap under their text). All text is
  at least 4.5:1 (the rank and label chips were darkened on 13 Oct 2026). While a sheet is open
  the page behind it is inert, so keyboard focus and screen readers stay in the sheet.
- Navigation (13 Oct 2026): switching tabs, by swipe or tap, replaces the history entry instead
  of adding one, so Back leaves the section. Scan closes like a modal: Done and its back button
  return to the screen it was opened from. A class link opened while logged out goes through
  Welcome and checks you in after Continue. Damaged saved data falls back to defaults, and a
  screen that fails shows Try again and Reset demo data.

## Known limits

- All data is mock; documents do not download; app icons reuse the old midnight ones; there is
  no service worker (not offline).
- Swiping moves the next section in after you let go; the next screen is not visible while you
  drag (an Instagram-style pager would render all three at once).
- No light appearance; the app is blue-ground only.
- Camera and swipe feel were tested with simulated input, not on a real phone.
- Open from the 13 Oct 2026 audit: Home flashes on first load, the search results are not
  usable with arrow keys, Windows high-contrast mode hides the notifications switch, a swipe
  from the right edge competes with Android's Back, and the morph does not play on Back. Fixed the same day: the camera stops when the app is in the
  background and says when another app takes it; Android reads each frame once. The demo also still shows a timetable, a scheduled pop quiz and a streak
  badge that [[feature-spec-v1]] rules out.

## Versions

- The version before the HIG pass is kept on branch and tag `app-demo-blue-yellow-v1`
  (GitHub `predragkirovmk/teacher-aid-app`).

## Ideas not built yet

- A row-swap animation when the leaderboard rank moves. Built 13 Oct 2026: points come only
  from quizzes and achievements (20 each, proposed), check-ins earn none ([[feature-spec-v1]]
  13.1), and the class leaderboard and the Home rank add them, so a good quiz moves Marko from
  4th to 2nd. A whole quiz answered right unlocks Perfect quiz.
- Real app icons in blue and yellow; teacher-desktop screens in the new system.
- Bring the website and the Flutter app to the new design system.

Related: [[design-system]] · [[prototype]] · [[user-flows]] · [[features]] · [[2026-10-12]] · [[2026-10-13]]
