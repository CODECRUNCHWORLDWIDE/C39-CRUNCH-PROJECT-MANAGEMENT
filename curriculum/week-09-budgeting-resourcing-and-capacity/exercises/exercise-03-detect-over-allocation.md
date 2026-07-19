# Exercise 3 — Detect Over-Allocation with pandas

**Goal:** Reproduce Lecture 3's over-allocation finding yourself, end to end, in pandas — load the real portfolio data, normalize percent to hours, and write your own detection and reporting logic rather than reading the lecture's output.

**Estimated time:** 60 minutes.

## Setup

You need Lecture 3's full `people` (9 rows) and `allocations` (44 rows) data loaded, plus November's `time_entries` (7 rows) from Lecture 3 §6. Confirm the row counts:

```python
import pandas as pd
from sqlalchemy import create_engine

engine = create_engine("postgresql://localhost/atlas_pm")  # or sqlite:///atlas_pm.db
people = pd.read_sql("SELECT * FROM people", engine)
allocations = pd.read_sql("SELECT * FROM allocations", engine)
time_entries = pd.read_sql("SELECT * FROM time_entries", engine)

assert len(people) == 9
assert len(allocations) == 44
assert len(time_entries) == 7
print("setup ok")
```

Create a script `over_allocation.py` and write each task as a clearly separated, commented section.

## Tasks

1. **Merge and normalize.** Merge `allocations` with `people` and compute `planned_hours` per row exactly as Lecture 3 §5 does (4 weeks/month, `weekly_capacity_hours × allocated_pct / 100`). Print the first 5 rows.

2. **Total allocation per person-month.** Group by `name` and `month_start`, and compute `total_pct` (sum of `allocated_pct`) for each. Print every row where `total_pct` exceeds 100. *(Expected: exactly 2 rows — Elena Cruz in October, Sofia Reyes in November, matching Lecture 3 §3–5.)*

3. **A split-and-stretched filter.** A flat ">100%" threshold misses a real risk Lecture 3 §7 names explicitly: someone at 90–100%, but **split across two or more active projects**, carries a hidden context-switching cost a single-project person at the same percentage doesn't. Write a filter that flags every person-month where `total_pct >= 90` **and** the person has 2 or more distinct `project` rows that month (i.e., exclude anyone dedicated 100% to one single project — they're busy, but not fragmented). *(Expected: 8 rows — Elena Cruz and Sofia Reyes, every one of their 4 months each, since both are split every month in this dataset. The 5 single-project people at flat 100% should **not** appear, even though 100 ≥ 90 — that exclusion is the point of the filter.)*

4. **Per-project breakdown for a flagged person.** For **Sofia Reyes only**, produce a small table showing her `allocated_pct` by project, for every month she appears in the data (4 months × up to 2 projects each). This is the exact shape of table you'd screenshot into a status report or a 1:1 conversation with her manager.

5. **Plan vs. actual, generalized.** Lecture 3 §6 hand-built one comparison table for November. Write a **function** `plan_vs_actual(person_name, month)` that takes a name and a month string and returns a small DataFrame with `project`, `planned_hours`, `hours` (actual), and `variance_hours` for that person in that month — reusable for any person, any month, not hard-coded to Sofia and November. Call it for `("Elena Cruz", "2025-11-01")` and report the output.

## Expected result (spot checks)

- Task 2 → exactly 2 rows: Elena Cruz / 2025-10-01 / 110, Sofia Reyes / 2025-11-01 / 110.
- Task 3 → exactly 8 rows (Elena Cruz and Sofia Reyes, all 4 months each). None of the 5 single-project-at-100% people appear, even though their `total_pct` is technically ≥ 90 — the split-project condition is what excludes them, and confirming that exclusion is the actual point of the task.
- Task 5, called on Elena Cruz / November → two rows (Atlas and Ledger Redesign), matching the `variance_hours` values in Lecture 3 §6's table (-4.0 and +15.0).

## Done when…

- [ ] `over_allocation.py` runs top to bottom without errors against a freshly seeded `atlas_pm` database.
- [ ] Task 2's output matches Lecture 3 exactly — this is your correctness check before moving to Task 3's stricter filter.
- [ ] Task 3's filter correctly excludes the 5 single-project 100% people despite `total_pct >= 90` being true for them — if your first attempt includes them, you're missing the "2+ distinct projects" condition.
- [ ] Task 5's function has no person- or month-specific logic hard-coded inside it — prove this by calling it a second time with a different, undramatic person/month pair (e.g., `("Marcus Webb", "2025-11-01")`) and confirming it still runs and returns a sensible (small-variance) result.

## Stretch

- Extend Task 2 to flag over-allocation **predictively** — instead of just the months already in the data, write a one-paragraph note on what additional data you'd need to *forecast* December's over-allocation risk before December's allocations are even finalized (hint: look at each person's trend across Sep→Oct→Nov).
- Compute total portfolio-wide planned hours per month (`SUM` of `planned_hours` across every person and project) and compare it to total portfolio-wide *capacity* (`SUM` of `weekly_capacity_hours × 4` across all active people) for each month. Is the portfolio, in aggregate, over- or under-committed — and does that aggregate number hide or reveal the two individual over-allocations you found in Task 2?

## Submission

Commit `over_allocation.py` to your portfolio under `c39-week-09/exercise-03/`.
