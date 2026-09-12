---
created: 2026-09-12
type: area
status: proposal — price not yet validated with a single headmaster
---

# Revenue and pricing

## The model (decided): per teacher seat, invoiced per school

- **EUR 5 per teacher per month → EUR 60 per seat per year**, invoiced once a year to the
  school. Unlimited students and lessons per seat.
- **First month free**, whole school, no card. Conversion decision on the first monthly
  report and on teacher usage.
- Minimum 10 seats per school so a "pilot" is a real pilot.
- Public-school volume (municipality buys for all its schools): EUR 4 / seat / month.

Why per seat and not per school: it prices the tool at the level where value is felt (the
teacher), lets a school start small with the champions, and grows automatically as adoption
spreads inside the school — the bottom-up route needs a price that follows adoption.

## Why EUR 5

- A 50-teacher gymnasium pays **EUR 3,000 / year** — less than one interactive whiteboard,
  and about one month's net salary of one teacher (MK teacher net pay ≈ EUR 600–700 / month;
  verify current figure). Cheap enough for a private school to decide in one meeting.
- Per teacher it is EUR 60 / year — comparable to Curipod / Kahoot paid teacher tiers, but
  paid by the school, not the teacher.
- Gross margin at full use: AI ≈ EUR 20 / seat / year, hosting and support ≈ EUR 5 →
  ~55–60% gross margin at EUR 60. Thin for SaaS; acceptable for the first year; improves
  with caching, cheaper models, and teachers reusing lessons. See [[cost-structure]].
- If validation says EUR 5 is too low for private schools (likely — they will not blink),
  test EUR 8. Do not go below 4.

## Sample school P&L (private gymnasium, 50 teachers, year 1)

| | EUR |
|---|---|
| Revenue (50 × 60) | 3,000 |
| AI inference (50 × ~20) | −1,000 |
| Hosting, realtime, storage | −150 |
| Support time (10 h × 15) | −150 |
| **Contribution** | **1,700** |

## Market math (North Macedonia only — decided scope)

Figures from the State Statistical Office, start of 2024/25, in [[market-size]].

| | Schools | Teachers (seats) | ARR at EUR 60 |
|---|---|---|---|
| Upper secondary (SAM) | 128 | ~7,000 (estimate) | ~EUR 420,000 |
| Primary + lower secondary (later) | 943 | 19,447 | ~EUR 1.17 M |
| **All MK schools (TAM)** | ~1,070 | ~26,500 | **~EUR 1.6 M / year** |

Obtainable, honestly:

| | Schools | Seats | ARR |
|---|---|---|---|
| Year 1 (2026/27) | 3 private + 2 public pilots | ~250 | ~EUR 15,000 |
| Year 2 | 15 | ~750 | ~EUR 45,000 |
| Year 3 | 40 (all Bitola + Skopje private + first municipalities) | ~2,000 | ~EUR 120,000 |

Say the small number on stage. Judges in Bitola know the country is small; pretending
otherwise costs credibility. The upside line for Q&A: the same product works in any country
with one national curriculum — Serbia, Bosnia, Albania, Kosovo are next, and they are 10×
the seats. (Market scope for the pitch stays MK.)

## Secondary revenue (decided to explore, not in year 1 numbers)

- **Sponsorship:** a telecom or bank sponsors "AI in Bitola's schools" — pays the seats for
  public schools as CSR, gets its name on the leaderboard season. Telecoms also sell the
  mobile data the students use.
- **Grants:** EU (Erasmus+ / IPA education calls), UNICEF (EDUINO partner), МОН innovation
  programs. Grants pay for the public-school rollout the municipalities cannot.
- **Premium content packs:** ready lesson sets per subject and year, curated with teachers,
  sold per school. Only after the corpus exists.

## Payment mechanics

- Private schools: invoice, bank transfer, yearly. Standard.
- Public schools: the municipality pays; under the small-value threshold of the public
  procurement law a direct contract is possible (verify the current threshold and procedure
  — this decides whether a EUR 3,000 contract is a one-week or a three-month process).
- No card payments, no teacher-paid plans in v1 (a teacher paying personally undermines
  the headmaster sale).

## Assumptions to validate first

1. A private-school headmaster says yes to EUR 5 / seat without negotiating (test: ask).
2. Average active seats per school ≥ 60% of teachers by month 3.
3. Renewal ≥ 80% after year 1.
4. The municipality can contract a EUR 3,000 pilot directly.

## Related

[[cost-structure]] · [[market-size]] · [[customer-segments]] · [[risks-and-assumptions]]
