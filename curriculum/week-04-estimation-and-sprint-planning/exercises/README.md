# Week 4 — Exercises

Three guided exercises, ~1 hour each. Work them in order — Exercise 2 assumes you've internalized Exercise 1's sizing, and Exercise 3 assumes Exercise 2's capacity number.

1. **[Exercise 1 — Point a backlog](exercise-01-point-a-backlog.md)** — run planning poker (solo, playing all four voters) on the 10 Sprint 3 candidates in `sprint3_candidates`.
2. **[Exercise 2 — Compute a sprint's true capacity](exercise-02-compute-capacity.md)** — turn `team_capacity_calendar` into a real Sprint 3 capacity number, in points.
3. **[Exercise 3 — Estimate as a range](exercise-03-estimate-as-range.md)** — convert a point total into a stated confidence range using the correction factors from Lecture 2.

## Before you start

- You've completed all three lectures.
- You ran the Week 4 seed (see the [week README](../README.md)) and all four `SELECT COUNT(*)` sanity checks returned the expected numbers.
- You have a shell open: `psql c39wk4` (Postgres) or `sqlite3 c39wk4.db` (SQLite).
- Python with `pandas` installed, for the range calculation in Exercise 3.

## Suggested workflow

- Read each task before writing any SQL — sizing and capacity work rewards thinking before typing.
- Keep `atlas_story_history` open in a second query window while you point new stories in Exercise 1 — you cannot size relatively without the reference set in view.
- Save your work per exercise (`exercise-01.sql`, `exercise-02.sql`, `exercise-03.sql` / `.py`) — you'll reuse all three files directly in the mini-project.

## A note on "no single right answer"

Exercise 1 in particular has no answer key with one correct number per story — planning poker is a judgment call, same as real practice. What's checked instead is whether your reasoning is grounded (compared to a real reference story) and whether your final numbers are internally consistent with each other (a story you called "about the same size as a 5-point reference" should end up pointed near 5, not at 13).
