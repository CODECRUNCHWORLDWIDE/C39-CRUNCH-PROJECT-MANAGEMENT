# Exercise 3 — Cycle-Time Percentiles in pandas

**Goal:** Reproduce Lecture 3's analysis yourself, end to end, from a cold connection to a finished report — percentiles, segmented flow efficiency, and aging WIP — and practice deciding what to compute in SQL versus what to compute in pandas.

**Estimated time:** 90 minutes.

## Setup

```bash
pip install pandas sqlalchemy psycopg2-binary   # or: pip install pandas sqlalchemy   (SQLite needs no driver)
```

Create a script `flow_analysis.py`. You'll build it up task by task; run it after every task to check your output against the "Expected" note.

## Task 1 — Connect and load (10 min)

Connect to `atlas_pm` and load `issues` and `issue_status_history` into DataFrames, with dates parsed correctly.

*Expected:* `issues.shape == (28, 11)`, `history.shape == (112, 5)`, and `issues.dtypes["resolved_at"]` reads `datetime64[ns]`.

## Task 2 — Decide what to push into SQL (10 min)

Before writing a line of pandas logic, answer in a comment at the top of your script: which of this week's numbers (velocity, throughput, WIP, cycle time, flow efficiency) would you rather compute with a `SELECT ... GROUP BY` **before** the data ever reaches pandas, versus after loading everything raw? There's no single right answer, but you must have one and be able to defend it. *(Hint: anything that's a clean single-table aggregate with no row-level judgment call is a good SQL candidate; anything you'll want to slice five different ways interactively is a good pandas candidate.)*

## Task 3 — Cycle time for every completed issue (15 min)

Reproduce Lecture 3, Section 2: compute `cycle_time_days` for every issue where `current_status == 'Done'`.

*Expected:* 23 rows in your `done` DataFrame. Longest cycle time: `ATLAS-115` at 14 days.

## Task 4 — The percentile report (15 min)

Compute p50, p85, p95, and max cycle time. Print them as a small, labeled table (not raw numbers with no context — this is going in `report.md`).

*Expected:* p50 = 8, p85 = 10, p95 = 11, max = 14.

Then answer in a comment: if Priya Chen asks "how long should I tell a customer to expect," which one of these four numbers do you hand her, and why not the mean?

## Task 5 — Segmented flow efficiency (20 min)

Reproduce Lecture 3, Section 4 exactly: compute per-issue flow efficiency, then the naive (unweighted) average, then the two-segment breakdown (`was_blocked` vs. `never_blocked`).

*Expected:* naive average ≈ `0.895`; never-blocked group = `18` issues at `100.0%`; was-blocked group = `5` issues averaging `~51.5%`.

Write one sentence: which of those three numbers would you actually put in a status report to Priya, and why is it not the naive average?

## Task 6 — Aging WIP as of the snapshot (15 min)

Reproduce Lecture 3, Section 5 for `SNAPSHOT = pd.Timestamp("2026-04-06")`. Produce a table with `issue_key`, `current_status`, `age_in_current_status_days`, `total_age_days`, sorted oldest-in-current-status first.

*Expected:* 3 rows (the current WIP). `ATLAS-123` has the highest `age_in_current_status_days`, closely followed by `ATLAS-126`.

## Task 7 — Write it up (5 min)

In `report.md`, write four to six sentences a non-technical sponsor could read: what's the typical cycle time, what's the p85 commitment number, what does the flow-efficiency split reveal, and which open issue deserves attention today and why.

## Done when…

- [ ] `flow_analysis.py` runs top to bottom with no errors against a freshly loaded `atlas_pm`.
- [ ] All six "Expected" checkpoints match.
- [ ] `report.md` exists and is readable by someone who has never seen a DataFrame.
- [ ] Your Task 2 comment states and defends a SQL-vs-pandas split.

## Stretch

- Compute the same p50/p85/p95 percentiles **separately for `story` vs. `bug`** issue types. Do bugs and stories have meaningfully different tail behavior, or does the small bug sample (4 issues) make the comparison unreliable? Say which, and why.
- Recompute flow efficiency using **lead time** (`resolved_at - created_at`) as the denominator instead of cycle time. How much does the story change, and why does adding backlog wait time into the denominator make blocked-time *look* less significant even though nothing about the blocking changed?

## Submission

Commit `flow_analysis.py` and `report.md` to your portfolio under `c39-week-08/exercise-03/`.
