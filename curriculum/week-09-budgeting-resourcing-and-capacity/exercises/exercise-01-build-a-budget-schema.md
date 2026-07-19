# Exercise 1 — Build a Budget Schema for a Second Project

**Goal:** Prove the `budget_lines`/`actuals` schema generalizes beyond Atlas by standing up a real budget for **Ledger Redesign** — the second Northlight initiative Elena Cruz and Sofia Reyes are already partially allocated to in Lecture 3 — inside the *same two tables*, distinguished only by the `project` column.

**Estimated time:** 60 minutes.

## Setup

You already have `budget_lines` and `actuals` populated with Atlas's 13 budget lines and 11 actuals from Lecture 2. Confirm:

```sql
SELECT project, COUNT(*) FROM budget_lines GROUP BY project;
-- Atlas | 13
```

## The brief

Ledger Redesign is a smaller, shorter initiative: an 8-week internal project to rebuild Northlight's billing/ledger module, running **October and November 2025** only (2 months, not 4). It has no dedicated Tech Lead — Elena Cruz product-owns it part-time (you already know her allocation from Lecture 3 §2), and it borrows two Atlas engineers part-time rather than having its own dedicated team. Northlight gave it a **$62,000** total budget.

## Tasks

1. **Design the budget.** Using the same four categories (`labor`, `tooling`, `vendor`, `contingency`), split Ledger Redesign's $62,000 across October and November. There's no single right split — a reasonable one is roughly 85% labor, 5% tooling, 0% vendor (no third-party dependency on this one), 10% contingency. State your split and your reasoning in `budget-design-notes.md` before writing SQL — this is the same discipline Lecture 1 §4 modeled for Atlas.

2. **Write the `INSERT`.** Insert your budget lines into the live `budget_lines` table with `project = 'Ledger Redesign'`, continuing the `budget_line_id` sequence from wherever Atlas's rows left off (i.e., starting at 14, not 1 — primary keys can't collide across projects in a shared table).

3. **Verify isolation.** Run this and confirm it returns exactly your new rows, none of Atlas's:

   ```sql
   SELECT * FROM budget_lines WHERE project = 'Ledger Redesign' ORDER BY budget_line_id;
   ```

4. **Add three realistic actuals.** Invent three plausible `actuals` rows against your new budget lines (e.g., an October timesheet total, a November timesheet total, a small tooling subscription) with real dollar amounts and one-sentence descriptions each. Insert them.

5. **Run one cross-project query.** Write a single query that returns **both** projects' total planned budget (`BAC`) side by side, in one result set, using `GROUP BY project`:

   ```sql
   SELECT project, SUM(planned_amount) AS bac
   FROM budget_lines
   GROUP BY project
   ORDER BY project;
   ```

   *Expected:* two rows — `Atlas | 280000` and `Ledger Redesign | 62000`.

## Expected result (spot checks)

- `SELECT SUM(planned_amount) FROM budget_lines WHERE project = 'Ledger Redesign';` → `62000` exactly. Your category split can vary; the total must not.
- The cross-project query in Task 5 returns exactly 2 rows.
- No `budget_line_id` collision — every ID in the table is unique across both projects.

## Done when…

- [ ] `budget-design-notes.md` states your category split and reasoning for Ledger Redesign, written *before* the SQL (matching Lecture 1's discipline).
- [ ] `ledger-budget.sql` contains your `INSERT` statements for budget lines and actuals, each under a `-- Task N` comment.
- [ ] The Task 5 cross-project query is included and its output pasted as a comment beneath it.
- [ ] You can explain, in one sentence, why adding a `project` column to a shared table is preferable to creating a second, near-identical `ledger_budget_lines` table.

## Stretch

- Add a fourth budget line for Ledger Redesign in the `vendor` category anyway, for a small third-party invoicing library, and explain in one sentence why "no vendor cost" from the brief was actually optimistic.
- Write a query that returns total planned budget **per category, per project**, using `GROUP BY project, category` — a genuine portfolio-wide budget summary in one result set.

## Submission

Commit `budget-design-notes.md` and `ledger-budget.sql` to your portfolio under `c39-week-09/exercise-01/`.
