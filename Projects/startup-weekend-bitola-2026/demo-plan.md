---
created: 2026-09-12
type: plan
status: v1 — URL and session code filled in when the prototype deploys
---

# Demo plan — the live opener with the judges

The demo is sixty seconds at the start of the pitch. It has one job: make the judges *do*
the product before hearing about it. Everything below exists so that those sixty seconds
cannot fail in a way that matters.

## What happens

1. Projector shows the host screen: a big QR, a short URL under it, "0 joined".
2. Judges scan. Their phone opens a web page: a pre-filled fun nickname, one button — Join.
3. The host counter climbs. The presenter says the number.
4. Presenter taps **Open** on the host (Space bar). Phones show the question:
   *Will Bitola see snow before the first of December?* — four options, then a one-line
   reason box.
5. Bars grow live on the projector as votes land.
6. Presenter taps **Reveal** (Space): bars settle, the reasons scroll, points are awarded —
   +20 for a reason, speed bonus for the first three.
7. Presenter taps **Leaderboard** (Space): top five nicknames. Then **Summary** (Space):
   "7 joined · 7 present · 7 reasons" — the teacher's view in one line.
8. Switch browser tab to Canva. Demo over.

Total: ~60 s. Scanning is the slow part; the script waits a maximum of 15 s for it.

## URLs and codes

- Host screen: `<PROTOTYPE_URL>/host` (fill in after deploy — see [[prototype]])
- Join link (the QR points here): `<PROTOTYPE_URL>/j/BITOLA`
- Short URL shown under the QR for anyone whose camera will not scan.
- Reset between rehearsals: press **R** on the host screen (asks to confirm), or open
  `<PROTOTYPE_URL>/host?reset=1`.
- Simulation mode (no network needed): `<PROTOTYPE_URL>/host?sim=1` — fake players join and
  vote on their own.

## Room checklist (Sunday morning, one teammate)

- [ ] Projector: open `/host` on the presenting laptop; the QR is readable from the judges'
      seats (test with a phone from the back row).
- [ ] Phones on **mobile data**, not the venue wifi — the judges' phones will be, and the
      venue wifi may block or throttle. Test one Android and one iPhone.
- [ ] Time from scan to "Join" tap under 10 s on both.
- [ ] Laptop on power, sleep disabled, notifications off (Do Not Disturb), browser zoom set
      so the QR fills the height.
- [ ] Browser tabs in order: tab 1 `/host`, tab 2 Canva Present mode. Nothing else open.
- [ ] Presenter's own phone joined in advance (it counts as a voter and lets you trigger a
      vote yourself if the room is slow).
- [ ] Printed QR (A4, from `/print`) — two copies, one on the judges' table.
- [ ] Session reset to zero right before the pitch.
- [ ] Sound off on the prototype (there is none) and on the laptop.

## Fallbacks, in order

1. **Fewer than three judges scan in 15 s.** Say "and the rest of you can watch the brave
   ones", vote from your own phone, continue. The bars work with two votes.
2. **A judge's phone will not scan.** Point at the short URL under the QR. Do not wait.
3. **The host page stalls (network).** Press **S** on the host: switches to simulation mode
   in place — fake players animate in, the flow continues. Say: "The venue wifi has
   opinions — here's the morning run." Honest, twenty seconds lost.
4. **The laptop/browser dies.** Switch to Canva on the backup device (a teammate's laptop
   with the deck open at slide 2, which carries the reveal screenshot). Skip the demo, say
   the "nobody checked Instagram" line anyway — it still works as a joke.
5. **Projector dies.** Present without slides; the script does not depend on them.

Rehearse fallback 3 once so pressing **S** is a reflex.

## What the judges see on their phones, step by step

1. A page with the TeacherAid mark, "Today's class: Geography, II-3" and a nickname like
   *Brave Falcon* with a Join button.
2. The question with four options; a reason box appears after tapping an option; Send.
3. "Sent — watch the board." Then the result and their points.
4. Their rank on the mini leaderboard.

Nothing to install, nothing to type except an optional reason.

## After the pitch

Leave `/host` on the summary screen during Q&A if the projector allows — the numbers stay
visible. If a judge asks to see the student side, hand them your phone.

## Related

[[prototype]] · [[pitch-script]] · [[rehearsal]]
