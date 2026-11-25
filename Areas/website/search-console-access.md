---
created: 2026-11-25
type: area
status: ready — one person does the setup once, on their own computer (about 15 minutes)
---

# Letting Claude read Search Console

Why: today Claude can only guess what people search. With read access to Search Console it sees
the real searches that show our pages, how often, and at what position, and can rewrite pages
around them. This is the most useful next step for ranking ([[ranking-plan]]).

How: a Google **service account** (a robot user) gets read access to the Search Console property.
Its key is a file on one computer. **The key never goes into the repo, a chat, an email or a
cloud session.** That is why this runs in Claude Code on your own computer, not in the cloud.

## Setup, once

1. **Google Cloud project.** Go to console.cloud.google.com → Select a project → New Project,
   name it `teacheraid-seo`, then select it.
2. **Turn on the APIs.** APIs & Services → Library → enable **Google Search Console API**,
   **PageSpeed Insights API** and **Chrome UX Report API**. Do not use the Indexing API: Google
   allows it only for job postings and livestreams.
3. **The service account.** IAM & Admin → Service Accounts → Create service account, name it
   `claude-seo`, skip the optional steps. Open it → Keys → Add key → Create new key → JSON.
   A file downloads.
4. **Put the key in a private place.** In Terminal:

       mkdir -p ~/.config/claude-seo
       mv ~/Downloads/teacheraid-seo-*.json ~/.config/claude-seo/service_account.json
       chmod 600 ~/.config/claude-seo/service_account.json

5. **Give it read access in Search Console.** Open the file and copy the `client_email` value
   (it ends in `iam.gserviceaccount.com`). In Search Console → Settings → Users and permissions →
   Add user → paste it → permission **Full**. (Not Owner: it only needs to read.)
6. **Tell the SEO skill where things are.** Create `~/.config/claude-seo/google-api.json`:

       {
         "service_account_path": "~/.config/claude-seo/service_account.json",
         "default_property": "sc-domain:teacheraid.education"
       }

7. **Check it.** In the `teacher-aid-app` folder:

       bash scripts/seo-skills-setup.sh
       ".claude/skills/seo/scripts/claude-seo" run google_auth.py --check

   It should say Search Console API: OK.

If the key is ever shared by mistake: Google Cloud → the service account → Keys → delete that key,
then make a new one.

## Each month, or whenever you want real data

Open Claude Code in the `teacher-aid-app` folder on your computer and paste:

> Use /seo google with the property sc-domain:teacheraid.education. Pull the Search Console
> queries and pages since the last check-up in the brain (Areas/website/ranking-plan.md), inspect
> every URL in the sitemap, and check the sitemap status. Then tell me: which searches show us,
> where we rank, which pages Google has not indexed and why, and the three page changes that
> would gain the most. Record the numbers in a dated section of the ranking plan in the brain.
> Make the page changes on a branch and open a draft pull request; do not merge.

## Related

[[ranking-plan]] · [[seo-geo-plan]] · [[outreach-texts]]
