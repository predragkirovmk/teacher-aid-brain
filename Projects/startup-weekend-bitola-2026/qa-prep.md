---
created: 2026-09-12
type: prep
status: v3 — pricing flat EUR 199/month per school (13 Sep); answers 2, 8, 9 and 17 rewritten
  against feature-spec-v1 (15 Sep)
---

# Q&A prep — five minutes of judges

Format: repeat the question in five words, answer in three sentences, stop. "We don't know
yet, here's how we'd find out" is a full-credit answer. Hand technical questions to the
teammate who owns them, by name.

Ordered by likelihood.

## 1. "Why wouldn't a teacher just use ChatGPT and Kahoot for free?"

They do — that's five tabs and a paper attendance list, and none of them talk to the
headmaster. TeacherAid is the whole lesson in one flow: plan, opener, pop-ups, attendance,
focus, anonymous questions, and a report the school actually pays for. The school pays,
not the teacher — free tools never get a headmaster's signature.

## 2. "How do you block apps on a student's personal phone? That's not possible."

It is, with the student's one-time permission. On iOS it's Apple's Screen Time API —
FamilyControls and ManagedSettings — the same mechanism focus apps like Opal use; on
Android it's usage-access plus an overlay, like AppBlock. Authorizing it is how you join the
lesson: a student who doesn't is marked present and sits the game out, the same as a student
with a dead battery. Nobody is marked absent over a phone setting. *(Backup slide b.)*

> **Updated 15 Sep.** The old answer was "a student can revoke it and the teacher just sees
> a focus flag — it's social and technical, not a prison, and that's why parents accept it."
> That is no longer true: authorization is now required to join ([[feature-spec-v1]] 6.1).
> The harder sentence has to be said honestly, and the fallback — present, no points, never
> absent — is the part that makes it acceptable to a parent. Do not give the old answer.

## 3. "Phones are being banned in schools across Europe. What about Macedonia?"

As of May this year the Minister said the teacher decides — she used exactly those words —
and that a ban in secondary schools would only be discussed. Any ban will carve out
teacher-sanctioned educational use, which is what we are. That's also why we start in
secondary schools, not primary.

## 4. "Who actually pays in a public school? Headmasters have no budget."

Correct — the municipality does, which is why we start with private gymnasiums where the
headmaster owns the budget and decides in a week. For public schools the municipality is
one office for seven schools in Bitola; a EUR 2,388/year pilot — one flat fee for a whole
school — fits under small-value procurement. We'll confirm the exact threshold with the
municipality next week.

## 5. "The market is tiny."

It is — about seven thousand secondary teachers, EUR 420k a year if we had every one. It's
the right first market: one curriculum, one language, one ministry, and we can meet every
headmaster in person. Every neighbor has the same shape of system — Serbia alone is ten
times the seats — and the product changes by curriculum corpus, not by design.

## 6. "What did you validate this weekend?"

We talked to [N] teachers. [Quote 1.] [Quote 2.] What we changed because of it: [e.g. the
anonymous question feature / the upload-your-plan input]. We have a warm contact with a
headmaster and a call booked for [day]. *(Fill from [[validation-log]] — this answer
decides a third of the score.)*

## 7. "Curipod does this already. Why won't they crush you?"

Curipod is the closest and it's a good product — a slide tool sold to teachers, in English,
with no attendance, no focus lock and no headmaster report. If they localized tomorrow
they'd still be selling to the wrong person. Our moat is the school relationships, the
daily teacher habit, and the school-vs-school leaderboard that gets better with every
school we add.

## 8. "Students are minors. What about the data?"

The school is the data controller — it already processes attendance — and we're the
processor under a data-processing agreement, exactly like any school software. We store what
the lesson produces: name, class, attendance, points, and which topics each student keeps
getting wrong. Nothing from the rest of the phone; EU hosting; deleted when the student
leaves. The school leaderboard uses nicknames. Nobody at TeacherAid can open a school's data
without a logged, time-limited grant the school can see.

## 9. "Points count toward grades? Is that fair? Is it allowed?"

The app never grades — the teacher decides if and how the numbers inform a participation
grade, the same discretion they have today with a raised hand, and nothing changes in МОН's
grading rules. What the app does do is show the teacher which topics a student keeps getting
wrong, which is information a good teacher already tries to hold in their head. The school
owns that data and it's covered in the processing agreement.

> **Updated 15 Sep.** The old answer stopped at "the app never grades" and did not mention
> that per-topic correctness is now stored per student ([[feature-spec-v1]] 8.4). Volunteer
> that second half rather than being caught by it — a judge or a headmaster who discovers it
> after the flat denial will hear the whole answer as evasion. See [[data-and-privacy]].

## 10. "What does the AI cost you? What are your margins?"

About five cents per generated lesson on today's prices — roughly EUR 20 per teacher per
year at heavy use. Because we price flat per school — EUR 199 a month, whole staff
included — margin depends on school size: around 80% at a small school, closer to 40% at
an average one, thinner at a very large one. We're watching that curve in the pilot, and a
size tier is the fix if a big school runs too close to the line.

## 11. "What if a teacher with 25 years' experience refuses?"

They're still covered — it's a flat fee for the whole staff, not a per-seat charge, so
nobody has to opt in for the school to be onboarded. In practice we don't need every
teacher on day one: the champion teachers use it immediately, the veteran comes around
once the class next door is visibly quieter and more engaged than theirs. The fee doesn't
wait for that to happen.

## 12. "Does it work when the school wifi is down?"

Students use their own mobile data — the messages are tiny, and a phone that drops out
queues the answer and syncs it when it's back, so nobody loses a question to the network.
The teacher's plan is on their device; if everything dies the lesson runs out loud and
points pause. We'll test it in a concrete-walled Bitola school in the free live session.

## 13. "Why not price per teacher instead of a flat fee? Doesn't that cap you at big schools?"

It does — flat pricing is deliberately cheap for a big staff, which caps how much a large
school ever pays us. We made that trade on purpose: one number a headmaster approves in a
meeting, with nothing to count and no "how many of our teachers actually use it"
negotiation. It also means the leaderboard and the habit-forming happen across the whole
school from day one, not just two volunteers' classes. If a school's usage costs us more
than the flat fee earns, that's a real number we're tracking — the fix is a size band
above a certain teacher count, not a redesign of the model.

## 14. "Why you? Who's on the team?"

[Names and roles — one line each.] What we don't have yet is a teacher on the team; we're
recruiting one this month from the teachers we interviewed and the pedagogy faculty at
UKLO.

## 15. "What about Albanian-language schools?"

Version one is Macedonian. The AI side is a language setting; the curriculum is the same
БРО program. Albanian is on the roadmap once the corpus for Macedonian is proven.

## 16. "How is this different from the Ministry's own platform, EDUINO?"

EDUINO is a content library — videos and resources. We're the live 45 minutes in the room.
We'd rather integrate than compete: their content, our lesson engine.

## 17. "Isn't a QR photo from home enough to be marked present?"

No — the QR rotates every 30 seconds, so a photograph is stale before it reaches anyone.
Signing in with the school email means a student can't play as someone else either. Also:
attendance in TeacherAid is for the dashboard — official attendance stays in e-Дневник,
where the teacher still sees the empty chair.

## 18. "How much are you raising / what do you need?"

We're not raising this weekend. What we need is one private gymnasium to run the free
month in October and an advising teacher. If anyone in this room knows a headmaster, that's
the introduction we'd take.

## 19. "What's the name? Teacher Aid sounds like first aid."

Fair. It's the working name; we've shortlisted alternatives — our favourite is Zvono, "the
bell", which works in every language in the region. We'll decide after we've decided the
harder things.

## Rules

- If two judges ask at once, answer the harder one.
- Numbers you say must match the deck: 199 euros a month, 2,388 a year, 128, seven
  thousand, 67,000, five cents.
- If a judge is wrong about a fact, agree with the concern, then give the fact.
- Never say "great question".

## Related

[[feature-spec-v1]] · [[pitch-script]] · [[competitors]] · [[data-and-privacy]] ·
[[technical-architecture]] · [[revenue-and-pricing]] · [[macedonian-education-system]] ·
[[validation-log]]
