---
created: 2026-09-12
type: area
status: living list — update after every conversation
---

# Risks and assumptions

Every claim the model rests on, what would prove it, and who owns finding out. Ordered by
how much damage being wrong does.

| # | Assumption | Evidence needed | How to get it | Owner | Status |
|---|---|---|---|---|---|
| 1 | A private-school headmaster will pay EUR 199 / month flat for the whole staff | One verbal yes | Ask the warm contact this week; ask the SW education mentor whether the number is sane | Presenter | Open |
| 1b | A large school (~80+ teachers) stays profitable at the flat fee — AI cost could exceed EUR 2,388/year revenue if usage is heavy | Measured AI cost per school after 100+ lessons | Instrument from day one; add a size tier if the data says so | Dev | New — flagged 13 Sep |
| 2 | Teachers will type three sentences (or upload a plan) before class | 3 of 5 interviewed teachers say "yes, that's less than I do now" | Ask in every teacher conversation; log in [[validation-log]] | Team | Partly: teachers interviewed, quotes to log |
| 3 | Students will authorize focus lock on their own phones | Pilot: ≥ 70% authorize in week 1 | Pilot class | Team | Untested |
| 4 | Focus lock is technically deliverable on iOS and Android | Working build using Screen Time API / usage access; Apple entitlement granted | Build after the weekend; Apple entitlement request takes weeks — start early | Dev | Untested; mechanism documented in [[technical-architecture]] |
| 5 | No national phone ban arrives for secondary schools | МОН statements; the May 2026 line is "teacher decides" | Monitor; build the relationship with МОН | Presenter | Currently favorable — see [[macedonian-education-system#Phone policy]] |
| 6 | Parents do not block it | No complaint that reaches the headmaster in the pilot | Parent notice template; nickname leaderboards; school as controller | Team | Untested |
| 7 | The monthly report is enough to make a headmaster renew | Renewal after the free month; the headmaster forwards the report at least once | Pilot | Presenter | Untested |
| 8 | AI generates usable Macedonian lessons inside the БРО programs | Teacher rates ≥ 4/5 on 10 generated lessons without heavy edits | Build the generator; test with the advising teacher | Dev | Untested |
| 9 | Municipalities can contract a small pilot directly | Confirmed threshold and procedure under the public procurement law | Ask the municipality education department; ask a public-school accountant | Team | Unknown |
| 10 | School wifi failure does not kill the lesson | Students on mobile data works in a concrete building | Free live session in a Bitola school | Team | Untested |
| 11 | The class-vs-class / school-vs-school leaderboard motivates rather than shames | Pilot survey; focus flags do not rise | Pilot; nickname option | Team | Untested |
| 12 | AI cost stays ≤ EUR 20 / seat / year | Measured token usage after 100 lessons | Instrument from day one | Dev | Estimate in [[cost-structure]] |
| 13 | Curipod does not localize to Macedonian before we have 20 schools | Watch their language list | Quarterly check | Presenter | Open |
| 14 | A teacher on the team / as advisor by end of September | Named person | UKLO, the education mentor, the interviewed teachers | Presenter | Open |

## Risks that are not assumptions (they will happen; plan for them)

- **The 25-year veteran who refuses.** Not every teacher needs to use it. Per-seat pricing
  means the school pays only for those who do.
- **Points gaming.** Students will share answers. Speed bonus and per-device sessions limit
  it; the teacher's discretion on grades removes the stakes that would make it worth it.
- **Support at 08:05.** Magic-link login, no passwords; the champion teacher as first line.
- **Data incident.** Minimal data, EU hosting, controller/processor structure — the blast
  radius is names and points.
- **Small market.** MK is the proving ground; every neighboring country has one national
  curriculum too. Say it in Q&A, not in the pitch.

## Related

[[business-model-canvas]] · [[qa-prep]] · [[validation-log]] · [[data-and-privacy]]
