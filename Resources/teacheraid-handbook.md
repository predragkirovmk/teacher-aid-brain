---
created: 2026-09-12
type: resource
source: compiled from this vault's Areas and Projects notes on 2026-09-12
tags: [handbook, business, reference]
---

# TeacherAid Handbook

Version 1.1 · compiled 12 September 2026, pricing updated 13 September 2026 · the night before and the morning of the Startup Weekend Bitola pitch. Every section here has a living note in the vault; when they disagree, the vault note is newer. This document exists so that one person — a teammate, a mentor, the first headmaster — can read the whole business in half an hour.

## 1. The business on one page

**What.** TeacherAid is a classroom system for secondary schools (гимназија, ages 15–18) in North Macedonia. A teacher plans a lesson with AI in five minutes; students join the lesson on their own phones by scanning a QR; the class opens with a prediction or a debate, continues with pop-up questions and points, and ends with the teacher reviewing what the class got wrong and answering the anonymous questions students sent during the lesson. The headmaster does nothing inside the app and receives a one-page report every month.

**For whom.** The buyer is the headmaster. The user is the subject teacher. The end user is the student. The payer is the school itself in private schools and the municipality in public schools.

**Why it works.** Predicting before instruction improves what is learned afterwards, even when the prediction is wrong (productive failure, the pretesting effect). Attention in a lecture lapses within minutes and recovers during active tasks. The phone is already in every pocket; as of May 2026 the Minister of Education says the teacher decides when it is used. TeacherAid is the sanctioned use.

**How it makes money.** EUR 199 a month, flat, per school — every teacher included, not just volunteers. EUR 2,388 a year, invoiced yearly. First month free. Private gymnasiums first, public schools through the municipalities second.

**How big.** 128 secondary schools and 15 universities — the real market today, 143 institutions, about EUR 341,500 a year. Every school and college in the country, including primary, would be about EUR 2.6 million a year — that's the ceiling, not today's target. The honest year-3 target, sold nationally rather than city-by-city, is 45 institutions (40 schools + 5 college faculties), about EUR 107,500 a year — roughly a third of the real market, in three years.

**What it costs to run.** About five cents of AI per generated lesson — roughly EUR 20 per teacher per year at heavy use — plus hosting, support and the team.

**Where it stands (12 September 2026).** Idea complete. Canvas corrected. Pitch written. A working web demo of the live opener is deployed at teacher-aid-bitola.vercel.app. Teachers have been interviewed; one headmaster contact is warm but has not said yes. No teacher on the team yet.

**The line.** Every class starts with a question.

## 2. The product

### 2.1 The 45-minute class

The unit of the product is the lesson period — 45 minutes in secondary school (40 in primary). Everything is designed around that clock, not around "content" or "courses".

| Minute | What happens | Who acts |
|---|---|---|
| 0 | Teacher projects a QR. Students scan. One scan = joined the session + marked present. Focus lock starts. | Students |
| 1–5 | **Opener.** A prediction or debate question generated from today's topic. Students vote and type a one-line reason. Live results on the projector. Being wrong is expected; reasoning earns points. | Students; teacher moderates |
| 5–40 | **Teaching.** The teacher teaches normally. Any time they want, one tap sends a pop-up question to every phone: 10–30 seconds to answer, points for correct and for fast. Students can send an anonymous question the teacher alone sees. | Teacher triggers; students answer |
| 40–45 | **Review.** The teacher goes through what the class got wrong and answers the anonymous questions. Points close. Rewards unlock. | Teacher |
| after | Points roll into class, school and national leaderboards. Dashboard updates. | App |

### 2.2 Before the class — the teacher's five minutes

1. Open TeacherAid. Type three sentences: subject, topic, what matters today. Or upload the existing годишен or тематски план and pick the unit.
2. The AI returns the opener question, 5–8 pop-up questions with answers, a suggested timing, and a short summary for the review block. All in Macedonian. All editable.
3. Optionally ask the teacher chatbot to adjust: "make it easier", "add a question about X", "make the opener a debate instead of a prediction".
4. Save. The lesson is ready to run.

### 2.3 After the class

- **Teacher:** attendance, per-student points and participation, who left the app (focus flags), the anonymous questions, the lesson saved for reuse next year.
- **Student:** points, rank in class and school, in-app rewards.
- **Headmaster:** nothing until the end of the month, then the one-page report — lessons run, average attendance, engagement, top classes, top teachers — written so it can be forwarded to parents, the municipality or the local press without editing.

### 2.4 Features

**QR join and attendance.** One QR per lesson, shown on the projector or on the teacher's phone, with a short code for phones whose camera will not scan. Scanning joins the session and marks the student present; late scans are timestamped. Attendance is separate from e-Дневник — the teacher still records official attendance there; TeacherAid attendance feeds the dashboard and the report. A rotating QR (every 30 s) to stop students scanning a photo from home is a day of work, decided after the pilot.

**Focus lock.** During a live session the student app blocks apps on a distraction list (social media, games, messaging) and shows the teacher who left the app. This is a real feature, not a slogan, and it is technically possible on personal phones: on iOS through the Screen Time API (FamilyControls, ManagedSettings, DeviceActivity — since iOS 16 an app can ask the device owner for individual authorization and then shield chosen apps for a time window; consumer focus apps such as Opal and one sec ship on exactly this, and Apple grants the entitlement on request); on Android through usage-access permission plus an overlay, or an accessibility service — the mechanism app blockers such as AppBlock and Forest use. Both require the student to authorize once at install. A student who revokes is not blocked, but the teacher sees a focus flag. The lock is social plus technical, which is also why parents accept it. Consequence: the student app must be native; the teacher side can be web.

**The opener.** Minutes 1–5. One question in one of two modes. Prediction: "What will happen if…?" with 3–4 options, vote, then one line of reasoning. Debate: a claim, pick a side, give one reason; the teacher reads two reasons out loud and lets the class argue for two minutes. Live bars on the projector, then the reveal. Being right earns points; giving a reason earns points whether right or wrong — deliberately, because the pedagogy is productive failure and the pretesting effect and the incentive must reward committing to a guess. Reasons are shown to the teacher with names; to the class anonymously by default.

**Pop-up questions.** Minutes 5–40. The teacher taps once; a question lands on every phone with a 10–30 s timer. Generated with the lesson plan (5–8 per lesson), editable, plus ad-hoc ones ("quick: yes or no?") without preparation. Points for correct and for speed. Wrong answers are collected for the review block. Teacher-triggered, not scheduled — unpredictability is the point.

**Anonymous questions.** Any time during the lesson a student can send a question only the teacher sees. The teacher answers them in the last five minutes or later. Rate-limited (default three per lesson).

**Points, rewards, leaderboards.** Points per lesson for the opener (right or reasoned), pop-ups (right or fast) and participation. Rewards are in-app: badges, streaks, rank. At the teacher's discretion, points inform the participation grade — the app shows the numbers, the teacher decides how they count; TeacherAid never grades anyone, which matters for parents and for МОН. Leaderboards at class, school and national level; school versus school is the retention hook and the network effect. Season length is open (a semester is the working assumption).

**Teacher planning with AI.** Input: three typed sentences and/or an uploaded plan (PDF or DOCX). Output: opener, pop-up questions with answers, timing, review summary — in Macedonian, editable. The teacher chatbot refines the plan conversationally and is for teachers only; there is no student-facing AI. The БРО programs for secondary subjects are loaded as reference so generation stays inside what is actually taught.

**Teacher dashboard.** Attendance per lesson and per student; per-student points over time; focus flags; the anonymous inbox; lesson history with every generated plan reusable next year. One screen per class, nothing to configure.

**Headmaster report.** Monthly, automatic, one page, PDF by email: lessons run, teachers active, average attendance, average engagement (answers per pop-up), top classes, top teachers, a two-line plain-language summary. Written to be forwarded. The headmaster has no login and nothing to configure.

**Onboarding and support.** In-app tutorials and short videos; a "first lesson in ten minutes" path; in-app help and the teacher chatbot as first line, a human behind it for the first schools. No in-person onboarding in the base offer, but the free live session — the team runs one real class in a school — is the sales stunt.

### 2.5 What TeacherAid is not

Not a learning management system: no homework, no grades, no content library in version one. Not a replacement for e-Дневник. Not a student tutor. Not in version one: parent app, Albanian-language UI, e-Дневник sync, primary-school mode. All are roadmap; none are promised on stage.

### 2.6 User flows in brief

- **Teacher, first time (10 min):** invite link → account (name, subjects, classes) → 90-second video → add one class. Students are added by scanning, never by typing a list.
- **Teacher, every lesson:** Start → QR on the projector → Open when most are in → moderate the opener → teach and fire pop-ups → glance at the anonymous inbox → Review → End.
- **Student, first time (2 min):** scan → install the app → name and class once → authorize focus mode once, with a one-sentence explanation and the note that turning it off is visible to the teacher.
- **Student, every lesson:** scan → vote and reason → answer pop-ups → optionally ask anonymously → see points and rank.
- **Headmaster, buying (one meeting):** a teacher who has seen or run a free live session brings TeacherAid in; a 20-minute meeting with the class flow, the sample report, the price and the free month; yes; the school admin receives invite links. The headmaster's involvement ends here. Every month: receives the PDF, forwards it.

## 3. The pedagogy behind it

The pitch claims "students learn more when they make mistakes". The precise, defensible version: committing to a guess before instruction improves what is learned afterwards, even when the guess is wrong, and errors made with confidence are corrected especially well.

- **Productive failure.** Students who attempt problems before being taught the method — and mostly fail — learn the later instruction more deeply. Kapur (2008) *Cognition and Instruction*; Kapur (2014) *Cognitive Science*; Sinha & Kapur (2021) *Review of Educational Research*, a meta-analysis of 53 studies with a moderate positive effect overall (d ≈ 0.36), larger when the design is followed faithfully.
- **The pretesting effect.** Being tested on material before studying it — and getting it wrong — improves later recall. Kornell, Hays & Bjork (2009) *JEP: Learning, Memory, and Cognition*; Richland, Kornell & Kao (2009) *JEP: Applied*. This is exactly the prediction opener.
- **The hypercorrection effect.** High-confidence errors are more likely to be corrected after feedback than low-confidence ones. Butterfield & Metcalfe (2001); Metcalfe (2017) *Annual Review of Psychology*, the single reference to cite if only one is cited. This is why the opener asks for a one-line reason: writing the reason raises commitment, and commitment raises the correction.
- **Attention.** Lapses begin early in a lecture and recur; they are fewer during active-learning segments. Bunce, Flens & Neiles (2010) *Journal of Chemical Education*. Supports the pop-ups.
- **Retrieval practice.** Testing beats re-reading for long-term retention. Roediger & Karpicke (2006) *Psychological Science*. Supports pop-ups and the review block.
- **Phones as the enemy.** Texting during lectures lowers note-taking and recall. Kuznekoff & Titsworth (2013) *Communication Education*. Supports the focus lock: the phone must be the lesson or it is the enemy.

On stage, one sentence and one name: "There's twenty years of research on this — productive failure, Manu Kapur — guess first, learn second, remember more." Citation details are from memory of the literature; verify volume and page numbers before anything printed or sent to МОН or UKLO.

## 4. Customers

Three roles in one sale. The person who pays is not the person who uses it, and the person who benefits most cannot buy it.

### 4.1 Buyer — the headmaster

- **Private gymnasium headmaster (beachhead).** Owns the budget, decides alone or with the owner, one meeting and one week. Cares about enrollment — parents choose private schools on reputation, and "AI in every classroom" is a reputation line. Small segment: roughly a dozen private secondary schools in the country, mostly in Skopje (verify count; Bitola may have none, so the first private school may be in Skopje).
- **Public secondary school headmaster (second wave).** Does not own the budget in any meaningful way; the municipality funds the school through block grants, and purchases above small-value thresholds run through public procurement. The headmaster decides whether; the municipality decides if it can be paid. Sales cycle: months. Bitola has seven public secondary schools (verify the list) — the natural first public cluster because the team is here and the municipality is one office.
- What they want to be seen as: progressive, in control, ahead of other headmasters. Who they want to impress: parents (enrollment), the municipality and МОН (funding, standing), other headmasters and local media.
- What they fear: parent complaints about phones, teachers revolting, a tool nobody uses after a month, anything they personally have to maintain.

### 4.2 User — the teacher

About 7,000 secondary teachers in the country (estimate). The champion is the teacher who already tries things — uses Kahoot, runs the school Instagram — one or two per school; they bring TeacherAid in and train colleagues by example. The bottom-up route is the decided go-to-market. The majority are twenty-plus years in, competent, tired of new platforms that add work; they are won only if the first lesson takes less than ten minutes to set up and the class visibly behaves better, and lost at the first login problem. Their pain in their own words is to be replaced with real quotes from the validation log: planning at night, phones under desks, the same five students answering, no idea what the quiet ones did not understand.

### 4.3 End user — the student

15–18, гимназија. Everyone has a phone; most have mobile data. Motivated by peers, rank and not being bored. Will try to game the points, which means they care. Not a customer, but the loudest channel: if students ask for it, the teacher keeps it.

### 4.4 Documented, not in the pitch

Parents (informed by the school through a template notice; the objection to expect is "phones in class"; the answer is that the teacher decides, the app blocks distractions, the school owns the data). МОН as a possible national-program buyer years out. Primary schools — 943 schools, 19,447 teachers, the bigger market — later, because the students are younger, fewer have phones, and any future phone restriction hits there first.

### 4.5 Order of attack

**Macedonia first, not city-by-city.** The team is based in Bitola, but the sale is national from month one.

1. The first private gymnasium (a warm contact exists), wherever in the country it is.
2. Two more private schools nationally, on the strength of the first monthly report.
3. The first public-school pilot through a municipality — any municipality — positioned as a story it can talk about.
4. Every other public secondary school and university faculty in the country, sold in parallel, not sequentially through one city.

## 5. Value propositions

**Teacher — "One tool for the whole lesson, not five."** Today a keen teacher assembles a lesson from ChatGPT (plan), Kahoot (quiz), Mentimeter (poll), a paper list (attendance) and a hope (attention). TeacherAid is the five in one flow built around the 45-minute period. Plan in three sentences. Attention handled by the game, not by shouting. See what the class did not understand — wrong answers collected for the review, anonymous questions from the students who never raise a hand. Time saved is the weak claim; a class that behaves and a teacher who knows what landed is the strong one.

**Student — "A class that starts with a question."** Predict first, learn second. Being wrong earns points for reasoning — the app is on the student's side. The phone is allowed, for once, and it is the game controller. Rank in class, in school, nationally. A private line to the teacher.

**Headmaster — "Does nothing. Gets everything."** "AI in every classroom" as a sentence they can say to parents this year without a training day, an IT project or a login. A monthly one-page report written to be forwarded — the measurable version of praise. Risk-free: first month free, one flat fee covering the whole staff, nothing installed on school infrastructure, the school owns the data.

**Against the alternatives, in one line.** Everything else is a tool for the teacher or a game for the students. TeacherAid is the lesson.

## 6. Business Model Canvas (v2)

Version 1 was the handwritten poster from Saturday morning with revenue, costs and partners empty, "mobile app" under channels, features under activities and "headmaster" under resources. This is the corrected version.

| Block | Content |
|---|---|
| **1. Customer segments** | Buyer: headmasters of private gymnasiums (beachhead), then public secondary via the municipality as payer. User: subject teachers, champions first. End user: students 15–18 on their own phones. Documented but out of the pitch: parents, МОН. |
| **2. Value propositions** | Teacher: one tool for the whole lesson; plan in three sentences; attention handled by the game; anonymous questions. Student: a class that starts with a question; safe to be wrong; points and rank. Headmaster: AI in every classroom with nothing to learn; a monthly report to forward. Differentiator: MK curriculum and language, the 45-minute period as the unit, the headmaster gets a report not a login. |
| **3. Channels** | Teacher referral (bottom-up); direct outreach to private-school headmasters; the free live session in a school; education events and teacher social media; later, municipality introductions. |
| **4. Customer relationships** | In-app tutorials and videos; in-app help and the teacher chatbot with a human behind it; the monthly report; the school-versus-school leaderboard as the retention hook. |
| **5. Revenue streams** | EUR 199 a month, flat per school (EUR 2,388/year), whole staff included, no seat count; invoiced yearly; first month free. Later: sponsorship (telecom or bank CSR), grants (EU, UNICEF, МОН programs), premium content packs. |
| **6. Key resources** | The AI lesson engine and prompt library; the MK curriculum corpus (БРО programs, then teacher-edited lessons — the asset that compounds); the live-session engine including the native focus lock; the team; an advising teacher (missing); AI credits; the trust artefacts (ДПА template, privacy notice, sample report). |
| **7. Key activities** | Build the planner, live session, dashboard and report; curate curriculum content per subject and year; sell to headmasters and run free live sessions; onboard and support teachers; run the leaderboard seasons; report monthly. |
| **8. Key partners** | МОН and БРО (legitimacy, curriculum data); Municipality of Bitola (payer for public schools, door-opener); UKLO Faculty of Education and teacher associations (advising teacher, a small study, credibility); telecom — A1 or Makedonski Telekom (CSR sponsorship, connectivity); the AI provider (credits); EDUINO (integration or ally, not a competitor). |
| **9. Cost structure** | Variable: AI inference (~5 cents per lesson, ~EUR 20 per teacher per year), hosting and realtime per session, support time. Fixed: development, curriculum curation, app-store fees, legal, sales time. Scales with seats: inference and support; with schools: sales; nearly free: the national leaderboard. |

## 7. Revenue and pricing

### 7.1 The model

- **EUR 199 a month, flat per school → EUR 2,388 a year**, invoiced once a year. Whole
  staff included — every teacher, not just volunteers. No seat count, no tiers.
- **First month free**, whole school, no card. Conversion is decided on the first monthly
  report and on how the staff actually uses it.
- Flat per school rather than per seat because it is one number a headmaster approves in a
  single meeting, with nothing to count and nothing to negotiate as staff changes. It also
  makes the school-vs-school leaderboard real from day one — every class in the building is
  on it immediately, not just two volunteers' classes.
- The trade-off: **cost still scales with active teachers, revenue does not** — see 7.2 and
  [[cost-structure]] for the margin-by-school-size risk this creates.

### 7.2 Why EUR 199, and the real trade-off

EUR 2,388 a year for a whole school is less than one interactive whiteboard, and a number a
private-school headmaster can approve without a board meeting. For a public school it
comfortably fits most small-value procurement thresholds for the entire staff (verify the
exact threshold with the municipality). But because AI cost is roughly EUR 20 per *active*
teacher per year while revenue is flat, margin swings hard with school size:

| School size | Teachers | Cost | Revenue | Margin |
|---|---|---|---|---|
| Small | 20 | ~EUR 500/yr | EUR 2,388 | ~79% |
| Average MK secondary | ~55 | ~EUR 1,375/yr | EUR 2,388 | ~42% |
| Large | 80 | ~EUR 2,000/yr | EUR 2,388 | ~16% |
| Very large | 100+ | ~EUR 2,500+/yr | EUR 2,388 | negative |

Decided: no size tiers for now, deliberately, for the simplicity of the sale. Small schools
are very profitable; a handful of very large, heavy-usage schools could run at a loss. A
size tier above a certain teacher count is the fix if the pilot data says so — not before.

### 7.3 Sample school, year one (private gymnasium, ~55 teachers)

| | EUR |
|---|---|
| Revenue (flat) | 2,388 |
| AI inference (55 × ~20) | −1,100 |
| Hosting, realtime, storage | −150 |
| Support time (10 h × 15) | −150 |
| **Contribution** | **~988** |

### 7.4 Market math (North Macedonia, every level, sold nationally)

Revenue is per institution, flat. Clients today are secondary schools and universities;
primary schools are real but not a client — they belong in the TAM ceiling, not in SAM.

| | Institutions | ARR at EUR 2,388/institution | Tier |
|---|---|---|---|
| Secondary schools | 128 | ~EUR 306,000 | SAM |
| Universities | 15 | ~EUR 35,800 | SAM |
| **SAM — secondary + universities, today's real market** | **143** | **~EUR 341,500** | |
| Primary schools (future, not a client today) | 943 | ~EUR 2.25 M | TAM only |
| **TAM — every school and college in MK** | **1,086** | **~EUR 2.6 M / year** | |

Obtainable, honestly (SOM) — sold **nationally from day one, not city-by-city**:

| | Institutions | ARR |
|---|---|---|
| Year 1 (2026/27) | 5 (3 private + 2 public pilots, anywhere in the country) | ~EUR 12,000 |
| Year 2 | 15 schools, nationally | ~EUR 36,000 |
| Year 3 | 40 schools + 5 college faculties, nationally | **~EUR 107,500** (~31% of SAM) |

Note: this ceiling is lower than a per-seat model would give at the same institution
count, because flat pricing is deliberately cheap for a large staff — that's the trade for
a simpler, faster, whole-institution sale. Say the small number on stage. The upside line
for Q&A: the same product works in any country with one national curriculum — Serbia, Bosnia
and Herzegovina, Albania, Kosovo, Montenegro are next and roughly ten times the schools.

### 7.5 Secondary revenue

Sponsorship: a telecom or bank sponsors "AI in Bitola's schools", pays the flat fee for public schools as CSR, gets its name on the leaderboard season. Grants: EU (Erasmus+ and IPA education calls), UNICEF (EDUINO partner), МОН innovation programs — they pay for the public-school rollout municipalities cannot. Premium content packs: ready lesson sets per subject and year, curated with teachers, sold per school, only after the corpus exists.

### 7.6 Payment mechanics

Private schools: invoice, bank transfer, yearly. Public schools: the municipality pays; under the small-value threshold of the public procurement law a direct contract is possible — verify the current threshold and whether software subscriptions qualify, because it decides whether a EUR 2,388 contract is a one-week or a three-month process. No card payments and no teacher-paid plans — the whole point of a flat school fee is that no individual teacher ever sees a bill.

### 7.7 Assumptions to validate first

1. A private-school headmaster says yes to EUR 199/month flat, for the whole staff, without negotiating.
2. Whole-staff rollout doesn't stall on teachers who never opted in — watch real usage rate in the free month.
3. Renewal ≥ 80% after year one.
4. The municipality can contract a EUR 2,388/year pilot directly.
5. The margin holds at real school sizes — instrument AI cost per school from day one.

## 8. Costs

### 8.1 AI inference

Anthropic API prices (first-party, 2026): Claude Sonnet 5 at $2 per million input tokens and $10 per million output tokens; Claude Haiku 4.5 at $1 and $5; Claude Opus 5 at $5 and $25.

One lesson, generously counted: plan generation (system prompt, curriculum excerpt, the teacher's text or plan pages) ≈ 3,000 input and 2,500 output tokens; two chatbot refinements ≈ 4,000 input and 1,000 output; total ≈ 7,000 input and 3,500 output.

| Model | Cost per lesson | Per teacher per year (400 generations) |
|---|---|---|
| Sonnet 5 | $0.049 | ~$20 |
| Haiku 4.5 | $0.025 | ~$10 |
| Opus 5 | $0.12 | ~$49 |

400 generations a year assumes about 20 lessons a week over 36 weeks with reuse across parallel classes and repeats from last year. Prompt caching on the system prompt and curriculum excerpt cuts the input side by roughly 90% on repeated calls; the numbers above ignore it. Recommendation: Sonnet 5 for full plans (Macedonian quality, instruction following), Haiku 4.5 for ad-hoc pop-ups and chatbot small talk. The stage number: about five cents per lesson. Consequence of flat per-school pricing: AI cost scales with active teachers per school while revenue does not — at the MK secondary average (~55 teachers), AI alone is about 46% of the EUR 2,388/year fee. This is the real margin risk of a flat model — see 7.2.

### 8.2 Hosting, support, fixed

- Hosting and realtime: EUR 50–150 per month for the first 20 schools; roughly EUR 2–3 per seat per year after that. App stores: Apple EUR 99 per year, Google EUR 25 once.
- Support and onboarding: videos, in-app tutorials, chatbot, a human behind it. Estimate 10 hours per school in year one at an internal EUR 15 per hour — EUR 150 per school, falling with maturity. The free live session costs half a day of two people and is worth it only where the headmaster meeting is already booked.
- Fixed: the team, unpaid for now with runway undecided — the first hire is a mobile developer for the student app; curriculum curation, about two weeks of one person one-off; legal (ДПА template, privacy policy, terms) EUR 500–1,500 with a lawyer; sales is founder time and local travel.

### 8.3 What gets expensive as it grows

| Cost | Scales with | Mitigation |
|---|---|---|
| AI inference | seats × lessons | caching, cheaper models for simple calls, lesson reuse, content packs |
| Support | schools, teacher churn | onboarding videos, champion teachers, chatbot |
| Sales | schools (each is a meeting) | municipality deals cover many schools per contract |
| Realtime | concurrent sessions at 09:00 | managed vendor, per region |
| National leaderboard | nearly free | — |

### 8.4 Break-even sketch

At EUR 2,388/year per school and an average school (~55 teachers, ~EUR 1,375 variable cost), each school contributes about EUR 1,000/year. A team of three at MK salaries (say EUR 4,500 a month all-in) needs about 54 average-sized schools — beyond the year-3 target of 40. That gap is real; it's closed by grants and sponsorship bridging years one and two, a size tier on large schools if the pilot margin data calls for it, or premium content packs arriving sooner than planned.

## 9. The market and its context

### 9.1 Who decides, who pays

| Body | Role | Why it matters |
|---|---|---|
| **МОН** — Ministry of Education and Science | Policy, law, national programs, e-Дневник, the Office 365 agreement for schools | Legitimacy; a possible national buyer years out; the phone-policy voice |
| **БРО** — Bureau for Development of Education | Writes the curricula per subject and year; teacher training | The curriculum corpus the AI generates inside |
| **ДИЦ** — State Examination Centre | Matura, national testing | Matura-prep packs are a later product |
| **ДПИ** — State Education Inspectorate | Inspects schools | Part of a headmaster's audience |
| **Municipalities** | Since decentralization (2005) they found and finance primary and secondary schools from block grants; the mayor appoints headmasters on the school board's proposal (verify) | The payer for public schools; one office per city |
| **Училишен одбор** — school board | Governs the school, proposes the headmaster | Part of the headmaster's audience |
| **Private schools** | Licensed by МОН, own budgets, tuition-funded | The beachhead |

### 9.2 Secondary education in numbers

128 regular upper secondary schools at the start of 2024/25 (гимназија, стручно, уметничко); 67,143 students; 17,984 first-year enrollments; enrollment down 0.2% year on year and about 20% over the decade. Teachers: about 7,000 (estimate; the decade trend is −5%; get the exact figure from the State Statistical Office before it goes on a slide). Primary and lower secondary: 943 schools, 180,627 pupils, 19,447 teachers. Class sizes are falling and schools are consolidating: nobody buys tools for growth, they buy for quality and reputation, which fits the headmaster pitch.

### 9.3 Phone policy

On 11 May 2026 Minister Vesna Janevska said a ban on phones during lessons in secondary schools "will be discussed", described the current practice — many schools collect phones during lessons and return them at the break — and said the teacher decides: "Доколку наставникот процени дека е потребно да се употреби мобилниот телефон на децата, тогаш тој ќе даде посебна дозвола." She framed educational use as a legitimate reason, noting that not every school has internet, smart boards and tablets. Municipalities, as the bodies responsible for schools, can set their own rules.

What this means: there is no national ban as of September 2026; the rule is teacher's discretion. If a ban comes it will almost certainly carve out teacher-sanctioned educational use — the Minister already described that exception — and TeacherAid is the sanctioned use. The secondary wedge is the safer one; a primary-school restriction is more likely. Keep the quote for Q&A, do not lead with it.

### 9.4 e-Дневник, EDUINO, Office 365

e-Дневник is the national electronic class register where official attendance and grades live; TeacherAid does not replace it and does not write to it in version one; sync is roadmap and depends on МОН. EDUINO is the national digital education platform launched in 2020 with UNICEF — video lessons, games, open resources — a content library, not a live classroom tool, and a partner rather than a competitor. Schools have had Microsoft accounts through МОН since the COVID years; Teams is for distribution and remote lessons, nothing for the 45 minutes in the room.

### 9.5 Money and Bitola

Public schools receive block grants from the state via municipalities; a headmaster has very little discretionary budget; purchases go through the municipality and the public procurement law, with small-value procurements possibly direct (verify threshold). Private schools decide in days on tuition income. Teacher net salary is roughly EUR 600–700 a month (verify). Bitola has seven public secondary schools under the Municipality of Bitola (verify list and headmaster names before outreach); UKLO has a Faculty of Education in the city — the natural research partner; Startup Weekend Bitola itself is a door to mentors, the municipality and local press.

## 10. Competition

| | Plans with AI | Students on phones | Attendance | Focus | Buyer-side report | MK curriculum + language |
|---|---|---|---|---|---|---|
| **TeacherAid** | yes | yes | yes | yes | yes | yes |
| Curipod | yes | yes | no | no | no | no |
| Kahoot / Quizizz (Wayground) / Blooket | partly | yes | no | no | no | no |
| Mentimeter / Wooclap | no | polls | no | no | no | no |
| MagicSchool / Brisk / Eduaide / SchoolAI | yes | no | no | no | no | no |
| ChatGPT / Claude directly | unstructured | no | no | no | no | if prompted |
| Google Classroom / MS Teams | no | distribution | partly | no | admin stats | n/a |
| EDUINO | no | no | no | no | no | content only |

- **Curipod** (Norway) is the closest: AI-generated interactive lessons, students join by code, polls, drawings, word clouds. Strong product, freemium with a paid teacher tier (verify price). It is a teacher tool sold to teachers, English-first, with no attendance, no focus lock, no headmaster report and nothing tied to the БРО programs. If it localized to Macedonian tomorrow it would still be a slide tool, not a lesson system with a buyer-side product.
- **Kahoot** and its variants are great for a ten-minute quiz and nothing before or after it; priced per teacher; schools rarely pay. **Mentimeter** and **Wooclap** could build the opener and nothing else. **MagicSchool**, **Brisk**, **Eduaide**, **SchoolAI** are AI assistants for teachers with no student side, big in the US, not in Macedonian. **ChatGPT plus anything** is what the champion teacher does today — five tabs — and none of the five speak to the headmaster. **Google Classroom** and **Teams** are the school's file cabinet. **EDUINO** is a library. Focus apps (Opal, Forest, one sec) prove the lock mechanism exists and are not classroom products.
- **Where TeacherAid is weaker.** Curipod and Kahoot have years of polish, templates and community; TeacherAid has a weekend. Any of them could add attendance in a sprint. Free tools are free.
- **The moat, honestly.** School relationships and daily teacher habit; the leaderboard network effect — every school added makes the game bigger for the rest; and, building, the corpus of MK lessons edited by real teachers.

## 11. Go-to-market

### 11.1 Channels, in decided order

1. **Direct outreach to headmasters, nationally (now the primary close, updated 13 Sep).** Since pricing moved to a flat, whole-staff fee, the sale is top-down: the headmaster commits the entire staff in one twenty-minute meeting — call, visit, the sample report in hand — not one teacher trying it first. Macedonia first, not Bitola first: the team is based in Bitola, but every private gymnasium and university in the country is a live target from month one.
2. **Teacher referral as the door-opener.** A champion teacher who's excited about it gets you the introduction and vouches for it inside the building, but the deal itself is whole-school from day one — their job shifted from "adopt it, then convince the headmaster" to "introduce us to the headmaster."
3. **The free live session.** Two people, one real class, one lesson, filmed with permission. Now doubles as proof for the headmaster meeting that a whole staff can be onboarded in a day, not just a marketing stunt.
4. **Education events and teacher social media.** Teacher Facebook groups, the annual education conferences, Instagram for the student side.
5. **Municipality introductions** for public schools: one meeting with any municipality's education department opens several schools at once — Bitola's is one such meeting (seven schools), not the only one; the same motion repeats nationally.

Evaluation happens in a whole-school pilot month; the artefacts that close the sale are the sample monthly report and usage across the actual staff, not just one teacher's results.

### 11.2 Relationships

- **Onboard:** because every teacher is on it from day one, not just volunteers, onboarding has to reach the whole staff at once — in-app tutorials, three short videos, a "first lesson in ten minutes" checklist, plus a single kickoff session with the whole staff in week one (the free live session, scaled up). The champion teacher is the in-building point of contact, not the only path in.
- **Support:** in-app help and the teacher chatbot as first line; a founder on WhatsApp behind it for the first schools. Login at 08:05 is the moment that matters — magic links, no passwords to forget.
- **Retain:** teacher habit measured as lessons run per active teacher per week (intervene below two); the monthly report as the headmaster's reason to renew; the school-versus-school leaderboard as a small real switching cost; a renewal conversation built on the year in numbers and the teachers' own words.
- **Signals of trouble:** a school where only the champion is active after month two; focus flags rising (students revoking the lock — the game stopped being worth it); anonymous questions at zero (students do not trust it).

## 12. Partners, resources, activities

| Partner | What we want | What they get | Status |
|---|---|---|---|
| МОН / БРО | Legitimacy; curriculum programs in structured form; eventually a national-program buyer | A tool aligned with their programs; engagement data | Not contacted; the Minister's May 2026 line is the opening |
| Municipality of Bitola | Payer for public schools; introductions to seven schools; a pilot they can publicize | "First municipality with AI in classrooms" | Not contacted; after the first private school |
| UKLO Faculty of Education; teacher associations | An advising teacher; a small study on the opener; credibility with МОН | Research material; a modern tool for their students | Not contacted; the SW education mentor is the first step |
| Telecom (A1, Makedonski Telekom) | CSR sponsorship of public-school seats; possibly zero-rated data | Their name on "AI in Bitola's schools" | Year-two conversation |
| AI provider (Anthropic) | Startup credits; reliability; Macedonian quality | A visible education case | Apply after the weekend |
| EDUINO | Integration or endorsement | A live classroom layer on their content | Not contacted |
| The first private school | The pilot, the first report, the referral | First-mover story, free month, a say in the roadmap | Warm contact, no yes yet |

Partners reduce two risks: legitimacy (МОН, UKLO) and payment (municipality, telecom, grants). Key resources and activities are as listed in the canvas (section 6); the two things missing today are a teacher on or advising the team and a mobile developer for the focus lock.

## 13. Technology

### 13.1 Shape

| Client | Form | Why |
|---|---|---|
| Student app | Native (Expo / React Native recommended, or Capacitor around web views) | Focus lock needs OS APIs a web app cannot touch; the QR deep-links into it |
| Teacher app | Web (PWA, installable), phone-friendly | Planning at home on a laptop, running the class from a phone; no store review cycle |
| Headmaster | Email (PDF report) | No login by design |
| Projector view | Web page opened by the teacher | QR, live bars, reveal, leaderboard |

Backend: one Node/TypeScript service (Next.js API or a small Fastify app), Postgres, a managed realtime layer for the live session (Supabase Realtime, Ably, or a small Socket.IO process), object storage for uploaded plans. EU hosting.

### 13.2 Live session

A session is one lesson with a state machine (lobby → opener → teaching → review → ended) and events: join, vote, reason, popup_open, answer, question_anon, focus_lost, focus_back, reveal, end. Scale per session is tiny (≤ 35 students); across sessions a 50-teacher school runs about 25 concurrently at 09:00 and a country a few thousand — any managed realtime service handles it. The server is the timer authority; clients render the countdown.

### 13.3 AI

One structured call per lesson: system prompt (role, format, Macedonian, the 45-minute frame) + БРО curriculum excerpt for the subject and year + the teacher's text or the relevant pages of the uploaded plan → JSON with opener, pop-ups (question, options, correct, explanation), timing and review summary, validated before saving. Prompt caching for the system prompt and curriculum. Curriculum: the БРО programs are public per subject and year — load once, chunk per unit, retrieve the unit the teacher picked; this is the moat in progress, a corpus of MK lessons and questions that improves with every teacher edit. Student names are never sent to the AI. Generated content is always reviewed by the teacher before students see it.

### 13.4 Data model, offline, the house stack

Minimum tables: schools, users, classes, students, lessons (plan JSON), sessions, attendance, answers, reasons, anon_questions, focus_events, points_ledger, reports. Students use their own mobile data when school wifi fails; the teacher's plan is cached on the device, so a dead network degrades the lesson to verbal with points paused. The teacher web app follows the house defaults — Next.js, Tailwind, Postgres, PWA-first, security headers, mobile-first; the native student app is the one deliberate exception because of focus lock. Not decided: Expo vs Capacitor, the realtime vendor, where the monthly PDF is rendered.

### 13.5 The demo prototype (built 12 September 2026)

A web app opened by QR — no install, no account — that runs the live opener with the judges' phones. Deployed at **https://teacher-aid-bitola.vercel.app**.

- `/host` — projector screen: QR and short URL, joined counter, the question, live bars, reveal with reasons, mini leaderboard, one-line class summary. Keys: Space next step, N next question, R reset (with confirm), S switch to simulation (fake students, no network needed), F fullscreen. `?sim=1` starts in simulation, `?reset=1` resets on load, `?motion=0` disables entrance animations.
- `/j/BITOLA` — the phone: auto nickname → Join → vote → one-line reason → result and points → rank.
- `/print` — A4 sheet with the QR and short URL.
- Content is pre-generated and labelled as generated by TeacherAid for a named topic; the demo never calls an AI live. Questions live in `content/opener.json` in the repo.
- Stack: Next.js 16, Tailwind 4, Upstash Redis on Vercel, polling not websockets (nothing to reconnect on stage). Private repo github.com/predragkirovmk/teacher-aid-app; host key `bitola`.
- Not the product: no focus lock, no AI call, no planning, no dashboard beyond the summary line. In English so the judges read it at the pace of the script. After the weekend the session engine is the seed of the real live-session service.

## 14. Data and privacy

**Stored (minimum viable):** student name and class; attendance per session; answers, points and reasons; anonymous questions (without student id); focus events; teacher lesson plans and uploads. Not stored: grades, student contact details, location, device identifiers beyond a push token, anything from other apps on the phone — focus lock never reads what the student does elsewhere; it only knows TeacherAid is not in the foreground.

**Legal structure.** The school is the data controller (it already processes attendance and participation under its legal mandate); TeacherAid is the processor under a data processing agreement (ДПА) with each school — the standard edtech structure that lets a school adopt the tool without a parental-consent campaign for the core data. Law: Закон за заштита на личните податоци (ЗЗЛП, 2020), GDPR-aligned — lawful basis, minimization, retention limits, breach notification. Verify the exact age of digital consent in ЗЗЛП before launch (GDPR default 16; member states may lower to 13). Parents are informed by the school through a one-page notice; a consent template is provided for schools that want explicit consent. Leaderboards outside the class use nicknames by default; the school chooses.

**Retention.** Session data: current school year plus one, then aggregated and deleted. Monthly reports kept (aggregate, no names). A student who leaves the school: deleted within 30 days on the school's request.

**Security.** EU hosting, encryption in transit and at rest, role-based access (a teacher sees only their classes), no TeacherAid staff access to student data without a ticket and a log; student names never reach the AI provider.

**The parent answer in one breath.** "The school owns the data, we process it under contract, we store the minimum — name, attendance, points — nothing from the rest of the phone, and we delete it when the student leaves. The teacher decides when phones are used, exactly as the Minister said in May."

## 15. Brand

**Working name: TeacherAid.** Used everywhere this weekend. Weaknesses: generic and descriptive, "teacher's aide" is a job title, "aid" reads as first aid or charity, it names the teacher although the loudest channel is students and the buyer is the headmaster, and it says nothing about the lesson, the game or Macedonia. It is fine for Sunday because judges understand it instantly.

| Alternative | Meaning | For | Against |
|---|---|---|---|
| **Zvono / Ѕвоно** (recommended) | the bell | The bell starts every lesson and the game; one word in Macedonian, Serbian, Bosnian, Croatian, Montenegrin — the whole expansion map; mascot-ready | Common noun; check .mk/.com and regional marks |
| Chas / Час | the lesson period | The unit of the product; shortest possible | Generic word, hard to search |
| Predvidi / Предвиди | predict | Names the mechanic and the pedagogy | Long; only the opener |
| Kreda / Креда | chalk | Classic, pan-Balkan | Retro |
| Prvi5 / Првите 5 | the first five minutes | Memorable hook | Names one feature |

Decide after the weekend. Checklist: domain (.mk and .com or .app), no existing edtech product with the name in the region, trademark search at ДЗИС, says well in Macedonian and English, works as an app icon.

**Mascot (concept only): Zvonko** — a small school bell with a face, named after the common name meaning "the one who rings". The teacher chatbot's persona; the little character on student screens that rings for a pop-up, shrugs at a wrong answer and jumps at a streak, never shaming; the mark on the QR screen and the report footer. Flat, two-colour, rounded, readable at 24 px and at two metres.

**Messaging.** The vision line: "Every class starts with a question." Judges: "We turn the most boring 45 minutes of a teenager's day into the one they don't want to miss." Teacher: "Three sentences in. A whole lesson out. And the phones work for you." Student: "Guess first. Being wrong scores." Headmaster: "Does nothing. Gets everything." Against alternatives: "One tool for the whole lesson, not five." Humor beats, one per minute at most and never at a teacher's expense: "Nobody checked Instagram"; "The teacher has a textbook. The students have phones. The headmaster has a speech about digitalization"; "The headmaster's favourite feature is that there isn't one." Words to avoid: revolutionize, disrupt, empower, gamify, AI-powered as a value, platform, ecosystem, solution, "we will change education".

## 16. Risks and assumptions

| # | Assumption | Evidence needed | How | Status |
|---|---|---|---|---|
| 1 | A private-school headmaster will pay EUR 199/month flat for the whole staff | One verbal yes | Ask the warm contact this week; ask the education mentor if the number is sane | Open |
| 1b | A large school stays profitable at the flat fee — AI cost could exceed EUR 2,388/year revenue if usage is heavy | Measured AI cost per school after 100+ lessons | Instrument from day one; add a size tier if the data says so | New |
| 2 | Teachers will type three sentences or upload a plan before class | 3 of 5 interviewed teachers say it is less than they do now | Every teacher conversation, logged | Partly — teachers interviewed, quotes to log |
| 3 | Students will authorize focus lock on their own phones | ≥ 70% authorize in week one of the pilot | Pilot class | Untested |
| 4 | Focus lock is deliverable on iOS and Android | Working build; Apple entitlement granted | Build after the weekend; start the entitlement request early, it takes weeks | Mechanism documented, untested |
| 5 | No national phone ban arrives for secondary schools | МОН statements | Monitor; build the МОН relationship | Currently favorable (May 2026) |
| 6 | Parents do not block it | No complaint reaching the headmaster in the pilot | Parent notice template; nickname boards; school as controller | Untested |
| 7 | The monthly report is enough to make a headmaster renew | Renewal after the free month; the report forwarded at least once | Pilot | Untested |
| 8 | AI generates usable Macedonian lessons inside the БРО programs | Teacher rates ≥ 4/5 on 10 generated lessons without heavy edits | Build the generator; test with the advising teacher | Untested |
| 9 | Municipalities can contract a small pilot directly | Confirmed threshold and procedure | Ask the municipality education department | Unknown |
| 10 | School wifi failure does not kill the lesson | Mobile data works in a concrete building | Free live session in a Bitola school | Untested |
| 11 | The leaderboard motivates rather than shames | Pilot survey; focus flags do not rise | Pilot; nickname option | Untested |
| 12 | AI cost stays ≤ EUR 20 per seat per year | Measured token usage after 100 lessons | Instrument from day one | Estimate |
| 13 | Curipod does not localize to Macedonian before we have 20 schools | Their language list | Quarterly check | Open |
| 14 | A teacher on the team or advising by end of September | A named person | UKLO, the mentor, the interviewed teachers | Open |

Risks that will happen regardless: the veteran teacher who refuses (the flat whole-staff fee means the school pays regardless, so nobody has to opt in — the veteran just comes around slower); points gaming (speed bonus, per-device sessions, and teacher discretion on grades removes the stakes); support at 08:05 (magic links, champion teacher as first line); a data incident (minimal data and controller/processor structure keep the blast radius to names and points); a small market (MK is the proving ground; every neighbor has one national curriculum).

## 17. Startup Weekend Bitola — the pitch

### 17.1 Format and criteria

Final pitches Sunday 13 September 2026, House of the Army, Bitola: five minutes plus five or more minutes of Q&A, judges are local founders and investors, criteria not announced — plan for the standard Techstars three, each scored 1–5 and weighted equally: customer validation (did you leave the building, who said what, what changed because of it — the weakest score today), execution and design (what works after 54 hours — the live QR demo), business model (who pays, how much, what it costs, how big — one slide). Deck in Canva by the presenter; present from a browser tab with the demo in the adjacent tab.

### 17.2 Structure and timings

| Time | Block | What happens |
|---|---|---|
| 0:00 | Live opener (~60 s) | Judges scan the projected QR, vote on "Will Bitola see snow before the first of December?", type one word why; reveal; mini leaderboard |
| 1:00 | Hook | "You just did the first ninety seconds of a TeacherAid class. Notice what didn't happen: nobody checked Instagram." |
| 1:20 | Problem | Three people walk into a Macedonian classroom: a teacher, thirty students, a headmaster who is not in the room |
| 1:55 | Solution | The 45 minutes from the bell: QR, opener, pop-ups and focus, review and anonymous questions; class, school, national leaderboards |
| 3:00 | Teacher and headmaster | Three sentences in, a lesson out; the headmaster does nothing and gets the monthly report |
| 3:35 | Business | EUR 199 a month, flat, per school — whole staff included; EUR 2,388 a year; first month free; private gymnasiums first; 128 schools, ~7,000 teachers, 67,000 students |
| 4:15 | Validation and why now | The teacher quote; the Minister's "the teacher decides"; a lesson costs five cents |
| 4:40 | Close | "Every class in Macedonia starts the same way: a teacher talking, thirty heads looking down. We want every class to start with a question." No ask. |

Lines never cut: "Nobody checked Instagram." / "Does nothing. That's the feature." / "Five euros per teacher per month." / "We want every class to start with a question." The full word-for-word script, the slide-by-slide outline, the cut order and the rehearsal plan are in the vault's project folder.

### 17.3 Demo choreography and fallbacks

Before walking up: tab 1 is the host screen at `/host` showing the QR (session reset), tab 2 is the Canva deck in Present mode; the presenter's own phone is already joined as a backup voter; a printed QR (from `/print`) lies on the judges' table. Fallbacks in order: fewer than three judges scan in 15 seconds — vote from your own phone and move on; a phone will not scan — point at the short URL; the host page stalls — press S for simulation and say "the venue wifi has opinions, here's the morning run"; the laptop dies — Canva on a teammate's laptop, slide 2 carries a screenshot of the reveal; the projector dies — present without slides.

### 17.4 The eight questions most likely to come, in one line each

1. Why not ChatGPT plus Kahoot for free? — Five tabs and a paper list, and none of them talk to the headmaster; the school pays for the whole lesson and the report.
2. How do you block apps on a personal phone? — Apple's Screen Time API and Android usage access, the mechanism Opal and AppBlock use, with the student's one-time permission; revoking shows as a focus flag.
3. Phones are being banned across Europe. — In May the Minister said the teacher decides; any ban carves out educational use; that is why we start in secondary.
4. Who pays in a public school? — The municipality; which is why private gymnasiums come first; a EUR 2,388/year flat-fee pilot fits under small-value procurement (confirming next week).
5. The market is tiny. — Yes, EUR 420k a year if we had every secondary teacher; the right first market because one curriculum, one language, one ministry; Serbia alone is ten times the seats.
6. What did you validate? — [N] teachers, their quotes, what we changed because of them, the headmaster call booked.
7. Curipod exists. — A slide tool sold to teachers in English, with no attendance, focus or report; our moat is school habit and the school-versus-school board.
8. Minors' data? — The school is the controller, we are the processor, name-attendance-points only, EU hosting, deleted when the student leaves.

### 17.5 Next actions before the pitch

Headmaster call (ten minutes, one quotable sentence); log tonight's teacher quotes and pick one for slide 10; ten minutes with the education mentor on the three-sentence input and points-to-participation-grade; ask the organizers for criteria, slide deadline, browser-tab presenting and Q&A length; room test on Sunday morning (projector, QR readable from the judges' seats, phones on mobile data); print the QR twice; build the deck; rehearse three times with a stopwatch; assign who takes the technical and the money questions in Q&A.

## 18. Roadmap

- **The week after (by 20 September):** recruit an advising teacher (the interviewed teachers, UKLO, the mentor); book the headmaster meeting with the sample report and the price; apply for AI provider startup credits; decide the name; start the Apple Family Controls entitlement request.
- **October–December 2026:** first private school on the free month; the AI planner generating real Macedonian lessons inside the БРО programs for one subject; the native student app with focus lock in a test build; the first monthly report sent; one free live session in a Bitola public school; the ДПА and privacy notice with a lawyer.
- **Year 1 (school year 2026/27):** three private schools and two public pilots, about 250 seats, about EUR 15,000 ARR; the municipality conversation; a grant or sponsorship application; e-Дневник sync scoped with МОН.
- **Year 2:** fifteen schools, about 750 seats; Albanian-language UI if a school needs it; premium content packs from the corpus; the first neighboring-country pilot only if a partner brings it.
- **Year 3:** 45 institutions nationally — 40 schools plus 5 college faculties, sold across the country rather than city-by-city — about EUR 107,500 ARR, roughly a third of the real serviceable market (secondary schools + universities); break-even needs more than this alone, see [[cost-structure]].

## 19. Team and open roles

Four or more people this weekend; the presenter (pitch, deck, script) presents alone; the demo prototype is built. Roles to name in the deck: product and pitch, engineering, sales and school relationships, content and pedagogy. Missing today and to be filled first: a teacher on or advising the team (every claim about classrooms needs one person who has stood in front of thirty 16-year-olds), and a mobile developer for the focus lock. Runway is undecided; the first paid role is the mobile developer.

## 20. Glossary

- **МОН** — Министерство за образование и наука, the Ministry of Education and Science.
- **БРО** — Биро за развој на образованието, the bureau that writes the national curricula (наставни програми).
- **ДИЦ / ДПИ** — State Examination Centre / State Education Inspectorate.
- **e-Дневник** — the national electronic class register (attendance, grades).
- **EDUINO** — the national digital education platform (МОН with UNICEF), a content library.
- **Гимназија** — the four-year general secondary school; **стручно** vocational; **уметничко** arts.
- **Годишен / тематски план** — a teacher's yearly / thematic lesson plan.
- **Училишен одбор** — the school board.
- **ЗЗЛП** — Закон за заштита на личните податоци, the personal data protection law (GDPR-aligned, 2020).
- **ДПА** — data processing agreement between a school (controller) and TeacherAid (processor).
- **ДЗИС** — State Office of Industrial Property (trademarks).
- **UKLO** — University St. Kliment Ohridski, Bitola.
- **Jавни набавки** — public procurement.
- **Seat** — one teacher's yearly license; **opener** — the first five minutes; **pop-up** — a mid-lesson question; **focus flag** — the signal that a student left the app.

## 21. Sources

- State Statistical Office, "Primary, lower secondary and upper secondary schools at the beginning of the school year 2024/2025" (07.04.2025): https://www.stat.gov.mk/PrikaziSoopstenie_en.aspx?rbrtxt=17
- Eurydice, statistics on educational institutions, Republic of North Macedonia: https://eurydice.eacea.ec.europa.eu/eurypedia/republic-north-macedonia/statistics-educational-institutions
- Lokalno.mk on the SSO release (teacher counts): https://lokalno.mk/se-povekje-nastavnici-se-pomalku-uchenici-zagrizhuvachki-podatoci-od-drzhavniot-zavod-za-statistika/
- Radio MOF, decade trends in pupils and teachers: https://www.radiomof.mk/za-edna-decenija-27-iljadi-pomalku-uchenici-vo-makedonija/
- Republika.mk, Minister Janevska on phones in secondary schools, 11.05.2026: https://republika.mk/vesti/makedonija/janevska-najavi-liczenczirane-na-uchilishnite-psiholozi-ke-se-razgovara-za-zabrana-na-telefoni-na-nastava-vo-srednite-uchilishta/
- Anthropic API pricing, 2026 (Sonnet 5, Haiku 4.5, Opus 5) as used in section 8.
- Research citations in section 3 (Kapur; Sinha & Kapur; Kornell, Hays & Bjork; Richland, Kornell & Kao; Butterfield & Metcalfe; Metcalfe; Bunce, Flens & Neiles; Roediger & Karpicke; Kuznekoff & Titsworth) — verify details before printing.
