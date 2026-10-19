---
created: 2026-09-16
type: assessment
status: current
---

# Can v1 be built?

An honest read on [[feature-spec-v1]]: what can be built with AI assistance, what only a
human can do, and the one dependency that belongs to neither.

## Verdict

**Buildable. Not by the AI alone.** Roughly ninety per cent of the code can be written in
Claude Code sessions. The remaining ten per cent is where the risk lives, and none of it is
code.

Three boundaries, none of them about writing code:

### Next.js alone cannot reach the App Store or Play Store

Next.js is the teacher web app. The student app has to be React Native (Expo), because focus
lock needs operating-system APIs a web page cannot touch. Two codebases in one monorepo,
sharing types and the session client — not one. See [[technical-architecture]].

### iOS focus lock depends on Apple granting the Family Controls entitlement

You apply; it is reviewed over weeks; and a classroom app shielding a student's *personal*
phone is not the parental-control scenario Apple designed the entitlement for.
[[feature-spec-v1]] 6.1 makes the lock mandatory to join, so a refusal does not degrade the
iOS product — it eliminates it.

### Focus lock cannot be verified without a physical device

Both native modules can be written. Verifying them needs a real phone in a real hand, which
means the build loop on native work is: write, install, run, report, fix. Many short
exchanges rather than a few long ones. That is the main reason the focus-lock estimates in
[[milestones]] carry the widest range.

## Division of labour

| AI-side — about 90% of the code | Human-side only |
|---|---|
| Backend, schema, auth, realtime session engine | Register the legal entity, obtain D-U-N-S |
| Teacher web app, console, projected class view | Apple and Google developer enrolment |
| Expo student app, four question types, offline queue | The Family Controls entitlement request |
| Android and iOS focus-lock native modules | Running every build on a device and reporting |
| AI generation pipeline, e-учебник ingestion | Store submission and answering review |
| Reports, dashboards, admin, privacy mechanisms | A school that issues student mailboxes |
| Tests, CI, EU hosting configuration | ДПА and privacy review; МОН rights |

## Why Android first

Three decisions were taken that cannot all hold on iOS: focus lock **mandatory, native from
day one**; **no developer accounts or legal entity yet**; and a first milestone of **one real
lesson as soon as possible**.

The iOS chain is entity → D-U-N-S → Apple enrolment → entitlement review. Six to twelve
calendar weeks before iOS focus lock can even be *tested*, and it may still be refused.

Android has no such gate. An APK sideloads today. Usage access and the overlay work with no
Google approval at all — Play approval is required to *publish*, never to run a lesson.

**So: Android first.** Focus lock stays mandatory and genuinely real from day one, a lesson
runs in a classroom in weeks, iOS follows when Apple answers, and store listing becomes an
outcome later rather than a gate on the first lesson. This is not a retreat from the
mandatory-lock decision; it is the sequencing that makes it reachable.

**Consequence to accept:** during the Android-only phase, iPhone students in a test class
take the path [[feature-spec-v1]] 7.1 already defines for a student without a working phone —
present, no points, reason shown on the dashboard. For the first lesson, either pick a class
accordingly or accept that some students watch.

## Risks that move the estimate

| Risk | Effect if it lands |
|---|---|
| **Apple refuses the entitlement** | The iOS product as specified does not exist. Fallback is an Android-only pilot, or making the lock optional on iOS — which reopens [[feature-spec-v1]] 6.1 under pressure rather than by choice |
| **Android OEM background limits** | Xiaomi's MIUI, Samsung and Huawei kill background services aggressively, and those are precisely the phones Macedonian students carry. Could add 5–10 sessions alone |
| **No school mailboxes** | The identity design is redone. Cross-cutting: auth, roster and attendance all move. Assumption 0 in [[risks-and-assumptions]] |
| **Play rejects the accessibility-service route** | Fall back to usage access plus overlay and declare the use case carefully. Recoverable |
| **МОН refuses e-учебник rights** | The primary planning input is gone; generation falls back to three typed sentences. Does not block milestone one, which uses hand-authored lessons |

**The one to act on first:** test focus lock on a cheap Android early, not late. Discovering
the OEM problem in month three is expensive; discovering it in week two is a design decision.

## Related

[[build-v1]] · [[milestones]] · [[feature-spec-v1]] · [[technical-architecture]] ·
[[risks-and-assumptions]]
