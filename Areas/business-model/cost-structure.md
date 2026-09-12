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
- Consequence for pricing: at EUR 60 / seat / year, AI is ~30% of revenue at Sonnet prices.
  Not negligible — it is the main variable cost, and the main reason the price cannot go
  below EUR 4.

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
| AI inference | seats × lessons | caching, cheaper models for simple calls, lesson reuse, content packs |
| Support | schools, and teacher churn | onboarding videos, champion teachers, chatbot |
| Sales | schools (each is a meeting) | municipality deals cover many schools per contract |
| Realtime | concurrent sessions at 09:00 | managed vendor, per-region |
| National leaderboard | nearly free | — |

## Break-even sketch

At EUR 60 / seat / year and ~EUR 25 variable cost per seat, each seat contributes ~EUR 35.
A team of three at MK salaries (say EUR 4,500 / month all-in) needs ~1,550 seats — roughly
30 schools of 50 teachers. That is the year-3 target in [[revenue-and-pricing]]. Grants and
sponsorship bridge years 1–2.

## Related

[[revenue-and-pricing]] · [[technical-architecture#AI]] · [[risks-and-assumptions]]
