# Week 11 — Homework

Five problems, ~4 hours total, spread across the week. These reinforce the lectures with a mix of hands-on tool work, SQL, and a written analysis. Commit each.

All queries run against `board_items` and `jira_issues` from the [README](./README.md) unless a problem says otherwise.

---

## Problem 1 — Board field audit (45 min)

Marcus's old board had one field nobody filled in consistently. Prevent that from happening to yours.

1. For each of `board_items`' four custom fields (`status`, `priority`, `size`, `iteration`), write a query that finds rows where the field is `NULL` or otherwise looks unset.
2. *(Expected: `size` is the only field with a `NULL` — one row, the draft issue. Every other field is fully populated across all 16 rows.)*
3. **Deliver** `field-audit.sql` with the four queries and their row counts, plus two sentences: if you were auditing a *real* team's board and found a field 40% empty, what would you do — drop the field, or chase down the missing values? Argue for one, given what Lecture 1 said about fields that answer a real question versus decoration.

---

## Problem 2 — Fifteen warm-up queries (75 min)

Write and run each against `board_items` and/or `jira_issues` as specified. Put them in `warmups.sql` with a `-- N` comment and the row count beneath each.

1. All `board_items` with `priority = 'P0'`.
2. All `board_items` that are Pull Requests (`item_type = 'PullRequest'`).
3. `board_items` with no `assignee`.
4. The 3 most recently updated `board_items`, by `updated_at` descending.
5. `board_items` grouped by `size`, counted — which size is most common?
6. `jira_issues` where `issue_type = 'Bug'`.
7. `jira_issues` with `story_points IS NULL` — what do all of these have in common? *(Hint: check `issue_type`.)*
8. `jira_issues` assigned to Yuki Tanaka, sorted by `created` ascending.
9. `jira_issues` in `'PLAT Sprint 14'` only.
10. Every `board_items` row with the label `automation` in its `labels` column (`LIKE '%automation%'`).
11. `board_items` where `status = 'In Review'`.
12. `jira_issues` where `reporter <> assignee` — work reported by someone other than who's doing it.
13. `board_items` closed (`closed_at IS NOT NULL`) in `'Sprint 8'`.
14. `jira_issues` with `priority = 'Low'`.
15. `board_items` where `title LIKE '%webhook%'` (case-sensitive) — compare to a case-insensitive version and note the difference, same trap as Exercise 2's Task 6.

---

## Problem 3 — Explain the schema mismatch (45 min)

In `schema-writeup.md`, answer in prose (no more than 400 words total):

1. List every column in `jira_issues` that has **no** direct equivalent column in `board_items`, and vice versa.
2. For one column on each side, explain in your own words *why* the two tools model that concept differently — not just "they're different," but what each tool's design assumption is.
3. If Northlight decided to build **one unified table** to report on both tools' data side by side (instead of migrating one into the other, which is what Challenge 1 does), what would that table's schema look like? Sketch a `CREATE TABLE` for it.
4. Which columns would be easy to unify, and which would force you to throw away information? Name one of each.

---

## Problem 4 — JQL fluency drill (45 min)

Five more JQL-to-SQL pairs, same discipline as Exercise 2. Write both (JQL if you have live Jira; SQL always) in `jql-drill.md`:

1. Issues in `PLAT Sprint 14` that are `Done`.
2. Issues where `priority = Medium` and `assignee != reporter`.
3. Issues created in the second half of the seed's date range (after 2026-04-10).
4. `Story`-type issues only, sorted by `story_points` descending, `NULL`s last.
5. Issues where the `summary` does **not** contain "dashboard".

**Deliver** the 5 pairs plus a one-sentence note on which of the 5 was hardest to translate from JQL into SQL, and why.

---

## Problem 5 — A grounded prompt, from scratch (60 min)

Pick **any** SQL query against this week's data that you haven't already used in a lecture or exercise example — something you write yourself. Build the full three-stage grounded pattern around it:

1. The SQL query (Stage 1).
2. The serialized JSON block it produces (Stage 2).
3. A written prompt (Stage 3) that would turn that JSON into one paragraph for a stakeholder, following Lecture 3's constraints (only stated facts, name items individually where relevant, say "not available" for anything ungrounded).

You don't have to actually run it against an AI for this problem — the exercise is designing the pipeline correctly, which you can check yourself by re-reading your own prompt and asking whether it could produce a fabricated claim given only your JSON.

**Deliver** `grounded-prompt-from-scratch.md` with all three stages and one sentence on what question your chosen query answers that none of this week's other examples did.

---

## Time budget

| Problem | Time |
|--------:|----:|
| 1 | 45 min |
| 2 | 75 min |
| 3 | 45 min |
| 4 | 45 min |
| 5 | 60 min |
| **Total** | **~4.5 h** |

After homework, take the [quiz](./quiz.md) and ship the [mini-project](./mini-project/README.md).
