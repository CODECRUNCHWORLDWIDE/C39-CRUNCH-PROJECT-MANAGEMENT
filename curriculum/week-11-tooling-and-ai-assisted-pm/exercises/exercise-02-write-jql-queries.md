# Exercise 2 — Write JQL Queries

**Goal:** Get JQL into your fingers, then prove you understand it by reproducing every query in SQL against `jira_issues`. Two languages, one skill.

**Estimated time:** 90 minutes.

## Setup

- If you have a Jira Cloud sandbox: create a project with key `PLAT`, issue types Story/Bug/Task, and manually enter (or CSV-import) this week's 14-row seed so you can run real JQL against it. This is optional but strongly recommended once this course — typing JQL into Jira's actual search bar and watching autocomplete work is a different experience than reading about it.
- If you're skipping the live Jira setup this week, work entirely in Part B against the SQL seed — the SQL is graded the same either way.
- Confirm the seed: `SELECT COUNT(*) FROM jira_issues;` returns **14**.

Create a file `jql-and-sql.md`. For every task, write the JQL (if you have live Jira) and the SQL side by side.

## Part A — JQL (live Jira, if available)

1. **Everything in PLAT.** `project = PLAT`
2. **Open Bugs only.** Bugs that aren't Done, in PLAT.
3. **My team's Highest-priority work.** Highest priority, not yet Done.
4. **Sprint 15 board.** Everything in `PLAT Sprint 15`, sorted by priority descending then created ascending.
5. **Reported by Priya.** Everything where the reporter is Priya Chen — the cross-team asks Sofia's team is fielding.
6. **Text search.** Anything with "webhook" in the summary.
7. **Compound.** High or Highest priority, status not Done, assigned to Omar Haddad or Yuki Tanaka.

## Part B — The same 7 queries, in SQL against `jira_issues`

Write each as a `SELECT`. Compare your row counts to the "Expected" hints — if they don't match, you likely mis-translated a JQL operator (check Lecture 2's operator table).

1. **Everything in PLAT.** *(Expected: 14 rows.)*
2. **Open Bugs only.** *(Expected: 1 row — PLAT-509 is the only Bug not yet Done.)*
3. **Highest priority, not Done.** *(Expected: 1 row — PLAT-505.)*
4. **Sprint 15, sorted.** *(Expected: 10 rows; write the `CASE`-based priority sort from Lecture 2, don't rely on alphabetical order.)*
5. **Reported by Priya Chen.** *(Expected: 3 rows.)*
6. **Summary contains "webhook".** *(Expected: 7 rows case-insensitively — but only 3 with a plain, case-sensitive `LIKE '%webhook%'` (PLAT-504, PLAT-507, PLAT-513 are the only ones spelled with a lowercase "webhook"). Run both and see the gap; see Week 1 if you need a refresher on `LIKE` vs `ILIKE`.)*
7. **Compound: (High or Highest) AND not Done AND (Omar or Yuki).** *(Expected: 2 rows — PLAT-507 and PLAT-509.)*

## Part C — One JQL feature SQL doesn't have for free

8. Jira's saved filter behind PLAT's board is:

   ```
   project = PLAT AND status != Done ORDER BY priority DESC, Rank ASC
   ```

   `Rank` is Jira's manually-maintained drag order — there's no `rank` column in this week's seed, because it isn't derived from any of the fields we exported. Write the SQL you *can* produce (`project_key = 'PLAT' AND status <> 'Done'`, sorted by the `CASE`-based priority rank from Lecture 2), and then write one sentence explaining exactly what's lost by dropping the `Rank ASC` tiebreaker — what two rows in your result would a human PM expect to see in a different order than your query gives them?

## Expected result (spot checks)

- Part B, Task 1 → 14 rows.
- Part B, Task 3 → 1 row (PLAT-505, "SLA dashboard data feed contract").
- Part B, Task 5 → 3 rows (PLAT-503, PLAT-505, PLAT-514).
- Part B, Task 7 → 2 rows (PLAT-507, PLAT-509).

## Done when…

- [ ] `jql-and-sql.md` has all 7 Part A/B pairs (JQL blank is fine if you skipped live Jira, but say so explicitly).
- [ ] Every Part B query's row count matches its expected hint, or you've written a one-line explanation of the discrepancy.
- [ ] Part C's SQL runs, and your one-sentence answer names two specific rows.
- [ ] You can explain, without looking it up, why `ORDER BY priority DESC` behaves differently in JQL than in raw SQL.

## Stretch

- Write the JQL **and** SQL for "issues that have been open (not Done) for more than 10 days as of today." *(Hint: JQL has no direct "days since created" comparator without a function; SQL's is `CURRENT_DATE - created > 10`. Note which language made this easier and why.)*
- Export your Part B queries' results with pandas (`pd.read_sql(query, engine)`) and produce one `.groupby("status").size()` summary — the pandas equivalent of `GROUP BY status, COUNT(*)`.

## Submission

Commit `jql-and-sql.md` and `stretch.sql` (or `.py`) to your portfolio under `c39-week-11/exercise-02/`.
