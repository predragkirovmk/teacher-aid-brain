# Teacher Aid — Second Brain

This is an Obsidian vault organized with the PARA method. An AI agent (or you,
months from now) should be able to open this folder cold and know where anything
goes.

## Structure

    Home.md      Dashboard — start here
    Projects/    Active work with a defined goal and a deadline. One folder per
                 project. When a project ends, move its folder to Archive/.
    Areas/       Ongoing responsibilities with no end date (finances, health,
                 a role you maintain). If it never "finishes," it's an area.
    Resources/   Reference material and topics of interest, no deadline attached.
    Archive/     Anything inactive, moved out of the three folders above. Don't
                 delete — archive. The history has value.
    Daily/       One note per day, named YYYY-MM-DD.md. Free-form log, open
                 loops, quick capture. Doesn't need to be tidy.
    Templates/   Note templates for daily/project/area/resource/meeting notes.
                 Used by Obsidian's core Templates plugin (Ctrl/Cmd+P → "Insert
                 template").

## Conventions

1. **One topic per note.** If a note is trying to cover two things, split it.
2. **Link, don't duplicate.** Use `[[wikilinks]]` to connect notes instead of
   copying content between them.
3. **Date anything time-sensitive** in the frontmatter or filename
   (`YYYY-MM-DD`). An undated fact is hard to trust later.
4. **New project → new folder under `Projects/`**, seeded from
   `Templates/project-note.md`.
5. **Keep `Home.md` short.** It's a map into the vault, not a place to write.

## Where this differs from a generic PARA vault

- **What this is.** The business brain for TeacherAid — an AI classroom system for
  Macedonian secondary schools (гимназија). Born at Startup Weekend Bitola,
  11–13 September 2026. Read `Home.md`, then `Areas/product/product-overview.md`
  and `Areas/business-model/business-model-canvas.md` before anything else.
- **Working name is "TeacherAid"** (one word in prose) until the team decides
  otherwise — alternatives and the decision checklist are in
  `Areas/brand/name-and-mascot.md`. Do not rename across the vault without that
  decision.
- **Language: English.** Macedonian terms stay in Cyrillic where they are the real
  name of a thing (МОН, БРО, e-Дневник, гимназија, ЗЗЛП). No emojis anywhere.
- **Areas are grouped:** `Areas/business-model/`, `Areas/product/`, `Areas/market/`,
  `Areas/brand/` — one subfolder per domain, one topic per note. New notes go into
  the matching subfolder; a new domain gets a new subfolder.
- **Pitch material lives in the project**, not in Areas:
  `Projects/startup-weekend-bitola-2026/` holds the script, slide outline, demo
  plan, Q&A prep, validation log and rehearsal notes. When the weekend is over,
  move the folder to `Archive/` and lift anything still true into Areas.
- **Numbers have sources.** Every figure that could land on a slide is either cited
  (State Statistical Office 2024/25, a dated press quote, a price table) or marked
  `(verify)`. Keep it that way — a judge, a headmaster or a ministry will ask.
- **Assumptions are tracked**, not buried: `Areas/business-model/risks-and-assumptions.md`
  is the register; every conversation that proves or disproves one updates it and
  `Projects/.../validation-log.md`.
- **The prototype is a separate repo** (`~/Desktop/teacher-aid-app`); this vault
  only documents it (`Areas/product/prototype.md`). Code never lives here.
- **Daily notes** are the running log of decisions; the day's decisions get copied
  into the relevant Area note the same day, with the date.

## Git

This vault is a git repo. Commit changes in small, focused commits — treat git
as the undo button for the vault's memory, same as any other project.
