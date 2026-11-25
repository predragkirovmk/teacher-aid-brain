---
created: 2026-11-25
type: area
status: snapshot — GEO audit of teacheraid.education, /seo-geo and /seo-google (claude-seo v2.5.0)
---

# GEO analysis, 2026-11-25

How ready teacheraid.education is to be found and quoted by AI search (Google AI Overviews and
AI Mode, ChatGPT search, Perplexity, Claude, Copilot). Method: the `seo-geo` and `seo-google`
skills, run against the live site on 2026-11-25. Google's own AI optimization guide is the
authority: optimizing for AI search "is still SEO", so findings are SEO fundamentals seen through
AI answers.

## GEO readiness: 60 / 100

| Area (weight) | Score | Why |
|---|---|---|
| Citability (25%) | 60 | A plain "TeacherAid е апликација за часот…" definition exists (FAQ, footer, llms.txt), facts are specific and dated, 3 pages cite sources. No original data of our own yet. |
| Structure (20%) | 75 | One H1 per page, 20 question headings on the FAQ, a real comparison table, lists, short paragraphs. The home page's headings are slogans, not questions. |
| Multi-modal (15%) | 45 | Real app screenshots in drawn devices, a downloadable logo kit. No video, no diagram. |
| Authority and brand (20%) | 25 | Dates and sources on every reading page. No team names (decided 2026-10-31). No outside presence: no LinkedIn, YouTube, Instagram or Facebook page found; the only outside mention found is the public GitHub repository. |
| Technical access (20%) | 90 | All 12 pages complete without JavaScript (437–1,517 words each), self-canonical, hreflang paired, structured data on every page, every AI crawler allowed, `/app` closed. No RSL licence file (optional). |

These are claude-seo's heuristics, not Google's internal signals.

## Platform readiness (not measured)

No tool measured real citations this time: a DataForSEO or SE Ranking account would be needed.
Qualitatively:

- **Google AI Overviews and AI Mode**: eligible. The home page is indexed (Search Console, 2026-11-24)
  and the "Search generative AI" setting should be on Include. They cite pages that already rank,
  so this follows ordinary ranking.
- **ChatGPT search, Copilot**: fed by Bing. All 12 pages were submitted through IndexNow on
  2026-11-25 (accepted); Bing had indexed nothing before.
- **Perplexity, Claude**: their search crawlers are allowed; both lean on brand presence on the web,
  which is the weakest area.

## AI crawler access (robots.txt, 2026-11-25)

Each bot governs a different thing, so each is listed on its own.

| Crawler | Governs | `/` and the reading pages | `/app` |
|---|---|---|---|
| Googlebot | Google Search, AI Overviews, AI Mode | allowed | blocked |
| Google-Extended | Gemini training and grounding (not Search) | allowed | blocked |
| OAI-SearchBot | ChatGPT search citations | allowed | blocked |
| GPTBot | OpenAI model training | allowed | blocked |
| ChatGPT-User | ChatGPT browsing for a user | allowed | blocked |
| Claude-SearchBot | Claude search citations | allowed | blocked |
| ClaudeBot | Anthropic model training | allowed | blocked |
| Claude-User | Claude browsing for a user | allowed | blocked |
| PerplexityBot | Perplexity search | allowed | blocked |
| Applebot / Applebot-Extended | Siri and Spotlight / Apple training | allowed | blocked |
| Bingbot | Bing, Copilot, ChatGPT search's index | allowed | blocked |
| CCBot | Common Crawl (training sets) | allowed | blocked |

`Content-Signal: search=yes, ai-input=yes, ai-train=yes`, as the founder decided on 2026-10-31.

## llms.txt

Present, 6.9 KB, generated from the page copy, lists all 12 pages. It carries no weight in the
score: Google says Search ignores it, and no AI search engine has confirmed reading it.

## Brand mentions

| Platform | Found |
|---|---|
| LinkedIn, YouTube, Instagram, Facebook | none |
| Wikipedia, Wikidata, Reddit | not checked: their APIs refuse requests from this cloud sandbox. None is known to exist. |
| GitHub | the public repository `predragkirovmk/teacher-aid-app`; its pull request descriptions show up in web search for the name, including internal notes such as "no published pricing because it has not been validated". |

## Passages (geo-seo-claude scorer, English pages only)

The scorer only reads English, so these numbers are a direction, not a grade: `/en` 36.6,
`/en/faq` 42.3, `/en/kahoot-alternative` 42.8, `/en/digital-tools-for-teaching` 41.2,
`/en/ai-in-education` 38.8, `/en/about` 37.4.

## Google data (/seo-google)

- **Credentials**: none configured in the cloud, so Search Console, CrUX and GA4 could not be
  read. Setup is in [[search-console-access]].
- **PageSpeed Insights API**: the shared keyless quota was used up for the day (HTTP 429). With an
  API key it works from the cloud too.
- **Lab data instead** (Lighthouse 13.6, mobile, through the cloud proxy, 2026-11-25): the live
  site scores 82–85 with LCP 3.0–3.1 s; the preview with app PR #20 scores 87–88 with LCP
  2.1–2.4 s. CLS 0 on both.
- **Google update ledger**: last verified 34 days ago (stale); the latest product entries (to
  2026-09-24) include Search Console's multimodal reporting. Nothing in it explains the site's
  visibility, which is a new-site problem.

## Top 5 changes, highest impact first

1. **Outside presence, under the same name** (team). A LinkedIn company page and a YouTube
   channel with one 60-second demo in Macedonian. Send the links to Claude for `sameAs`.
   Authority is the weakest area and these move it most.
2. **The first independent mentions** (team). Use [[outreach-texts]] after every free lesson,
   and once each for Startup Weekend Bitola, УКЛО and local media. The press page (app PR #20)
   gives them everything they need.
3. **A first-hand page with real numbers** (team gives the facts, Claude writes). After the first
   lessons in a school: what happened, how many answered, what the class got wrong, dated. Our own
   data is what makes a page worth quoting; it is what Google's guide asks for.
4. **The demo video on the site** (Claude, once it exists). Embed it on the home page and the FAQ
   with VideoObject markup.
5. **Decide whether the app repository stays public** (founder). Its pull request descriptions are
   the only outside text about TeacherAid in search today, internal notes included.

Done in this audit: the AI guide's section headings are now the questions people search
(app PR #20).

## Related

[[ranking-plan]] · [[seo-geo-plan]] · [[search-console-access]] · [[outreach-texts]]
