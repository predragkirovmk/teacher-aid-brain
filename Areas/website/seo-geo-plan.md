---
created: 2026-10-31
type: area
status: built 2026-10-31; PR predragkirovmk/teacher-aid-app#1
---

# SEO and GEO plan for teacheraid.education

How teacheraid.education gets found in Google and Bing, and how it gets cited by ChatGPT,
Perplexity, Claude, Gemini and Google's AI Overviews. Built from two Claude Code skill packs,
tested against each other and against the live site on 2026-10-31.

## The two packs

| | geo-seo-claude (zubair-trabzada) | claude-seo v2.5.0 (AgriciDaniel) |
|---|---|---|
| Dated | February 2026 | October 2026 |
| Leans on | Market statistics, a 2024 GEO paper, agency scoring | Google's own docs (AI optimization guide, 2026-05-15, updated 2026-07-10), dated studies |
| Good at | Citability scoring per passage, brand-entity thinking | Crawler-by-purpose precision, schema status, agent readiness, honesty rules |
| Weak at | Repeats claims Google has since rejected; built to sell audits to clients | Little on writing; mostly audit |

## Baseline (live site, 2026-10-31)

- Two indexable URLs: `/` (Macedonian) and `/en`. Everything else is the app (noindex) or demo
  (blocked in robots.txt).
- Server-rendered, canonical and hreflang correct, OG images present, all images have alt
  text, security headers strong. The technical floor is good.
- **No structured data at all.** No Organization, no WebSite, nothing that tells a machine
  what TeacherAid is.
- **Citability:** average 35/100 on `/en`, 23/100 on `/` (geo-seo-claude scorer, 16 passages
  each). Headings are poetic ("One click. Every phone.") and match no question anyone types.
  No paragraph says plainly what TeacherAid is.
- Title and description carry the slogan, not what people search for (quiz, attendance,
  QR, AI, lesson, school).
- No `/llms.txt`.
- **The name collides**: TeachAid (an AI lesson tool), Teacher Aide (iOS), Teacher Aide Pro
  (attendance app). Machines must be told which TeacherAid this is: Bitola, North Macedonia,
  secondary schools and universities.
- **The gap that matters most:** a search in Macedonian for a Kahoot alternative, classroom
  quizzes or QR attendance returns no Macedonian-language source. An answer engine asked in
  Macedonian today has nothing local to cite. TeacherAid can be the first.

## The battle: what survived

Rule: where a pack contradicts Google's published guidance, Google wins. Where neither has a
primary source, the advice survives only if it also helps a human reader.

| Advice | Source pack | Verdict | Why |
|---|---|---|---|
| Write content only TeacherAid can write: first-hand, specific, in Macedonian | both (claude-seo cites Google) | **Keep, core** | Google's AI guide names unique, non-commodity content as what matters. Our Macedonian answers have no competition. |
| Plain definition near the top ("TeacherAid is ...") | both | **Keep** | Helps people and answer engines; costs one paragraph. |
| Question-shaped headings and self-contained answer paragraphs | both | **Keep, without a word count** | geo's "134–167 words" is a heuristic; Google says don't chunk for AI. Write each answer to stand alone because a teacher reads it alone. |
| Organization + WebSite + WebApplication JSON-LD with `sameAs` | both | **Keep** | Entity disambiguation is the real fix for the name collision. `sameAs` only to profiles that exist. |
| BreadcrumbList on sub-pages | claude-seo | **Keep** | Active, cheap. |
| FAQPage schema | geo | **Drop** | Google retired FAQ rich results for all sites on 2026-05-07; no confirmed AI benefit. The visible FAQ stays; the markup doesn't. |
| HowTo schema, `speakable` | geo | **Drop** | HowTo deprecated 2023; speakable is still beta and news-only in practice. |
| `llms.txt` "positions you ahead of competitors" | geo | **Keep the file, drop the claim** | Google: ignored by Search. No AI provider has confirmed reading it. It costs nothing, Lighthouse's Agentic Browsing audit checks it, and coding/research agents do fetch it. Lowest priority. |
| Allow AI search crawlers (OAI-SearchBot, Claude-SearchBot, PerplexityBot) | both | **Keep (already true)** | robots.txt allows everything. Training crawlers are a separate decision (see below). |
| Treat GPTBot and OAI-SearchBot as one | geo (scorer lists only GPTBot-era names) | **Drop** | Different purposes. Report and decide per purpose. |
| Wikipedia article | geo | **Drop** | A pre-launch startup fails Wikipedia notability, and a founder-written article breaks its conflict-of-interest rules; it would be deleted. |
| Wikidata item | geo | **Later** | Only after independent press exists to reference. |
| Brand mentions on YouTube, LinkedIn, local press, teacher Facebook groups | both | **Keep, off-site** | Ahrefs (2025-12) found mentions correlate with AI visibility far more than backlinks. A one-minute Macedonian demo on YouTube is the single best off-site move. Real mentions only, never seeded. |
| IndexNow | geo, claude-seo technical | **Keep, small** | Bing's index feeds ChatGPT search and Copilot. A key file plus one ping per deploy. |
| Content freshness and `dateModified` | geo | **Keep honestly** | Real dates when content really changes. Google lists faked freshness as a warning sign. |
| GEO market-size tables, "4.4x conversion" | geo | **Drop** | Sales material, no bearing on the site. |
| Comparison page ("Kahoot alternative") | claude-seo competitor-pages | **Keep if approved** | High-intent query with no Macedonian answer. Fair, factual, dated. |
| Content-Signal line in robots.txt | claude-seo agentic | **Only with a policy** | Draft standard. Worth adding once the training choice is made. |

## Decisions (founder, 2026-10-31)

1. The two skill packs are committed to the app repo (`.claude/skills`), not installed per
   machine.
2. New pages: FAQ, Kahoot comparison, About. A school-facing data page and a "how a lesson
   runs" page were not chosen for now.
3. No team names and no social profiles on the site yet. `sameAs` stays empty until official
   profiles exist.
4. AI crawlers may search, cite and train (`Content-Signal: search=yes, ai-input=yes,
   ai-train=yes`). Claude merges the PR once the Vercel preview passes.

## What was found while building (2026-10-31)

- **geo-seo-claude's citability scorer is English-only.** It rewards English regex patterns
  ("X is a", "according to", capitalised names, `$` figures) and 134–167-word blocks, so
  Macedonian text scores low whatever its quality. Its numbers are directional for English
  pages at best. This was the decisive point against the pack's scoring.
- **claude-seo's setup downloads Chromium into its own folder.** It is now gitignored; it
  nearly went into the repo.
- **Curipod has pivoted** to "writing practice with feedback" (curipod.com/pricing,
  2026-10-31), so it is no longer the closest competitor and is not on the comparison page.
- Quizizz is now Wayground (since June 2025).
- Kahoot's free-plan participant cap is stated inconsistently on its own page (10 and 40),
  so the comparison page quotes only its paid entry plan (USD 3 a month, billed yearly).

## Results

| | Before (live, 2026-10-31) | After (production build) |
|---|---|---|
| Indexable pages | 2 | 8 (4 per language) |
| Structured data | none | Organization, WebSite, WebApplication, WebPage, Breadcrumb on every page |
| llms.txt | 404 | generated from the page copy, passes Lighthouse's rule |
| Lighthouse, new pages (mobile) | – | 100 on performance, accessibility, best practices, SEO and agentic browsing |
| Lighthouse, home (mobile) | performance 68–74 | 67–73 (no change; LCP is the hero's entrance animation) |
| Citability, English (geo scorer, directional) | home 35.5 | FAQ 42.3, comparison 42.8, about 37.4 |

## What was built (app repo, branch `claude/trusting-fermat-xqvmdd`)

Every page in Macedonian first, then English, with hreflang pairs.

1. **Home, both languages:** keyword-bearing title and description; a plain "what it is"
   paragraph; one sentence per chapter that says the fact under the poetic heading.
2. **Structured data:** Organization (Bitola, North Macedonia; email; logo; `sameAs`),
   WebSite, WebApplication (education, browser, no price), WebPage, BreadcrumbList.
3. **New pages:** `/prasanja` and `/en/faq`, `/alternativa-na-kahoot` and
   `/en/kahoot-alternative`, `/za-nas` and `/en/about`.
4. **Sitemap** with every page and its real date; **llms.txt**; IndexNow key and ping script.
5. **Verification:** build, lint, typecheck, 745 unit tests, 148 end-to-end tests, Lighthouse,
   and both packs re-run against the build. Results above.

Pricing stays off the site: EUR 199 a month has not been validated with a single
headmaster ([[revenue-and-pricing]]).

## Off-site (the founder does these; Claude can't)

1. Google Search Console: verify the domain, submit the sitemap, leave "Search generative AI"
   on include. Then run `node scripts/indexnow.mjs` in the app repo.
2. Bing Webmaster Tools: import from Search Console. This one step covers Bing, Copilot and
   ChatGPT search.
3. LinkedIn company page, then a YouTube channel with one 60-second demo in Macedonian.
4. Ask Startup Weekend Bitola and any outlet that covered it to link to the site.
5. Wikidata once two independent articles exist.

## Next content worth writing

- A school-facing data and privacy page (ЗЗЛП, the processing agreement): a real query
  with no Macedonian answer today.
- A one-minute demo video in Macedonian, embedded on the home page and on YouTube.
- When the first pilot runs: a real lesson report with the school's permission. That is the
  first-hand content Google's guide asks for.

## Related

[[product-overview]] · [[messaging]] · [[competitors]] · [[name-and-mascot]] · [[prototype]]
