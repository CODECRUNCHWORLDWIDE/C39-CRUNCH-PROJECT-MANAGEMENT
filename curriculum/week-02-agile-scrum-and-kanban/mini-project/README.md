# Mini-Project — One Backlog, Two Boards: Scrum vs. Kanban on Real Numbers

> Run Project Atlas's actual backlog two ways — as a time-boxed Scrum sprint, and as a continuous, WIP-limited Kanban pipeline — and produce a data-backed recommendation for which fits the team, instead of a values-based opinion.

**Estimated time:** 2.5–3 hours, best done Saturday after the exercises and challenges.

This is the week's capstone, and it's deliberately built to stop you from answering "Scrum or Kanban?" with a gut feeling. You have one real backlog (`atlas_backlog`, extended with this week's WIP-flood rows from Exercise 2). You'll simulate running it to completion as a Scrum sprint, then simulate running the *same* work as a Kanban pipeline, compute the metrics each framework actually cares about, and write a memo Priya could read and trust — because it's backed by queries, not vibes.

---

## Deliverable

A directory in your portfolio `c39-week-02/mini-project/` containing:

1. `setup.sql` — the `UPDATE`/`INSERT` statements you run to simulate sprint 2 reaching its end (Part 1 below).
2. `scrum-run.sql` — every query you use to compute Part 1's Scrum metrics.
3. `kanban-run.sql` — every query you use to compute Part 2's Kanban metrics.
4. `comparison-memo.md` — the write-up: both frameworks' results side by side, and your recommendation (Part 3 below).
5. `notes.md` — a short reflection (see the end).

Everything runs against `atlas_backlog` from the [week README](../README.md), extended with Exercise 2's flood rows (21–26). Works on PostgreSQL or SQLite; note which you used.

---

## Part 1 — Run it as Scrum (a time-boxed sprint)

Sprint 2 is a **10-day, two-week box**, running 2026-01-19 through 2026-02-06. At the start of the sprint the team committed to the ten `sprint_id = 2` items from the seed table (items 10–15, 19 — plus 16, 17, 18, 20 which stayed in the backlog, **not** pulled into the sprint).

**Simulate the sprint reaching its end.** Run this against your database (this is realistic: not everything committed gets finished — that's normal, not a failure, and part of what you're measuring):

```sql
UPDATE atlas_backlog SET status = 'done', done_date = '2026-02-03' WHERE item_id = 10;
UPDATE atlas_backlog SET status = 'done', done_date = '2026-02-04' WHERE item_id = 11;
UPDATE atlas_backlog SET status = 'done', done_date = '2026-02-02' WHERE item_id = 12;
UPDATE atlas_backlog SET status = 'done', done_date = '2026-02-05' WHERE item_id = 13;
UPDATE atlas_backlog SET status = 'done', done_date = '2026-02-06' WHERE item_id = 14;
-- item 15 (Workspace activity feed, 5 pts) and item 19 (the invite-email bug fix, 1 pt)
-- do NOT finish in the sprint — leave their status as-is; they carry over to sprint 3.
```

**Compute and report:**

1. **Committed points** — the sum of `story_points` for everything pulled into sprint 2 at the start (items 10–15, 19).
2. **Completed points (velocity)** — the sum of `story_points` for everything that reached `done` with a `done_date` inside the sprint window.
3. **Completion rate** — completed ÷ committed, as a percentage.
4. **What carried over, and why it matters** — list the item(s) still not done at sprint end. In one sentence each, state what happens to them next (Lecture 2, §2: they go back into planning for the next sprint, not silently forgotten).
5. **A one-paragraph sprint summary**, written the way you'd actually post it for Priya and Elena at sprint review — plain language, the real number, no spin.

---

## Part 2 — Run the *same* backlog as Kanban (continuous flow)

Now set the sprint boundary aside entirely. Treat every item that's ever been in `in_progress`, `in_review`, or `done` (across the whole seed table, including Exercise 2's flood items 21–26) as flowing through one continuous pipeline with **no fixed two-week box** — items ship whenever they're ready, and the pipeline runs the WIP limit you set on `in_review` in Exercise 2.

**Compute and report:**

1. **Throughput per week** — using Lecture 3 §5d's query pattern, count items completed (`done_date`) grouped by week, across every `done` item in the table (sprint 1's nine items plus whatever finished in Part 1's simulated sprint 2).
2. **Lead time distribution** — using Lecture 3 §5c's query, report the minimum, maximum, and average lead time (`done_date - created_date`) across every `done` item.
3. **Current WIP against your Exercise 2 limit** — query current WIP per active column (Lecture 3 §5a) including the flood rows, and state whether the pipeline is currently **over** the WIP limit you set in Exercise 2, and by how much.
4. **The expedite question.** Item 19 (the invite-email bug — a production bug, not a planned feature) sat in `in_progress` starting 2026-01-28. In a real Kanban pipeline with WIP limits, a production bug like this is often given an **expedite lane** — a narrow exception that's allowed to bypass the WIP limit because the cost of leaving it unfixed outweighs the cost of the limit violation. In 3–4 sentences: would item 19 qualify for an expedite lane under a reasonable policy, and what should that policy explicitly say to keep expediting rare (a team that expedites everything has no WIP limit at all)?

---

## Part 3 — The comparison memo

In `comparison-memo.md`, write a memo (400–600 words) structured as:

1. **Side-by-side numbers** — a small table putting Part 1's Scrum metrics next to Part 2's Kanban metrics (they're not directly comparable one-for-one, and saying *why* they're not — velocity is sprint-scoped, throughput isn't — is itself part of a good answer).
2. **What each framework's numbers are actually good at telling you** — velocity/completion-rate answers "did we deliver what we promised this cycle," while throughput/lead-time/WIP answer "how fast does work move through the pipeline, regardless of any particular cycle." Say which question Priya and Elena more often need answered for Atlas's kind of work, and why (tie back to Lecture 3 §6's fit table).
3. **Your recommendation for Atlas**, and — this is the nuance a strong memo includes — whether that recommendation should be **pure** (all Scrum, or all Kanban) or whether item 19's expedite-lane question from Part 2 suggests a **hybrid** element is worth adding to Atlas's otherwise-Scrum process (recall Lecture 3 §6's closing note on Scrumban). Defend your call with numbers from Parts 1 and 2, not just framework preference.

---

## Rubric

| Criterion | Weight | "Great" looks like |
|-----------|------:|--------------------|
| Correctness of Scrum metrics | 20% | Committed/completed points and completion rate are computed correctly from the actual data |
| Correctness of Kanban metrics | 20% | Throughput, lead time, and WIP-vs-limit are computed correctly and reference Exercise 2's limit specifically |
| Comparison reasoning | 25% | The memo explains *why* velocity and throughput answer different questions, not just that they're different numbers |
| Recommendation quality | 20% | The recommendation is defended with this project's actual numbers, addresses the expedite-lane nuance, and doesn't default to "Scrum because that's what Atlas already does" without re-deriving why |
| Clarity for a non-technical reader | 15% | The sprint summary and memo are readable by Priya — plain language, no unexplained jargon |

---

## Reflection (`notes.md`, ~200 words)

1. Which number surprised you more — the sprint's completion rate, or the current WIP overage against your Exercise 2 limit?
2. If Atlas ran pure Kanban instead of Scrum, what would Priya and Elena actually lose — be specific, not just "less structure."
3. Where did treating "the same backlog" two different ways feel like it distorted one of the frameworks — i.e., where did this exercise's setup not quite match how a team would really choose one framework from day one rather than retrofitting both onto existing data?
4. One thing you'd change about `atlas_backlog`'s schema to make next sprint's version of this exercise easier (foreshadows a fuller event-history table).

---

## Stretch goals

- **Full cumulative flow diagram.** Add a `status_log` table that records every status transition with a timestamp (rather than only the current status), backfill it plausibly from `atlas_backlog`'s existing dates, and use Lecture 3 §5e's pandas pattern to plot a real day-by-day cumulative flow diagram for sprint 2 — a stacked area chart of WIP per column, over time.
- **Cycle time, properly.** With the `status_log` table from the stretch above, compute *true* cycle time (time from first entering `in_progress` to `done`) for every finished item, and compare it to the simpler lead time (`done_date - created_date`) this mini-project used — report how far apart the two numbers are and explain why.
- **A second team, for real contrast.** Build a second seed table modeling Northlight's Support/Platform team (Lecture 3 §3) — ticket arrivals with no `sprint_id` at all, just continuous pull — and show that Kanban's metrics make sense for that table in a way Scrum's velocity never could (there's no sprint to measure it against).

---

## Why this matters

Most "Scrum vs. Kanban" debates on real teams are fought with opinions and whichever framework someone read about most recently. This mini-project is the shape of the actual, defensible version of that decision: same underlying work, two different lenses, real numbers under each lens, and a recommendation grounded in what those numbers actually show. That habit — reach for the data before reaching for a framework preference — is the difference between a PM who facilitates ceremonies and a PM who runs delivery.

When done: push, then take the [quiz](../quiz.md) and move on to Week 3 — backlogs, user stories, and estimation.
