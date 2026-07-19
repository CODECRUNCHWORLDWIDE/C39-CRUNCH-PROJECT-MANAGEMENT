# Exercise 1 — Charter and Plan Your Capstone Project

**Skill:** Applying Lecture 1 in full — scoping, chartering, slicing, and standing up the board — on your own real project.

**Estimated time:** 1.5 hours. Do this Monday, before touching any code.

---

## Setup

You need:

- Your `capstone_pm` database created and the schema from the week's `README.md` run against it.
- A GitHub (or Jira) account with a new empty project ready to configure.
- A repo (new or existing) for your capstone's actual code.

---

## Part A — Scope it (15 min)

Write, in plain sentences, in a new file `SCOPING.md`:

1. What's the full-size version of your idea — the version you're *not* building this week?
2. What's the smallest real slice of it that has a genuine, checkable "done" by Saturday? Apply Lecture 1, §2's rule of thumb explicitly: can you picture the literal final `git push`?
3. List at least 2 things you're deliberately cutting to get from (1) to (2).

If you can't answer (3) with at least two real cuts, your scope in (2) is probably still too big — go back and cut further.

## Part B — Write the charter (35 min)

In `charter.md`, following Lecture 1, §4 (which is Week 1 Lecture 3's anatomy):

1. Objective, including your scoping decision from Part A stated explicitly.
2. At least 3 success criteria, each passing the "would two reasonable people agree" test.
3. A first-pass definition of done.
4. Scope in (≥5 items) and scope out (≥3 items, matching your Part A cuts).
5. At least 3 real constraints.
6. Decision authority — all four hats named, even if one person holds them all.

## Part C — Slice the backlog (25 min)

Produce a table of **8–15 backlog items**, each with a `key`, `title`, `item_type`, and MoSCoW `priority` (Week 3). At least 4 must be `must`-priority — if everything is `must`, you haven't actually prioritized (Week 4's estimation lecture makes the same point about inflated commitment).

Insert every item into `capstone_items`:

```sql
INSERT INTO capstone_items
    (item_id, item_key, title, item_type, priority, created_at, current_status)
VALUES
    (1, 'YOUR-01', 'Your first backlog item title', 'feature', 'must', CURRENT_DATE, 'Backlog');
    -- one row per item
```

Verify:

```sql
SELECT priority, COUNT(*) FROM capstone_items GROUP BY priority;
```

You should see at least 4 `must` rows and at least 1 `could` (a real, nameable "first thing to cut if the week runs short").

## Part D — Stand up the board (15 min)

1. Create the GitHub Projects (or Jira) board with columns matching your `current_status` values exactly: Backlog, Ready, In Progress, Blocked, In Review, Done.
2. Add every backlog item as a card in the matching column (all should start in `Backlog`).
3. Set the WIP limit on **In Progress** to 1.
4. Move your first `must` item to `Ready`.

## Part E — Delivery approach (10 min)

In `delivery-approach.md`, state and defend your approach (Lecture 1, §7) — almost certainly Agile/Kanban flow for a project this size, but justify it with your project's specific facts, not the textbook default.

---

## Expected outcome

By the end of this exercise you have, in your capstone repo:

- `SCOPING.md`, `charter.md`, `delivery-approach.md`
- A `capstone_items` table with 8–15 rows, correctly prioritized
- A live board with matching columns and your first item in `Ready`

Sanity-check query — this should return your full backlog with nothing missing a priority or type:

```sql
SELECT item_key, title, item_type, priority, current_status
FROM capstone_items
ORDER BY item_id;
```

## Common mistakes

- **Scoping to "an MVP" without actually cutting anything concrete.** If Part A's cut list is vague ("keep it simple"), it isn't a real cut — name the specific feature you're not building.
- **Writing success criteria that describe the charter's *scope* instead of its *outcome*.** "Build the login page" is scope; "a user can log in and see their data within 2 seconds" is a criterion. Reuse Week 1 Exercise 3's test if this trips you up.
- **Skipping the WIP limit** because you're solo and "obviously" only work on one thing at a time. In practice, solo builders context-switch between "quick fixes" constantly without moving cards — the limit exists to make that switching visible on the board, not to prevent something you'd never do anyway.
