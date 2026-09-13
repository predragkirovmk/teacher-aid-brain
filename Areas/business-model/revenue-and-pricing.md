---
created: 2026-09-12
updated: 2026-09-13
status: proposal — price not yet validated with a single headmaster
---

# Revenue and pricing

## The model (decided): flat per school, whole staff included

- **EUR 199 per month per school → EUR 2,388 per year**, invoiced once a year, flat —
  regardless of how many teachers are on staff. No per-seat counting, no minimum, no tiers.
- **We set it up for every teacher**, not just volunteers. The sale is "the whole staff is
  on this," not "let one teacher try it." This is a top-down, whole-school rollout, not a
  bottom-up product-led adoption.
- **First month free**, whole school, no card. Conversion decision on the first monthly
  report and on how teachers actually use it during that month.

Why flat per school and not per seat: it is one number a headmaster can say yes to in a
single meeting, with nothing to count and nothing to negotiate as staff changes. It also
makes the product's network effect real from day one — the school-vs-school leaderboard
means something the moment every class in the building is on it, not just two volunteers'
classes.

## Why EUR 199

- **EUR 2,388 / year** for a whole school — cheaper than one interactive whiteboard, and a
  number a private-school headmaster can approve without a board meeting.
- For a public school, EUR 2,388/year for the entire staff sits comfortably inside most
  small-value procurement thresholds (verify the exact threshold with the municipality).
- Compares to nothing else on the market priced this way: Kahoot/Curipod-style tools are
  priced per teacher, which either caps out cheap (if few teachers pay) or gets expensive
  fast (if the whole staff does). One flat number removes that conversation entirely.

## The real trade-off: margin now depends on school size, not price

Cost still scales with how many teachers actually use the product (~EUR 20/teacher/year in
AI alone — see [[cost-structure]]); revenue does not. That means:

| School size | Teachers | AI + support cost | Revenue | Margin |
|---|---|---|---|---|
| Small | 20 | ~EUR 500/yr | EUR 2,388 | **~79%** |
| Average MK secondary | ~55 | ~EUR 1,375/yr | EUR 2,388 | **~42%** |
| Large | 80 | ~EUR 2,000/yr | EUR 2,388 | **~16%** |
| Very large | 100+ | ~EUR 2,500+/yr | EUR 2,388 | **negative** |

**Decided:** no size tiers for now — one flat number, deliberately, for the simplicity of
the sale. This means small schools are very profitable and a handful of very large schools
could run at a loss if every teacher uses it heavily. Watch this in the pilot; a size band
(e.g. a higher price above ~70 teachers) is the fix if it becomes a real problem, not before.

## Sample school P&L (private gymnasium, ~55 teachers, year 1)

| | EUR |
|---|---|
| Revenue (flat) | 2,388 |
| AI inference (55 × ~20) | −1,100 |
| Hosting, realtime, storage | −150 |
| Support time (10 h × 15) | −150 |
| **Contribution** | **~988** |

## Market math (North Macedonia only — decided scope)

Figures from the State Statistical Office, start of 2024/25, in [[market-size]]. Revenue is
now **per school**, not per seat.

| | Schools | ARR at EUR 2,388/school |
|---|---|---|
| Upper secondary (SAM) | 128 | **~EUR 306,000** |
| Primary + lower secondary (later) | 943 | ~EUR 2.25 M |
| **All MK schools (TAM)** | ~1,070 | **~EUR 2.6 M / year** |

Obtainable, honestly:

| | Schools | ARR |
|---|---|---|
| Year 1 (2026/27) | 3 private + 2 public pilots | **~EUR 12,000** |
| Year 2 | 15 | **~EUR 36,000** |
| Year 3 | 40 (all Bitola + Skopje private + first municipalities) | **~EUR 95,500** |

Note: this is a **lower ceiling** than the per-seat model would have given at the same
school count, because flat pricing is deliberately cheap for a large staff. That's the
trade you're making for a simpler, faster, whole-school sale — say so plainly if a judge
asks "why not price per teacher."

Say the small number on stage. Judges in Bitola know the country is small; pretending
otherwise costs credibility. The upside line for Q&A: the same product works in any country
with one national curriculum — Serbia, Bosnia, Albania, Kosovo are next, and they are 10×
the schools. (Market scope for the pitch stays MK.)

## Secondary revenue (decided to explore, not in year 1 numbers)

- **Sponsorship:** a telecom or bank sponsors "AI in Bitola's schools" — pays the flat fee
  for public schools as CSR, gets its name on the leaderboard season. Telecoms also sell the
  mobile data the students use.
- **Grants:** EU (Erasmus+ / IPA education calls), UNICEF (EDUINO partner), МОН innovation
  programs. Grants pay for the public-school rollout the municipalities cannot.
- **Premium content packs:** ready lesson sets per subject and year, curated with teachers,
  sold per school. Only after the corpus exists.

## Payment mechanics

- Private schools: invoice, bank transfer, yearly. Standard.
- Public schools: the municipality pays; under the small-value threshold of the public
  procurement law a direct contract is possible (verify the current threshold and procedure
  — this decides whether a EUR 2,388 contract is a one-week or a three-month process).
- No card payments, no teacher-paid plans — the whole point of a flat school fee is that no
  individual teacher ever sees a bill.

## Assumptions to validate first

1. A private-school headmaster says yes to EUR 199/month flat, for the whole staff, without
   negotiating (test: ask).
2. Whole-staff rollout doesn't stall on the teachers who never wanted to opt in — watch
   actual usage rate in the free month, not just "installed."
3. Renewal ≥ 80% after year 1.
4. The municipality can contract a EUR 2,388/year pilot directly.
5. **The margin holds at real school sizes** — instrument AI cost per school from day one;
   if a large school's cost approaches the EUR 2,388 flat fee, revisit the no-tiers decision.

## Related

[[cost-structure]] · [[market-size]] · [[customer-segments]] · [[risks-and-assumptions]] ·
[[channels-and-relationships]]
