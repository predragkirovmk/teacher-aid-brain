---
created: 2026-11-25
type: area
status: ready to use — open each profile, paste, upload, send the links to Claude
---

# Social profile kit

Everything needed to open TeacherAid's profiles in about 30 minutes: the pictures at the right
sizes, one bio in three lengths, the first three posts, the YouTube video with its text, and the
Google Business Profile wording. Why it matters: in the GEO audit ([[geo-analysis-2026-11-25]]),
outside presence was the weakest area (25/100). Same name, same words and same link everywhere,
so Google joins the profiles into one TeacherAid.

Every fact here is already on teacheraid.education. No team names, no pricing (decided
2026-10-31). Profiles are opened by a person on the team; Claude cannot create accounts.

## The rules for every profile

- **Name:** `TeacherAid`, exactly. Not "Teacher Aid", not "TeacherAid MK".
- **Link:** `https://teacheraid.education`, always the full address. The name alone is read as
  "teacher aide" ([[ranking-plan]]).
- **Email:** `hello@teacheraid.education`.
- **Place:** Битола, Северна Македонија.
- **Handle:** try `teacheraid` first; if it is taken, `teacheraid.mk`, then `teacheraidmk`.
  Use the same one everywhere you can.
- **When done:** send Claude every profile link. They go into the site's structured data
  (`sameAs`), which tells Google the profiles and the site are one organisation.

## Pictures (`images/`)

| File | Where it goes | Note |
|---|---|---|
| `profile-1080x1080.png` | Profile picture on every network | The team's existing mark on blue |
| `linkedin-cover-1128x191@2x.png` | LinkedIn company page, cover | Words sit right; the logo covers the lower left |
| `facebook-cover-820x312@2x.png` | Facebook page, cover | Words are central, so phones do not cut them |
| `youtube-banner-2560x1440.png` | YouTube channel, banner | Everything important is in the middle 1235x338 that phones show |
| `youtube-thumbnail-1280x720.png` | The demo video's thumbnail | |
| `post-1-celiot-klas-1080x1350.png` | First post | Instagram, Facebook, LinkedIn |
| `post-2-prisustvo-1080x1350.png` | Second post | |
| `post-3-besplaten-chas-1080x1350.png` | Third post | |

All screens in the pictures are the real app with its demo data.

## Video (`video/`)

`teacheraid-demo-mk.mp4`: 60 seconds, 1920x1080, in Macedonian, made from the real teacher and
student screens. Seven steps: the lesson goes in, the questions are ready, students scan,
one click sends the question, each answers on their phone, answers arrive live, the list is
ready at the end; then "Првиот час е бесплатен". It has no voice or music; the words are on
screen. `teacheraid-demo-mk.srt` is the same text as subtitles, for YouTube.

How it was made: each frame was drawn from the app's own screens (teacher screens captured
with `scripts/teacher-capture.mjs` in the app repo, student screens from
`public/site-v2/screens`) and joined with ffmpeg. To change a word or make a vertical version
for Reels, ask Claude; it can rebuild it from the same screens.

**Upload to YouTube as:**

- **Title:** `TeacherAid: часот за 60 секунди`
- **Description:**

  > TeacherAid е апликација за часот за средни училишта и факултети во Македонија. Од
  > лекцијата на наставникот прави прашања, учениците одговараат на своите телефони,
  > присуството се запишува со скенирање на QR-код, а директорот секој месец добива
  > извештај. Направено во Битола.
  >
  > Првиот час во вашето училиште е бесплатен: https://teacheraid.education
  > Прашања и одговори: https://teacheraid.education/prasanja
  > hello@teacheraid.education

- **Language:** Macedonian. **Subtitles:** upload the `.srt`, language Macedonian.
- **Thumbnail:** `youtube-thumbnail-1280x720.png`. **Audience:** "No, it's not made for kids".
- Then send Claude the video link: it goes on the home page and the FAQ with VideoObject markup.

## The bio, in three lengths

**Short (up to 150 characters; Instagram, X, the YouTube tagline)**

> Апликација за часот, на македонски. Прашања од вашата лекција, присуство со QR-код. Направено во Битола.

**Medium (Facebook "Intro", LinkedIn tagline is the first sentence)**

> TeacherAid е апликација за часот за средните училишта и факултетите во Македонија. Од
> лекцијата прави прашања, учениците одговараат на своите телефони, а присуството се
> запишува со едно скенирање. Првиот час е бесплатен: teacheraid.education

**Long (LinkedIn "About", YouTube "Description", Facebook "About")**

> TeacherAid е апликација за часот за средни училишта и факултети во Македонија, направена во
> Битола.
>
> Наставникот ја додава лекцијата, како PDF, Word или неколку реченици, а TeacherAid подготвува
> пет до осум прашања со точните одговори и кратко објаснување. Ништо не стигнува до учениците
> додека наставникот не го прегледа.
>
> Во час наставникот ги испраќа прашањата од лаптопот, а учениците одговараат на своите
> телефони, без да симнуваат апликација. Присуството се запишува кога учениците ќе го скенираат
> QR-кодот на таблата. На крајот на часот наставникот го има списокот за е-Дневник и прашањата
> на кои класот најмногу грешеше. Директорот секој месец добива извештај, без имиња на ученици.
>
> Податоците се на училиштето и се чуваат во Европската унија.
>
> Првиот час е бесплатен: доаѓаме во вашето училиште и водиме вистински час со едно
> одделение. Пишете ни на hello@teacheraid.education.
>
> https://teacheraid.education

**English (LinkedIn allows a second language; use it there only)**

> TeacherAid is a classroom app for secondary schools and universities in North Macedonia, made
> in Bitola. It turns the teacher's lesson into questions, students answer on their phones,
> attendance is taken by scanning a QR code, and the director gets a report every month. The
> first lesson at your school is free: https://teacheraid.education

## Where to open, in order

1. **YouTube channel.** Banner, picture, long bio, links (site first). Upload the video.
2. **LinkedIn company page.** Industry: E-Learning Providers (or the closest LinkedIn offers).
   Size: 2-10. Location: Bitola. Website, cover, logo, long bio, English bio.
3. **Facebook page.** Category: Education (or Software). Cover, picture, medium bio, website,
   email. Instagram can be linked from the page's settings.
4. **Instagram.** Business or creator account, short bio, link to the site.
5. **Google Business Profile** (below).

Each founder: add "TeacherAid" as the current role on your own LinkedIn and link the company
page. This is about the team's own profiles, not the website, so the 2026-10-31 decision
(no team names on the site) still holds.

## The first three posts

Post them a few days apart, the same week the profiles open. Same text on Facebook, Instagram
and LinkedIn; on Instagram the link goes in the bio, not the post.

**Post 1, with `post-1-celiot-klas` or the video**

> Целиот клас одговара.
>
> TeacherAid од вашата лекција прави прашања за часот. Ги испраќате од лаптопот, учениците
> одговараат на своите телефони, а вие веднаш гледате кој разбрал.
>
> За средни училишта и факултети, на македонски. Направено во Битола.
> https://teacheraid.education

**Post 2, with `post-2-prisustvo`**

> Присуство со едно скенирање.
>
> На почетокот на часот таблата покажува QR-код. Ученикот го скенира и е запишан, без
> прозивка. Кодот се менува на секои 30 секунди, па слика од него испратена дома не работи.
> На крајот го имате списокот, подготвен за е-Дневник.
>
> Како работи: https://teacheraid.education/prisustvo-so-qr-kod

**Post 3, with `post-3-besplaten-chas`**

> Првиот час е бесплатен.
>
> Доаѓаме во вашето училиште и со едно одделение водиме вистински час, од скенирањето до
> списокот на крајот. Вие и наставниците гледате. Во учебната 2026/27 почнуваме со првите
> училишта.
>
> Пишете ни: hello@teacheraid.education

After that: one post a week is enough. The best ones will be from real lessons (what happened,
how many answered, a photo of the board or the teacher; never students without written consent
from their parents).

## Google Business Profile

TeacherAid goes to schools, so it is a **service-area business**: no street address on the map,
a service area instead. Google asks for in-person contact with customers at their place; the
free lesson in the school is that. Check eligibility during sign-up (verify).

- **Business name:** `TeacherAid` (nothing added; Google suspends names stuffed with keywords).
- **Category:** Software company (primary). Add Educational consultant only if Google offers it
  and it fits.
- **Service area:** Bitola, then the cities where the first schools are. Up to 20 areas.
- **Hide the address:** yes (service-area business).
- **Phone:** a team number you answer during school hours, or leave empty.
- **Website:** `https://teacheraid.education`
- **Description (750 characters max; this is 512):**

  > TeacherAid е апликација за часот за средни училишта и факултети во Македонија, направена во
  > Битола. Од лекцијата на наставникот прави прашања, учениците одговараат на своите телефони,
  > а присуството се запишува со скенирање на QR-код. На крајот на часот наставникот го има
  > списокот за е-Дневник, а директорот секој месец добива извештај, без имиња на ученици.
  > Податоците се на училиштето и се чуваат во Европската унија. Првиот час е бесплатен:
  > доаѓаме во вашето училиште и водиме вистински час со едно одделение.

- **Photos:** the profile picture as logo, the YouTube banner or `post-1` as cover, then the
  three posts. Later, photos from real lessons.
- **Verification:** Google decides how (video, phone or mail). Plan for a short video showing
  the team and something that proves the business is real.

## Related

[[ranking-plan]] · [[outreach-texts]] · [[geo-analysis-2026-11-25]] · [[messaging]] · [[name-and-mascot]]
