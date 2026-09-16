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
| 0 | **Every target school issues student mailboxes.** The whole login design rests on it — students sign in with their school email, and without it no student can join at all | Confirmation from the pilot school, in writing | Ask before any build. If the answer is no, the identity design has to be redone | Dev | **New — flagged 15 Sep, highest damage in this table** |
| 0b | **Rights to use the e-учебници** can be cleared with МОН/БРО. Pre-loading МОН-published textbooks into a commercial product is the primary planning input | A written answer from МОН or БРО | Ask, with the lawyer already engaged for the ДПА. Blocking: the feature does not ship without it | Presenter | **New — flagged 15 Sep, blocking** |
| 0c | **Mandatory focus lock does not trigger the parent complaint** the old "not blocked, only flagged" design avoided | No complaint reaching the headmaster in the pilot | Parent notice rewritten and reviewed before week one; the "present, no points, never absent" fallback is the part that makes it defensible | Team | **New — flagged 15 Sep** |
| 0d | **Teachers and parents accept per-student assessment data** being stored (per-topic correctness over time) | The advising teacher and a parent representative say it is fine | Ask directly; ДПА clause drafted before the pilot | Presenter | **New — flagged 15 Sep** |
| 0e | **A class will give up 45 minutes for a setup lesson** before seeing any value | The pilot teacher agrees to schedule one per class | Ask the pilot teacher | Team | **New — flagged 15 Sep** |
| 0f | **Apple grants the Family Controls entitlement in time.** Without it there are no iOS students at all | Entitlement granted | Start the request immediately — it takes weeks | Dev | **New — flagged 15 Sep** |
| 1 | A private-school headmaster will pay EUR 199 / month flat for the whole staff | One verbal yes | Ask the warm contact this week; ask the SW education mentor whether the number is sane | Presenter | Open |
| 1b | A large school (~80+ teachers) stays profitable at the flat fee — AI cost could exceed EUR 2,388/year revenue if usage is heavy | Measured AI cost per school after 100+ lessons | Instrument from day one; add a size tier if the data says so | Dev | New — flagged 13 Sep |
| 2 | Teachers will pick the unit from the e-учебник before class (typing three sentences is the fallback) | 3 of 5 interviewed teachers say "yes, that's less than I do now" | Ask in every teacher conversation; log in [[validation-log]] | Team | Partly: teachers interviewed, quotes to log |
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

- **The 25-year veteran who refuses.** Not every teacher needs to use it on day one. The
  flat whole-staff fee means nobody has to opt in for the school to be covered; the veteran
  comes around once the class next door is visibly quieter.
- **Practical lessons are out of v1.** In a technical school a large share of periods are
  практична настава, so lessons-per-teacher — the retention metric below — is structurally
  halved. Measure per *theory* period or the pilot will look worse than it is.
- **The pilot subjects imply the wrong beachhead.** History, electronics and physics point at
  a стручно or technical school; every note in this folder assumes a private гимназија. One
  of the two is out of date, and it is a business decision nobody has taken yet.
- **Points gaming.** Students will share answers. Speed bonus and per-device sessions limit
  it; the teacher's discretion on grades removes the stakes that would make it worth it.
- **Support at 08:05.** Magic-link login, no passwords; the champion teacher as first line.
- **Data incident.** Minimal data, EU hosting, controller/processor structure — the blast
  radius is names and points.
- **Small market.** MK is the proving ground; every neighboring country has one national
  curriculum too. Say it in Q&A, not in the pitch.

## Related

[[feature-spec-v1]] — rows 0–0f come from its section 14 ·
[[business-model-canvas]] · [[qa-prep]] · [[validation-log]] · [[data-and-privacy]]
