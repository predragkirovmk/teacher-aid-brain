---
created: 2026-09-13
type: area
---

# Mockups and deck assets

Hi-fi mockups of the app (paper / ink / amber system, Bitter + Sofia Sans), built for the
Startup Weekend deck on 13 Sep 2026. Each `.png` is a 2x render of its `.html` source —
edit the HTML and re-render with headless Chrome to change anything.

- `mock-phones.png` — student phone, four screens of one lesson: opener (min 1–5), pop-up
  (min 22), review + anonymous question (min 40–45), leaderboard.
- `mock-dashboard.png` — teacher dashboard, live at minute 22.
- `teacheraid-vs-competitors.png` — five-feature comparison vs Kahoot and Curipod.
- `tam-sam-som-concentric.png` / `tam-sam-som-actual-pie.png` — market-size slides.

Re-render: `"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new
--force-device-scale-factor=2 --window-size=1920,1060 --screenshot=out.png file://…/mock-phones.html`

OpenDesign projects for the same screens exist at `~/Desktop/open-design` ("TeacherAid —
Teacher dashboard", "TeacherAid — Student phone") but need the terminal `claude` CLI logged
in (`claude` → `/login`) before they can generate; the prompts are in the project
conversations.
