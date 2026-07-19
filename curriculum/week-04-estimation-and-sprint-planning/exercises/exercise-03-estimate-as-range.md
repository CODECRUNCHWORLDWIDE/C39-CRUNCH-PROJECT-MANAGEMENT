# Exercise 3 — Convert a Point Estimate into a Confidence Range

**Goal:** Take a committed sprint scope and express its likely real-world duration as a stated range — grounded in Atlas's own history and the outside reference class — instead of a single number.

**Estimated time:** 45–60 minutes.

## Setup

You'll need Exercise 2's `commitment_points` figure and both correction factors from Lecture 2. Confirm the source data:

```sql
SELECT COUNT(*) FROM reference_class_projects;   -- must print 5
```

Create `exercise-03.py` (pandas) and `exercise-03-notes.md` for your written range statement.

## Part A — Pull both correction factors (15 min)

In pandas, connect to your database and compute both factors from Lecture 2, don't hardcode them:

```python
import pandas as pd
import sqlite3

conn = sqlite3.connect("c39wk4.db")

history = pd.read_sql("SELECT * FROM atlas_story_history", conn)
inside_factor = history["actual_days"].sum() / history["ideal_days_est"].sum()

ref = pd.read_sql("SELECT * FROM reference_class_projects", conn)
ref["overrun"] = ref["actual_weeks"] / ref["planned_weeks"]
outside_factor = ref["overrun"].mean()

print(f"Inside-view factor:  {inside_factor:.2f}")
print(f"Outside-view factor: {outside_factor:.2f}")
```

*Expected: inside ≈ `1.32`, outside ≈ `1.40`.*

## Part B — Look at the distribution, not just the mean (15 min)

A single mean flattens real variation. Print the full distribution of Atlas's own per-story overrun ratios and identify the worst case:

```python
history["overrun"] = history["actual_days"] / history["ideal_days_est"]
print(history[["title", "overrun"]].sort_values("overrun", ascending=False))
print(history["overrun"].describe())
```

In `exercise-03-notes.md`, name the single worst-overrun story and its ratio, and the best (a story that landed exactly on or under its ideal-day estimate, if one exists).

## Part C — Build the range for your committed sprint (20 min)

Using Exercise 2's `commitment_points` (≈21–22) as your baseline **ideal-day equivalent** (since 1 point was originally sized as ≈1 ideal day):

```python
commitment_points = 21.5   # replace with YOUR Exercise 2 result

low_estimate  = commitment_points                     # if the sprint runs close to the original plan
high_estimate = commitment_points * inside_factor      # Atlas's own measured realistic case
outside_check = commitment_points * outside_factor     # cross-check against the outside reference class

print(f"Optimistic (ideal):     {low_estimate:.1f} ideal-days of work")
print(f"Realistic (inside view): {high_estimate:.1f} ideal-days of work")
print(f"Outside-view cross-check: {outside_check:.1f} ideal-days of work")
```

Write the final range as a sentence a stakeholder could actually use, e.g.: *"Sprint 3's committed scope is sized at 21.5 ideal-days of work. Based on this team's own delivery history, expect it to realistically consume 28.4 focused engineer-days to actually finish — a 32% margin, consistent with a comparable-project cross-check of 40%. Plan the sprint's real capacity (28.4 focused days, from Exercise 2) around the realistic number, not the optimistic one."*

## Done when…

- [ ] `exercise-03.py` runs end to end and prints both correction factors, computed from the tables — not hardcoded numbers.
- [ ] `exercise-03-notes.md` names the worst- and best-overrun stories from Part B.
- [ ] You've written the final range as a plain-English sentence suitable for a stakeholder, not just raw Python output.
- [ ] Your range states **both** ends and the reasoning behind each — not a single "corrected" number.

## Stretch

- The `describe()` output includes a median. Recompute the range using the **median** overrun instead of the **mean** — does the range change meaningfully? Which would you present to Elena, and why? (Lecture 2, section 6 has a hint.)
- Add a third scenario using the single *worst* historical overrun ratio from Part B as a "worst realistic case" — how much wider does the range get, and would you ever actually state that number out loud to a stakeholder, or is it useful only for your own risk-awareness?

## Submission

Commit `exercise-03.py` and `exercise-03-notes.md` to your portfolio under `c39-week-04/exercise-03/`.
