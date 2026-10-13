---
created: 2026-09-24
updated: 2026-10-13
type: area
status: current — version 2 (blue and yellow); the midnight-glass version 1 is retired
---

# Design system

The visual identity of TeacherAid. **Version 2, decided 12 Oct 2026:** a bright royal blue with
one sunny yellow. It replaces the midnight-blue glass system of 24 Sep 2026. It applies to the
student app, the website and decks. It feels clear, warm and quick, still calm and adult.

The mascot is not drawn yet (see [[name-and-mascot]]); the brand mark is a blocky M with a
T-shaped cut-out and two eyes.

## Where it lives

| What | Where |
| --- | --- |
| Shareable design system (tokens, brand book, cover) | https://claude.ai/artifact/7rFcFs1QqbevXBgSFM5Rvt (updated to version 2) |
| Source of truth for code | `~/Desktop/teacher-aid-flutter/design/teacheraid/`: `DESIGN.md`, `tokens.css`, `USAGE.md` |
| Version 1 archive | `~/Desktop/teacher-aid-flutter/design/teacheraid/_archive/v1-midnight-glass/` |
| Working reference build | the student demo at `/app` in `~/Desktop/teacher-aid-app` (`app/(app)/app.css`), documented in [[student-app-demo]] |
| The five source screens | sent in the 12 Oct 2026 session (home, drawer, language, notifications, profile); the drawer was later replaced, see below |

## Standard to maintain

- **Two hues: blue and yellow.** Yellow is the accent: fills, icons, rings, indicators, text of
  18px and above. Never small yellow text on blue (4.2:1 on the ground, 3.6:1 on a card). Text
  on a yellow fill is ink.
- **One yellow control per screen.** On the student app it is the QR scan. Yellow content (a
  subject card) and small signals (unread dot, check mark) are fine.
- **Right and wrong are never colour.** An icon plus a word. No green, no red.
- **Flat content cards; glass only for overlays** (tab bar, sheet, toast). Lists are inset grouped
  cards. Nothing moves in the background; the old moving field lagged on phones.
- **Navigation is labelled and standard.** Three tabs (Home, Alerts, Profile) and a detached
  yellow Scan button beside the bar, because Scan is an action, not a section. No drawer and no
  More tab (changed 13 Oct after an Apple HIG review): Documents, Achievements, Settings,
  Language, FAQ and Log out are grouped rows on Profile. Tab screens have large titles; the logo
  is on Home only.
- **Motion marks change.** Short, one ease, transform and opacity only: the check-in moment, a
  figure counting to its new value, a card morphing into its screen, physical sheets, swipe
  between sections. Nothing loops; reduced motion turns it all off.
- **Line icons**, 2px stroke, round caps, 24px minimum (Lucide in the reference build). The
  duotone glass set is retired.
- **Macedonian first**, English second, one dictionary, language names in their own language,
  no flags.

## Colours

| Name | Hex | Role |
| --- | --- | --- |
| Ground | `#1A5DAE` | Top of the screen gradient. White on it 6.5:1 |
| Ground deep | `#134785` | Bottom of the gradient. White 9.3:1 |
| Card | `#2468BE` | Flat cards. White 5.5:1, secondary text 4.7:1 |
| Card press | `#1D5AA6` | Pressed card |
| Sheet | `#0A2347` | Deep blue surfaces and the deep subject card |
| **Yellow** | **`#FFC83D`** | **The accent.** Ink on it 11.7:1 |
| Yellow soft | `#FFDA7A` | Pressed yellow |
| Ink | `#07162F` | Text on yellow and ice |
| Ice | `#E8F1FC` | Light subject card |
| White | `#FFFFFF` | Text on blue |
| Text 2 | `#E1EDFB` | Secondary text on blue |

Subject cards rotate three tones: yellow (ink text), ice (ink text), deep (white text, yellow
monogram), each with a monogram fallback for the teacher.

## Type

Golos Text (OFL) for titles and numbers, the system font for body, labels and captions. Number
46/700, display 34/700, subject 30/700, title 22/600, body 17/400, label 14/600, caption 13/400. Tabular figures. Cyrillic runs 20 to
30 percent wider, so rows wrap and never truncate.

## For the website

Use the same tokens. Blue grounds with flat cards, one yellow object per section, Golos Text,
Macedonian in Cyrillic, no photos of students. The site built on 24 Sep still uses version 1
(midnight glass); bring it to version 2 when it is next redesigned.

## Recurring tasks

- [ ] When the mascot lands, add it and replace the M mark.
- [ ] Redo the teacher-desktop screens in this system.
- [ ] Move the Flutter app (`teacher-aid-flutter`) off the midnight glass and duotone icons.
- [ ] Bring the website to version 2.
- [ ] Real app icons in blue and yellow (the demo reuses the old midnight icons).
- [ ] A light appearance (the app is blue-ground only).

## Related

[[name-and-mascot]] · [[messaging]] · [[product-overview]]
