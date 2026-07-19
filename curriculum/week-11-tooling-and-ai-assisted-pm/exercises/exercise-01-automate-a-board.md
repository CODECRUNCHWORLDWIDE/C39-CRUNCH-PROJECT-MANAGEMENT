# Exercise 1 — Automate a GitHub Projects Board

**Goal:** Configure a real GitHub Projects board's fields and automations from scratch, then script its export into SQL — the exact repair job Marcus owes Atlas's old, half-configured board.

**Estimated time:** 90 minutes.

## Setup

1. Create a **free public repository** (e.g., `<your-username>/atlas-practice`) if you don't already have one to attach a project to. A handful of throwaway issues is fine.
2. From your GitHub profile or org, create a new **Project** (Projects v2) — **+ New project → Table**.
3. Confirm `gh` is installed and authenticated: `gh auth status`. If not, `gh auth login`.
4. Confirm you can query the seed: `SELECT COUNT(*) FROM board_items;` returns **16**.

## Part A — Configure the board (do this in the real GitHub UI)

1. **Add four custom fields** matching this week's seed schema:
   - `Priority` — single select: `P0`, `P1`, `P2`, `P3` (give each a distinct color).
   - `Size` — single select: `XS`, `S`, `M`, `L`, `XL`.
   - `Iteration` — the native Iteration field type, 2-week duration, start date = any Monday.
   - Extend the built-in `Status` field with two extra options so it reads `Todo`, `In Progress`, `In Review`, `Blocked`, `Done`.
2. **Add 5–8 issues** from your practice repo to the project (create a few throwaway issues first if you need to), and set realistic values on every custom field for each.
3. **Create two saved views**:
   - `Board` layout, grouped by `Status`.
   - `Table` layout, filtered to `status:Blocked` — your "what's stuck" view.
4. **Turn on two built-in automations** (Settings → Workflows): item closed → Status: Done, and auto-archive items Done for 14+ days.

Take a screenshot or note the field/view names — you'll need them in the writeup.

## Part B — Script the export

5. Write a script (Python, using `gh api graphql` piped in, or the `requests` library against GitHub's GraphQL endpoint directly) that pulls every item on your project — content title/number, and every custom field value — and writes it to `my_board_export.json`. Use Lecture 1 Section 7 as your starting template; adapt the field list to your project's field names.

6. Load `my_board_export.json` into a table `my_board_items` in your database with pandas, following the flatten-then-`to_sql` pattern from the lecture.

7. Run and record the output of:

   ```sql
   SELECT status, COUNT(*) FROM my_board_items GROUP BY status ORDER BY COUNT(*) DESC;
   ```

## Part C — Query the seed (no live tool required for this part)

8. Against this week's `board_items` seed, write and run queries for:
   - How many items are in each `iteration`? *(Expected: 4 in Sprint 7, 12 in Sprint 8.)*
   - Which items are `Blocked`, and who's assigned to each? *(Expected: 2 rows — issue #44 Wei Zhang, issue #52 Chris Okoye.)*
   - For each `priority`, the count of open items (`status <> 'Done'`). *(Expected: P0 has 1 open — #52 is Blocked; PR #210 is Done so it's excluded. P1 has 3 open, P2 has 4 open, P3 has 0 open — every P3 item already shipped.)*
   - Which items have no `assignee`? *(Expected: 1 row — the draft issue, item 16.)*

## Done when…

- [ ] Your real GitHub Projects board has 4 custom fields, 2 saved views, and 2 automations turned on.
- [ ] `my_board_items` exists in your database, populated from a script you wrote (not hand-typed).
- [ ] You can explain, from your script, exactly which GraphQL field maps to which SQL column.
- [ ] Your four Part C queries run against the seed and match (or you can explain any mismatch against) the expected counts.

## Stretch

- Add a **third** automation not covered in the lecture: research `auto-add` with a label filter, and wire it so any new issue labeled `atlas-team` in your practice repo joins the project automatically. Open a new issue with that label and confirm it appears without manual action.
- Extend your export script to also pull the linked pull requests' review status, and add a `review_status` column.

## Submission

Commit `export_board.py`, `my_board_export.json` (redact anything private), and `part-c-answers.sql` to your portfolio under `c39-week-11/exercise-01/`, plus a `board-config.md` with the field/view/automation names from Part A.
