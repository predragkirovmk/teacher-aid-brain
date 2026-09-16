---
created: 2026-09-16
status: active — not started building
deadline: none set; first milestone is "as soon as possible"
---

# Build v1

## Goal

Get one real teacher running one real lesson with one real class, on Android, with focus lock
mandatory and working. Then grow that into the whole-school pilot shape.

The product is specified in [[feature-spec-v1]]. This project is about building it.

## Where things stand (16 September 2026)

- Specification complete and integrated across the vault.
- Feasibility assessed — see [[feasibility]]. Verdict: buildable, 110–165 sessions total.
- Milestones defined — see [[milestones]].
- **Nothing built yet.** No monorepo, no schema, no accounts.
- The demo prototype from Startup Weekend still runs and is reusable — see [[prototype]].

## Start now, in parallel — these run on calendar time, not build time

Nothing in the build shortens these, and the first two gate everything on iOS.

- [ ] **Register the legal entity and request a D-U-N-S number.** Longest pole in the whole
      project. Weeks, not days.
- [ ] **Apple Developer enrolment** the moment the entity exists. EUR 99/year.
- [ ] **Google Play Console.** EUR 25 once. Needed only to publish, not to test.
- [ ] **The Family Controls entitlement request**, the day the Apple account is live. Write it
      as a classroom-consent case, not a parental-control one. **Most time-critical action in
      the project** — and it can be refused, see [[feasibility]].
- [ ] **Confirm a school issues student mailboxes.** The entire login design fails without it.
      Assumption 0 in [[risks-and-assumptions]], the highest-damage row in that table.
- [ ] **Get hold of a cheap Android** — a two-year-old Xiaomi or budget Samsung. Focus lock
      will work on a clean Pixel and fail there, and there is what students carry.
- [ ] **Line up a friendly class** for the first lesson. Not the pilot school's opening day.

## Next actions in the build

1. Foundations: monorepo, Postgres schema, auth for teacher and student, environments, CI.
2. Port the prototype's session engine to real realtime, roles and four question types.
3. Everything else in milestone order — see [[milestones]].

## Open decisions that belong to the team, not the build

- **The proposed point values and timer defaults** in [[feature-spec-v1]] 13.1 and 4.4 are
  labelled unsigned-off. They stay proposals until someone decides.
- **The beachhead contradiction.** Pilot subjects are history, electronics and physics, which
  point at a стручно or technical school; every business note assumes a private гимназија.
  One of the two is out of date.
- **Pricing** is being reconsidered. [[revenue-and-pricing]] holds the current number.

## Related

[[feasibility]] · [[milestones]] · [[feature-spec-v1]] · [[risks-and-assumptions]] ·
[[technical-architecture]] · [[prototype]]
