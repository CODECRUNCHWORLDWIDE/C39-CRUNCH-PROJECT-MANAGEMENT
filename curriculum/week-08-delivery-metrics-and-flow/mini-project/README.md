# Mini-Project — A Flow Dashboard for Atlas, With a Written Read

> Compute a complete flow dashboard from Atlas's real issue export — velocity, throughput, cycle-time percentiles, WIP, and a burndown — entirely in SQL and pandas, then write the read Priya Chen actually asked for: is delivery slowing down, and why.

**Estimated time:** 3 hours, best done Saturday after the exercises and challenges.

This is the week's capstone, and it's deliberately shaped like a real assignment rather than a set of textbook questions: a sponsor asked a plain-English question, you have the raw data, and your job is to turn it into a small number of trustworthy metrics plus a written interpretation a non-technical reader can act on. Nobody will hand you the SQL — you're producing it, checking it against itself, and then explaining what it means in words a VP of Product will actually read.

---

## Deliverable

A directory in your portfolio `c39-week-08/mini-project/` containing:

1. `dashboard.sql` — every query behind your dashboard (you may reuse and refine your Challenge 1 views, or write fresh queries — either is fine, but they must be complete and runnable top to bottom against a clean `atlas_pm`).
2. `flow_analysis.py` — the pandas half: cycle-time percentiles, segmented flow efficiency, and aging WIP, loading data via `pd.read_sql` against the same database.
3. `dashboard.md` — the numbers themselves, laid out as a short set of labeled tables (not raw query dumps — a reader shouldn't have to guess what a column means).
4. `report.md` — 300–500 words, written for Priya Chen: is delivery slowing down, by how much, why, and what (if anything) should change before Sprint 8 planning.
5. `notes.md` — a short reflection (see the end).

Everything runs against the seed data from the [week README](../README.md). Works on PostgreSQL or SQLite; note which you used.

---

## What the dashboard must cover

Structure `dashboard.md` around these five sections. Each one names the specific numbers it must contain — you decide the exact query shape and formatting.

### 1. Velocity

- Velocity for every sprint, Sprint 1 through Sprint 7, clearly marking which sprints are closed and which (Sprint 7) is still in progress.
- The average velocity across the six **closed** sprints, and how the two most recent closed sprints compare to that average.

### 2. Throughput

- Weekly throughput (issues resolved per calendar week) for the full data range.
- A 4-week rolling average, and the single highest and lowest non-zero weeks.

### 3. Cycle time

- p50, p85, p95, and max cycle time across all completed issues.
- The same broken out by `issue_type` (`story` vs. `bug` vs. `task`) — note where a group is too small to trust (a 1- or 4-issue sample isn't a distribution, it's an anecdote; say so if it applies).

### 4. WIP and flow efficiency

- Current WIP (as of the 2026-04-06 snapshot), with each open issue's age in its current status.
- Flow efficiency, computed **both** as a single naive average **and** segmented by "was ever blocked" vs. "never blocked" — and one sentence explaining why the two numbers tell different stories.

### 5. Burndown

- A day-by-day burndown table for Sprint 7 (committed points, cumulative done, remaining), from the sprint's start through the snapshot date.
- One sentence comparing the actual remaining-points trajectory to what a straight-line ideal burndown would predict at the same point in the sprint.

---

## Milestones

Pace yourself; don't try to build all five sections in one sitting.

- **Milestone 1 (45 min):** Sections 1–2 (velocity, throughput) — all SQL, `GROUP BY` and one window function.
- **Milestone 2 (45 min):** Section 3 (cycle time) — the join pattern from Lecture 2, then percentiles in pandas.
- **Milestone 3 (45 min):** Section 4 (WIP, flow efficiency) — reuse Lecture 3's segmentation logic; don't report the naive number alone.
- **Milestone 4 (45 min):** Section 5 (burndown) — the window-function running-total query from Lecture 2.
- **Milestone 5 (30–45 min):** `report.md` and `notes.md` — write these last, once every number in `dashboard.md` is checked and correct.

---

## Rules

- **Every number in `report.md` must trace back to a number in `dashboard.md`.** No new claims invented in the prose that the dashboard doesn't support.
- **State the open-sprint caveat wherever it applies.** Sprint 7's velocity, throughput's most recent week, and the burndown are all partial — say so explicitly, don't present them with the same confidence as closed-sprint numbers.
- **Segment before you average.** Section 4's flow-efficiency requirement is not optional decoration — reporting only the naive average is an incomplete answer this week, specifically.
- **No spreadsheet touches this project.** Every number is produced by a `.sql` file or a `.py` script that could be re-run tomorrow against a refreshed export and produce a new, correct answer automatically.

---

## Rubric

| Criterion | Weight | "Great" looks like |
|-----------|------:|--------------------|
| Correctness | 35% | Every number in `dashboard.md` is right and matches the underlying queries |
| Completeness | 15% | All five sections present, nothing skipped or stubbed |
| Segmentation discipline | 15% | Flow efficiency reported both ways; open-sprint numbers clearly caveated |
| Written interpretation | 20% | `report.md` answers the actual question (is delivery slowing, why) in language a non-technical sponsor trusts |
| Reproducibility | 15% | `dashboard.sql` and `flow_analysis.py` run cleanly, top to bottom, against a fresh `atlas_pm` |

---

## Reflection (`notes.md`, ~200 words)

1. Which of the five sections took the longest to get right, and why?
2. Where did the naive-vs-segmented distinction (flow efficiency, or anywhere else) change what you would have reported if you'd stopped at the first number you computed?
3. If you had one more week of Atlas data (an eighth sprint), what's the single query you'd run first to see whether the situation improved or got worse?
4. Which part of this project would you have gotten wrong, or not thought to check, without this week's lectures?

---

## Why this matters

This is the shape of real delivery-metrics work: a sponsor's plain-English question, a real (messy, partially-open, occasionally inconvenient) dataset, and a small set of numbers you have to trust enough to put your name on. Do this once, correctly, with the caveats it deserves, and you have a dashboard you could hand to any PM lead on any team and say "here's how you'd build this for your own project" — which is exactly the foundation Week 9 (budgeting and resourcing) builds on next.

When done: push, then take the [quiz](../quiz.md) and start [Week 9 — Budgeting, resourcing & capacity](../../week-09-budgeting-resourcing-and-capacity/).
