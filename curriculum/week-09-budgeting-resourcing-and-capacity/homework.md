# Week 9 — Homework

Five problems, ~4 hours total, spread across the week. These reinforce the lectures with a mix of applied SQL/Python work, structured analysis, and one interview-style reflection. Commit each.

Problems reference Project Atlas and Northlight Software (see [Lecture 1](./lecture-notes/01-budgeting-a-tech-project.md) for the cast) unless a problem says otherwise.

---

## Problem 1 — Interview someone about a real budget or resourcing crunch (60 min)

Budgeting and resourcing judgment is learned partly by watching real ones unfold. Pick **one**:

- **(A)** Find someone in your network who has managed or worked close to a project budget (a PM, engineering manager, freelancer managing their own time/rate, or team lead — formal title not required) and ask: "Tell me about a time a project ran over budget — how early did you know, and what would have told you sooner?" and "Describe a time you or a teammate were double-booked across two things at once — how did it actually get resolved?" Write up their answers plus one thing that surprised you, in `interview.md`.
- **(B)** If you can't find someone to interview, find a **public postmortem or retrospective** that discusses cost overrun or resourcing failure (search "[company] project over budget postmortem" or "engineering team burnout retrospective") and analyze it through this week's lens instead: was there a burn signal that should have been visible earlier, using Lecture 2's variance-over-time thinking? Was there an over-allocation pattern (one person or team quietly double-booked) that a portfolio-wide allocations query, like Lecture 3's, would have caught? Write this analysis in `postmortem-analysis.md` (250–400 words), citing the source.

---

## Problem 2 — Build a tiny real budget for something you actually do (45 min)

In `my-budget.sql`, using this week's `budget_lines` schema, build a small, real budget for something you're actually part of or planning — a class project, a freelance gig, a personal project with a real time cost, a club or community initiative. At minimum: 2 categories, a stated contingency percentage with one sentence justifying the size, and a total. This is the same discipline as Exercise 1, applied to something with no answer key — the check is whether you could defend every line to someone who knows the project as well as you do.

---

## Problem 3 — Trace one real over-allocation from your own experience (45 min)

Outside of this week's challenges, think of (or reconstruct from memory) a time **you personally** were allocated to two or more things at once in a way that didn't actually fit — two classes with overlapping deadlines, two jobs/gigs, a class project and a part-time job, family + work + school. In `my-over-allocation.md` (200–300 words), describe it using this week's vocabulary: what would your `allocated_pct` across each commitment have summed to, roughly, if someone had modeled it honestly in advance? What actually gave — did quality suffer somewhere, did you lose sleep, did one commitment quietly get shortchanged (Lecture 3 §6's "starves the lower-visibility project" pattern)? What would a resourcing plan built *before* the conflict, instead of discovered during it, have changed?

---

## Problem 4 — Earned value in your own words (30 min)

In `evm-reflection.md` (200–300 words), answer: PV, EV, and AC are three different numbers that all sound like "how much have we spent or planned." Explain, to someone who has never heard of earned value management, why you need all three — specifically, describe a scenario (invented or real) where a project is *exactly on budget* (`AC = PV`) but is actually in serious trouble, and a second scenario where a project is *over budget* (`AC > PV`) but is actually in good shape. What does each scenario reveal that a single "how much have we spent" number would hide?

---

## Problem 5 — Compute a tiny burn/variance/forecast by hand (60 min)

Not every budget needs a database. In `tiny-budget-forecast.md`, invent (or use a real) small budget for something concrete — planning a trip, running a small event, a short freelance engagement — with a total budget, at least 3 months (or weeks, if the timeline is shorter) of planned spend, and at least 2 months of actual spend that diverges from plan in a realistic way. Compute plan-to-date, actual-to-date, variance, and a simple EAC using the `EAC = BAC / CPI`-style method (you can approximate CPI as `PV / AC` if you don't have a clean "% scope delivered" figure for something this small — state that you're approximating and why). State, in one sentence, what you'd do differently based on your own forecast.

---

## Time budget

| Problem | Time |
|--------:|-----:|
| 1 | 60 min |
| 2 | 45 min |
| 3 | 45 min |
| 4 | 30 min |
| 5 | 60 min |
| **Total** | **~4 h** |

After homework, take the [quiz](./quiz.md) and ship the [mini-project](./mini-project/README.md) if you haven't already.
