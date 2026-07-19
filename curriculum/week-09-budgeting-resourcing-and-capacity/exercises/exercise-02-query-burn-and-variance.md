# Exercise 2 — Query Burn and Variance

**Goal:** Get comfortable writing your own burn, variance, and trend queries against Atlas's real budget — the exact question shape ("are we over, by how much, since when") a stakeholder will ask you cold, with no lecture open beside you.

**Estimated time:** 60 minutes.

## Setup

You need Lecture 2's full `budget_lines` and `actuals` data loaded (13 budget lines, 11 actuals, all `project = 'Atlas'`). Confirm:

```sql
SELECT COUNT(*) FROM budget_lines WHERE project = 'Atlas';  -- 13
SELECT COUNT(*) FROM actuals;                                -- 11
```

Create a file `solutions.sql` and put each answer under a `-- Task N` comment.

## Tasks

1. **Total burn, no breakdown.** Write a query that returns Atlas's single total actual-cost-to-date number, excluding contingency (per Lecture 2 §5). *(Expected: 205400.)*

2. **Burn by month, one category.** Write a query returning `tooling`'s actual spend for each month it has actuals, ordered chronologically. *(Expected: 3 rows — 2000, 2000, 2100.)*

3. **Variance by category, generalized.** Adapt Lecture 2 §3's variance query so the "as of" date is a value you can change in one place (a `WITH` CTE holding the date, referenced everywhere else) rather than hard-coded three times. Re-run it with `DATE '2025-10-31'` instead of `2025-11-30` and report the new variance numbers for all three non-contingency categories. *(Expected: with only 2 months of actuals in the window — Sep and Oct — labor variance should be `(48000+58000) - (50200+61500) = -5700`.)*

4. **Worst single-line overage.** Not category-level — find the single **`actuals` row** (not the category) with the largest dollar amount, and report its category, description, and amount. *(Hint: `ORDER BY amount DESC LIMIT 1`.)*

5. **Contingency remaining, as a percentage.** Write a query that returns contingency remaining as a **percentage of the original $30,000 reserve**, not just the dollar figure. *(Expected: `25700 / 30000 = 85.7%` remaining.)*

6. **Your own "is this getting worse" check.** Using Lecture 2 §4's running-total pattern as a model, write a query that computes the running cumulative actual for the **`vendor`** category (not `labor`) by month, and state in a one-line comment whether vendor's overrun looks like a one-time event or a trend, based on the shape of the numbers. *(Expected: the jump is concentrated entirely in November — say why that's a different pattern than labor's steady month-over-month creep.)*

## Expected result (spot checks)

- Task 1 → `205400`.
- Task 2 → `2000, 2000, 2100` across three months.
- Task 3 → labor variance at end of October = `-5700`.
- Task 5 → `85.7` (or `85.67`, depending on your rounding).

## Done when…

- [ ] `solutions.sql` has all 6 queries, each under a `-- Task N` comment, with the actual query output pasted as a trailing comment.
- [ ] Task 3's CTE genuinely makes the "as of" date a single edit point — prove it by re-running with a third date (`2025-09-30`) and reporting what changes.
- [ ] Task 6's one-line comment correctly distinguishes a one-time spike from a trend, using the shape of the running total as evidence, not a guess.
- [ ] You can explain out loud why Task 1 excludes contingency but a query for "contingency remaining" (Task 5) obviously must include it.

## Stretch

- Write a query that returns, for every category, the **single month with the worst dollar variance** (not the cumulative worst) — this requires ranking months within each category, a preview of window functions beyond the running-sum pattern from Lecture 2 §4.
- Compute PV, EV, AC, CPI, and SPI as of `DATE '2025-10-31'` instead of the lecture's `2025-11-30`, using **150 of 240 points delivered** as the October scope figure. Compare your October CPI/SPI to the lecture's November numbers — is Atlas's cost-efficiency improving or worsening month over month?

## Submission

Commit `solutions.sql` to your portfolio under `c39-week-09/exercise-02/`.
