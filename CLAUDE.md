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

(Add project-specific rules here as they come up — a note-naming quirk, a
recurring workflow, anything you'd otherwise have to re-explain every time.)

## Git

This vault is a git repo. Commit changes in small, focused commits — treat git
as the undo button for the vault's memory, same as any other project.
