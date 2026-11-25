---
created: 2026-11-24
type: area
status: active — the plan to make teacheraid.education findable in Google
---

# Ranking plan for teacheraid.education

The site is in Google: Search Console's URL Inspection shows the home page as "URL is on
Google" (checked 2026-11-24). It does not show up because of two problems, and neither is
technical.

## Why it does not show up (2026-11-24)

1. **The name collides with a giant search term.** Type "teacheraid" into Google and it suggests
   "teacher aide": teaching-assistant jobs, courses and salaries, mostly in Australia. It does
   the same for "teacheraid education". Google reads our brand as a misspelling of a common
   phrase (Google autocomplete, hl=mk, 2026-11-24). Until Google learns TeacherAid is a thing of
   its own, a search for the name alone lands on other people's pages.
2. **Nothing points to the site.** The domain is weeks old and almost no other site links to it
   or mentions it. Google ranks a page partly by who vouches for it. Bing has not indexed it at
   all yet (`site:teacheraid.education` returns nothing there, 2026-11-24).

What was checked and is fine: the sitemap (8 URLs, all 200, self-canonical), robots.txt,
Search Console verification (DNS TXT record), DNS, HTTPS, structured data, and that nothing is
set to noindex by mistake. The two "not indexed" rows in Search Console are a correct redirect
(`www`/`http` to the main address) and the student app's sign-in redirect. The app is now kept
out of crawling (`robots.txt`, `Disallow: /app/`).

## What people actually search (Google autocomplete, Macedonian, 2026-11-24)

- `kahoot mk`: teachers look for Kahoot in Macedonian. `/alternativa-na-kahoot` answers this.
- `дигитални алатки во наставата` (digital tools in teaching). No page yet.
- `вештачка интелигенција во образованието` (AI in education). No page yet.
- `е дневник ...`: very common, but these people want to log into e-Дневник. Not our query.

Macedonian search volume is small. Search is a supporting channel; schools are won in the room.
The goal is that **anyone who hears of TeacherAid and searches for it finds it**, and that a
teacher looking for classroom tools in Macedonian meets it.

## What the team does (only people can do these)

In order of impact.

1. **Get real sites to link to teacheraid.education.** This matters most. Each link is a vote.
   - Every school where a free lesson runs: ask the school to post a short news item on its own
     website with a link. School and university sites (`.edu.mk`) carry weight.
   - Startup Weekend Bitola, its organisers and mentors: a recap post or a link on their page.
   - УКЛО (the university in Bitola): a faculty news item, a student project, a talk.
   - Local and tech media: a short story about a Bitola team building for Macedonian schools.
   - Never buy links or swap them in bulk; Google punishes it.
2. **Profiles under exactly the same name, all linking back.** LinkedIn company page, Facebook
   page, Instagram, a YouTube channel with one 60-second demo in Macedonian. Same name
   ("TeacherAid"), same one-line description, same link. Send the profile links to Claude, who
   adds them to the site's structured data (`sameAs`), so Google joins them into one entity.
3. **Google Business Profile** as a service-area business in Bitola (we go to schools). It can
   put TeacherAid on the map and in a panel beside brand searches. Check eligibility when signing
   up (verify).
4. **Write the address, not just the name.** On slides, flyers, emails, signatures and QR codes,
   use `teacheraid.education`. Anyone who types the name alone meets "teacher aide".
5. **Search Console, once a week.** URL Inspection → Request indexing for each of the 8 pages
   (once). Then watch Performance → Queries: which searches show us, and where.
6. **Bing Webmaster Tools**: import the site from Search Console. Bing also feeds ChatGPT search
   and Copilot.
7. **A teacher's first-hand writing.** Once a month, a teacher on or near the team writes or
   reviews one piece from real lessons: what happened, what students got wrong, what changed.
   Google rewards first-hand experience over generic advice. Photos of the board or the teacher,
   never of students without written consent from their parents.
8. **The name decision.** The collision with "teacher aide" will always make brand searches
   noisy. That belongs in [[name-and-mascot]] when the name is decided. Not a reason to rename by
   itself.

## What Claude does (in the code and the vault)

1. Keep the student app out of crawling: `robots.txt` disallows `/app`, `/app?` and `/app/`
   exactly, so the icons and the manifest stay crawlable. **Live 2026-11-25** (app PR #19, merged).
2. Tell Google the site's name and its short forms (WebSite `alternateName`: "TeacherAid
   Македонија", "teacheraid.education"), so results show "TeacherAid", not a guess. **Live 2026-11-25** (app PR #19).
3. Add each official profile to `sameAs` as soon as the team sends the links.
4. Write two new pages for the real queries above, in Macedonian first: "Дигитални алатки во
   наставата" (an honest guide to the tools a Macedonian teacher can use, ours among them) and
   "Вештачка интелигенција во образованието" (what AI can and cannot do in a Macedonian lesson,
   and how student data is protected). Each is reviewed by a teacher before it goes live.
   **Written 2026-11-25**, app PR #19: `/digitalni-alatki-vo-nastavata` and
   `/vestachka-inteligencija-vo-obrazovanieto` (English at `/en/digital-tools-for-teaching`,
   `/en/ai-in-education`). **Live 2026-11-25** (app PR #19). A teacher should still read them; corrections go in a new PR.
5. Link the reading pages from the home page itself, not only from the footer. **Live 2026-11-25**: a "Прочитајте повеќе" list before the booking scene.
6. Speed up the home page on phones: its largest paint is about 4.4 s on a slow phone
   (Lighthouse, 2026-10-31), held back by the hero's entrance animation. **Measured 2026-11-25**:
   4.1 to 4.2 s, performance 75 to 78, SEO 100, CLS 0. The largest element is now the hero's
   laptop capture; it loads in about 50 ms and waits about 460 ms for the motion scripts. The
   rest is a design change to the hero, left as its own task.
7. Tell Bing and IndexNow after every website change (`node scripts/indexnow.mjs`). **Done
   2026-11-25**: all 12 pages accepted (HTTP 200).
8. Once a month, re-run the SEO checks (`/seo`, `/geo`) and record what changed here.

9. A press page, so anyone writing about us has the facts, logos and the link in one place:
   `/za-mediumi` and `/en/press`. **App PR #20, 2026-11-25.**
10. Ready-to-send outreach texts in Macedonian (a school news post, emails to directors,
    Startup Weekend Bitola, УКЛО and media, a LinkedIn post): [[outreach-texts]].

## How we will know it works

- In 2 to 6 weeks: searching `teacheraid.education`, `TeacherAid Битола` and `TeacherAid
  Македонија` shows the site first. Search Console lists all 8 pages as indexed.
- In 2 to 3 months: impressions in Search Console for `kahoot`, `дигитални алатки` and
  `вештачка интелигенција во образованието` queries; at least 5 sites linking to us.
- Searching `teacheraid` alone is the last to come, and depends on links, mentions and time.

## Related

[[seo-geo-plan]] · [[name-and-mascot]] · [[channels-and-relationships]]
