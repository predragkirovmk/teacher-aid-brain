---
created: 2026-09-12
type: area
status: estimate
---

# Cost structure

## AI inference

Prices (Anthropic API, first-party, 2026):

| Model | Input $/1M tokens | Output $/1M tokens |
|---|---|---|
| Claude Sonnet 5 | 2.00 | 10.00 |
| Claude Haiku 4.5 | 1.00 | 5.00 |
| Claude Opus 5 | 5.00 | 25.00 |

One lesson, generously counted:

| Call | Input tokens | Output tokens |
|---|---|---|
| Plan generation (system prompt + curriculum excerpt + teacher text/plan pages) | 3,000 | 2,500 (opener, 6 pop-ups, timing, review) |
| Two chatbot refinements | 4,000 | 1,000 |
| **Total** | **7,000** | **3,500** |

| Model | Cost per lesson | Per teacher per year (400 generations) |
|---|---|---|
| Sonnet 5 | $0.014 + $0.035 = **$0.049** | ~$20 |
| Haiku 4.5 | $0.007 + $0.018 = **$0.025** | ~$10 |
| Opus 5 | $0.035 + $0.088 = **$0.12** | ~$49 |

Notes:
- 400 generations/year assumes ~20 lessons a week over 36 weeks with reuse across parallel
  classes and repeats from last year. Heavy users cost more; most teachers less.
- Prompt caching on the system prompt + curriculum excerpt cuts the input side roughly 90%
  on repeated calls; the numbers above ignore it (conservative).
- Ad-hoc pop-ups and chatbot small talk on Haiku 4.5; full plans on Sonnet 5.
- **The stage number: "about five cents per lesson."**
- Consequence for pricing (updated 13 Sep — flat EUR 199/month per school, not per seat):
  AI cost now scales with **how many teachers actually use it**, while revenue is flat. At
  ~55 teachers (the MK secondary average) and Sonnet prices, AI alone is ~46% of the
  EUR 2,388/year fee. This is the real margin risk of flat pricing — see
  [[revenue-and-pricing#The real trade-off: margin now depends on school size, not price]].

## Hosting and realtime

- Vercel / a small Node service + Postgres + managed realtime: EUR 50–150 / month for the
  first 20 schools; scales roughly EUR 2–3 per seat per year after that.
- Object storage for uploaded plans: negligible.
- App store: Apple EUR 99 / year, Google EUR 25 once.

## Support and onboarding

- Decided: videos + in-app tutorials + chatbot. Human support behind it.
- Estimate 10 hours per school in year 1 (first-lesson calls, login issues, the teacher who
  hates it). At an internal EUR 15 / hour: EUR 150 per school. Falls with product maturity.
- The **free live session** as a sales stunt: half a day of two people per school. Worth it
  only where the headmaster meeting is already booked.

## Fixed

- Development: the team, unpaid for now (runway undecided). The first hire is a mobile
  developer for the student app / focus lock.
- Curriculum curation: loading БРО programs per subject and year — one-off, ~2 weeks of
  one person; ongoing as programs change.
- Legal: ДПА template, privacy policy, terms — one-off with a lawyer, EUR 500–1,500.
- Sales: founder time; travel to schools is local.

## What gets expensive as it grows

| Cost | Scales with | Mitigation |
|---|---|---|
| AI inference | active teachers × lessons, **but revenue per school is flat** | caching, cheaper models, lesson reuse, content packs, watch large-school margin |
| Support | schools, and teacher churn | onboarding videos, champion teachers, chatbot |
| Sales | schools (each is a meeting) | municipality deals cover many schools per contract |
| Realtime | concurrent sessions at 09:00 | managed vendor, per-region |
| National leaderboard | nearly free | — |

## Break-even sketch

At EUR 2,388/year per school and an average school (~55 teachers, ~EUR 1,375 variable
cost), each school contributes ~EUR 1,000/year. A team of three at MK salaries (say
EUR 4,500 / month all-in) needs ~54 average-sized schools — well beyond the year-3 target
of 40 schools in [[revenue-and-pricing]]. That gap is real: **flat per-school pricing
reaches break-even later than per-seat pricing would have**, unless a few things offset it
— grants and sponsorship (bridging years 1–2), a size tier on large schools if the margin
data says so, or the secondary-revenue lines (content packs) coming online sooner than
planned. Flag this honestly in Q&A rather than paper over it.

## Related

[[revenue-and-pricing]] · [[technical-architecture#AI]] · [[risks-and-assumptions]]
