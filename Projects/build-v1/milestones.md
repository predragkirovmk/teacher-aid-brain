---
created: 2026-09-16
type: plan
status: current
---

# Milestones and session budget

Two milestones. A session is one Claude Code context, not a calendar day — the two are related
only loosely, and the calendar items in [[build-v1]] run in parallel with all of it.

Estimates assume native work is many short turns: write, install on a device, report, fix.

## Milestone 1 — one real lesson, one real class

One teacher, one class, one period, on Android, with focus lock mandatory and real.

| Workstream | Sessions | Notes |
|---|---|---|
| Monorepo, Postgres schema, auth, environments, CI | 5–7 | Schema already specified in [[feature-spec-v1]] 11.1 and [[technical-architecture]] |
| Live session engine: realtime, state machine, four question types, scoring | 8–10 | Roughly 70% seeded by the existing prototype |
| Teacher console and class view, two synchronised windows | 5–7 | The projected/private split is the delicate part |
| Expo student app: auth, QR, answering, offline queue | 8–10 | Answer queueing during a dropout is the fiddly bit |
| **Android focus-lock module** | **8–14** | Widest variance. Usage access, overlay, foreground service, revocation detection, OEM behaviour |
| Absentee list, session end, integration, one rehearsal lesson | 7–11 | Real-device debugging dominates |
| **Total** | **41–59** | |

**The prototype is a real head start.** The demo repository already contains about 1,750 lines
implementing the phase state machine, scoring, QR generation, vote, reason, reveal, leaderboard
and polling — roughly seventy per cent of the largest subsystem here, already proven on stage.
See [[prototype]].

### What milestone 1 deliberately excludes

AI generation, e-учебници, dashboards, the admin role, the headmaster report, practice sets,
badges, seasons, the school library, iOS, both stores, billing. The first lesson runs on a
hand-authored lesson, because the prototype already proved scripted content works in front of
an audience.

### How milestone 1 is verified

Not by tests passing. By a real teacher running a real period with real students on real
Android phones, and by all six of these holding in that room:

1. Every student who authorized the lock joins without incident.
2. A student who did not authorize is present with no points, and is **never** marked absent.
3. An answer given during a network dropout is credited, with its original time.
4. The console shows "offline" distinctly from "has not answered".
5. The absentee list matches the empty chairs in the room.
6. The projected window never once shows the anonymous inbox.

If those hold, the foundation is sound and milestone 2 is mostly addition. Run this with a
friendly class, not the pilot school's opening day — if something fails it should fail in
front of a teacher who agreed to help.

## Milestone 2 — the pilot shape

A whole school on the free month, as [[startup-weekend-bitola-2026]] and the roadmap describe.

| Workstream | Sessions |
|---|---|
| AI generation plus e-учебник ingestion and chunking | 10–14 |
| Dashboards, admin role, roster lifecycle | 8–10 |
| Headmaster report: PDF, email, permanent private link | 4–5 |
| Practice sets, badges, leaderboards, seasons | 6–8 |
| Privacy mechanisms: audit log, staff access, data requests, retention | 4–5 |
| Macedonian interface, accessibility baseline | 4–6 |
| **iOS app and focus lock** — only once the entitlement lands | 12–18 |
| Onboarding: guided first lesson, setup lesson | 4–6 |
| Store submission on both platforms, including rejections | 6–12 |
| Pilot hardening from real classroom use | 10–20 |
| **Total** | **68–104** |

## Full build

**110–165 sessions.** Above a first estimate of 80–130, and the increase comes directly from
two choices: native from day one, and starting with no developer accounts. A web-first path
would have reached a real lesson in roughly 25–35 sessions, at the cost of shipping without
the focus lock — which was rejected deliberately. See [[feasibility]].

## Related

[[build-v1]] · [[feasibility]] · [[feature-spec-v1]] · [[prototype]]
