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
  reads a class QR (native BarcodeDetector first, jsQR fallback for iPhone Safari). Photo
  upload and typing the code are fallbacks. A class code records attendance, moves attendance
  from 87% to 88%, adds a notification and unlocks the "First scan" achievement. A code for a
  class that is not yours, or any other QR, shows a clear message instead.
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
- Accessibility: contrast checked, 44px targets, reduced motion, reduced transparency and
  increased contrast respected, text scales to 200%, a one-time camera pre-prompt.

## Known limits

- All data is mock; documents do not download; app icons reuse the old midnight ones; there is
  no service worker (not offline).
- Swiping moves the next section in after you let go; the next screen is not visible while you
  drag (an Instagram-style pager would render all three at once).
- No light appearance; the app is blue-ground only.
- Camera and swipe feel were tested with simulated input, not on a real phone.

## Versions

- The version before the HIG pass is kept on branch and tag `app-demo-blue-yellow-v1`
  (GitHub `predragkirovmk/teacher-aid-app`).

## Ideas not built yet

- Points per check-in so the leaderboard rank moves (4th to 3rd) with a row-swap animation.
- Real app icons in blue and yellow; teacher-desktop screens in the new system.
- Bring the website and the Flutter app to the new design system.

Related: [[design-system]] · [[prototype]] · [[user-flows]] · [[features]] · [[2026-10-12]] · [[2026-10-13]]
