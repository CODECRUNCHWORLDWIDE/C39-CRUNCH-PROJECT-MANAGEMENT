# Week 8 — Exercises

Three guided exercises, 60–90 minutes each. **Run every query and every script yourself** — don't just read the lecture's version and assume it would have worked. This week's numbers are specific and checkable; if your result doesn't match the "Expected" note, that mismatch is the most useful thing that will happen to you all day.

1. **[Exercise 1 — Load an issue export](exercise-01-load-issue-export.md)** — load the full 112-row status-change history into PostgreSQL/SQLite and prove the foreign key actually enforces the relationship.
2. **[Exercise 2 — Query velocity and cycle time](exercise-02-query-velocity-and-cycle-time.md)** — velocity, throughput, WIP, and cycle time, written from scratch in SQL.
3. **[Exercise 3 — Cycle-time percentiles](exercise-03-cycle-time-percentiles.md)** — the same data in pandas: percentiles, segmented flow efficiency, aging WIP, and a written report.

## Before you start

- You've read all three lectures.
- `sprints` (7 rows) and `issues` (28 rows) are loaded from the [week README](../README.md).
- You have a SQL shell open (`psql atlas_pm` or `sqlite3 atlas_pm.db`) **and** a Python environment with `pandas` and `sqlalchemy` installed — you'll need both this week.

## Suggested workflow

- Do the exercises in order — Exercise 2 assumes `issue_status_history` exists (Exercise 1), and Exercise 3 assumes you're comfortable with the SQL definitions from Exercise 2 before translating them into pandas.
- For every "Expected" number, **check it before moving on.** A silently wrong query in Task 3 will produce a confidently wrong answer in Task 7 — this week's exercises build on each other more than most.
- Keep a scratch file open for the "write one sentence" prompts scattered through the exercises. They're not filler — being able to say *why* a number matters, in plain English, is the actual skill this week is teaching, not just the SQL syntax.

## Note on the two engines

Every SQL task works on both PostgreSQL and SQLite, with the differences (date arithmetic, `generate_series` vs. a recursive CTE, `FILTER` support) called out explicitly where they come up. If you're on SQLite, run `PRAGMA foreign_keys = ON;` at the start of every session — Exercise 1 depends on it.
