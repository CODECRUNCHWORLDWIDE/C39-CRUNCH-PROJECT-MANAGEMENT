# Exercise 1 — Load an Issue Export into PostgreSQL/SQLite

**Goal:** Take a raw issue export and its status-change history — the two-file shape a real Jira or GitHub export actually comes in — and load both into a relational database correctly, with the relationship between them enforced, not just eyeballed.

**Estimated time:** 60 minutes.

## Setup

Make sure `sprints` and `issues` are already loaded from the [week README](../README.md). Confirm:

```sql
SELECT COUNT(*) FROM sprints;   -- 7
SELECT COUNT(*) FROM issues;    -- 28
```

If either is missing, go back and run the README's setup SQL first — this exercise assumes both tables exist and only adds `issue_status_history`.

## Task 1 — Create the table (5 min)

If you haven't already, create `issue_status_history` exactly as specified in the README:

```sql
CREATE TABLE issue_status_history (
    history_id  INTEGER PRIMARY KEY,
    issue_id    INTEGER NOT NULL REFERENCES issues(issue_id),
    status      TEXT    NOT NULL,
    entered_at  DATE    NOT NULL,
    left_at     DATE
);
```

On SQLite, run `PRAGMA foreign_keys = ON;` in this session before Task 2 — without it, an insert referencing a nonexistent `issue_id` would succeed silently instead of failing loudly, which defeats the entire point of the foreign key.

## Task 2 — Load the full status history (15 min)

Save this as `seed-issue-status-history.sql` and run it (`\i seed-issue-status-history.sql` in `psql`, `.read seed-issue-status-history.sql` in `sqlite3`) — or paste it directly into your shell.

```sql
INSERT INTO issue_status_history (history_id, issue_id, status, entered_at, left_at) VALUES
(1,1,'To Do','2026-01-05','2026-01-05'),
(2,1,'In Progress','2026-01-05','2026-01-13'),
(3,1,'In Review','2026-01-13','2026-01-15'),
(4,1,'Done','2026-01-15',NULL),
(5,2,'To Do','2026-01-05','2026-01-06'),
(6,2,'In Progress','2026-01-06','2026-01-12'),
(7,2,'In Review','2026-01-12','2026-01-14'),
(8,2,'Done','2026-01-14',NULL),
(9,3,'To Do','2026-01-08','2026-01-08'),
(10,3,'In Progress','2026-01-08','2026-01-09'),
(11,3,'In Review','2026-01-09','2026-01-10'),
(12,3,'Done','2026-01-10',NULL),
(13,4,'To Do','2026-01-19','2026-01-19'),
(14,4,'In Progress','2026-01-19','2026-01-27'),
(15,4,'In Review','2026-01-27','2026-01-29'),
(16,4,'Done','2026-01-29',NULL),
(17,5,'To Do','2026-01-19','2026-01-20'),
(18,5,'In Progress','2026-01-20','2026-01-26'),
(19,5,'In Review','2026-01-26','2026-01-28'),
(20,5,'Done','2026-01-28',NULL),
(21,6,'To Do','2026-01-19','2026-01-21'),
(22,6,'In Progress','2026-01-21','2026-01-27'),
(23,6,'In Review','2026-01-27','2026-01-29'),
(24,6,'Done','2026-01-29',NULL),
(25,7,'To Do','2026-01-22','2026-01-22'),
(26,7,'In Progress','2026-01-22','2026-01-23'),
(27,7,'In Review','2026-01-23','2026-01-24'),
(28,7,'Done','2026-01-24',NULL),
(29,8,'To Do','2026-02-02','2026-02-02'),
(30,8,'In Progress','2026-02-02','2026-02-10'),
(31,8,'In Review','2026-02-10','2026-02-12'),
(32,8,'Done','2026-02-12',NULL),
(33,9,'To Do','2026-02-02','2026-02-03'),
(34,9,'In Progress','2026-02-03','2026-02-09'),
(35,9,'In Review','2026-02-09','2026-02-11'),
(36,9,'Done','2026-02-11',NULL),
(37,10,'To Do','2026-02-02','2026-02-04'),
(38,10,'In Progress','2026-02-04','2026-02-10'),
(39,10,'In Review','2026-02-10','2026-02-12'),
(40,10,'Done','2026-02-12',NULL),
(41,11,'To Do','2026-02-02','2026-02-05'),
(42,11,'In Progress','2026-02-05','2026-02-09'),
(43,11,'In Review','2026-02-09','2026-02-10'),
(44,11,'Done','2026-02-10',NULL),
(45,12,'To Do','2026-02-16','2026-02-16'),
(46,12,'In Progress','2026-02-16','2026-02-19'),
(47,12,'Blocked','2026-02-19','2026-02-23'),
(48,12,'In Progress','2026-02-23','2026-02-26'),
(49,12,'In Review','2026-02-26','2026-02-27'),
(50,12,'Done','2026-02-27',NULL),
(51,13,'To Do','2026-02-16','2026-02-17'),
(52,13,'In Progress','2026-02-17','2026-02-24'),
(53,13,'In Review','2026-02-24','2026-02-26'),
(54,13,'Done','2026-02-26',NULL),
(55,14,'To Do','2026-02-16','2026-02-20'),
(56,14,'In Progress','2026-02-20','2026-02-25'),
(57,14,'In Review','2026-02-25','2026-02-26'),
(58,14,'Done','2026-02-26',NULL),
(59,15,'To Do','2026-02-20','2026-02-25'),
(60,15,'In Progress','2026-02-25','2026-02-27'),
(61,15,'Blocked','2026-02-27','2026-03-04'),
(62,15,'In Progress','2026-03-04','2026-03-10'),
(63,15,'In Review','2026-03-10','2026-03-11'),
(64,15,'Done','2026-03-11',NULL),
(65,16,'To Do','2026-03-02','2026-03-02'),
(66,16,'In Progress','2026-03-02','2026-03-10'),
(67,16,'In Review','2026-03-10','2026-03-12'),
(68,16,'Done','2026-03-12',NULL),
(69,17,'To Do','2026-03-02','2026-03-03'),
(70,17,'In Progress','2026-03-03','2026-03-09'),
(71,17,'In Review','2026-03-09','2026-03-11'),
(72,17,'Done','2026-03-11',NULL),
(73,18,'To Do','2026-03-05','2026-03-05'),
(74,18,'In Progress','2026-03-05','2026-03-06'),
(75,18,'In Review','2026-03-06','2026-03-07'),
(76,18,'Done','2026-03-07',NULL),
(77,19,'To Do','2026-03-16','2026-03-16'),
(78,19,'In Progress','2026-03-16','2026-03-18'),
(79,19,'Blocked','2026-03-18','2026-03-25'),
(80,19,'In Progress','2026-03-25','2026-03-26'),
(81,19,'In Review','2026-03-26','2026-03-27'),
(82,19,'Done','2026-03-27',NULL),
(83,20,'To Do','2026-03-16','2026-03-17'),
(84,20,'In Progress','2026-03-17','2026-03-20'),
(85,20,'Blocked','2026-03-20','2026-03-24'),
(86,20,'In Progress','2026-03-24','2026-03-26'),
(87,20,'In Review','2026-03-26','2026-03-27'),
(88,20,'Done','2026-03-27',NULL),
(89,21,'To Do','2026-03-19','2026-03-19'),
(90,21,'In Progress','2026-03-19','2026-03-20'),
(91,21,'In Review','2026-03-20','2026-03-21'),
(92,21,'Done','2026-03-21',NULL),
(93,22,'To Do','2026-03-23','2026-03-26'),
(94,22,'In Progress','2026-03-26','2026-03-27'),
(95,22,'Blocked','2026-03-27','2026-03-31'),
(96,22,'In Progress','2026-03-31','2026-04-01'),
(97,22,'In Review','2026-04-01','2026-04-01'),
(98,22,'Done','2026-04-01',NULL),
(99,23,'To Do','2026-03-30','2026-04-02'),
(100,23,'In Progress','2026-04-02',NULL),
(101,24,'To Do','2026-03-30','2026-03-30'),
(102,24,'In Progress','2026-03-30','2026-04-03'),
(103,24,'In Review','2026-04-03','2026-04-05'),
(104,24,'Done','2026-04-05',NULL),
(105,25,'To Do','2026-04-01','2026-04-02'),
(106,25,'In Progress','2026-04-02','2026-04-06'),
(107,25,'In Review','2026-04-06',NULL),
(108,26,'To Do','2026-03-30','2026-04-01'),
(109,26,'In Progress','2026-04-01','2026-04-03'),
(110,26,'Blocked','2026-04-03',NULL),
(111,27,'To Do','2026-03-30',NULL),
(112,28,'Backlog','2026-04-02',NULL);
```

## Task 3 — Verify the load (10 min)

Run each check and confirm the expected value:

```sql
SELECT COUNT(*) FROM issue_status_history;                              -- 112
SELECT COUNT(DISTINCT issue_id) FROM issue_status_history;              -- 28 (every issue has history)
SELECT COUNT(*) FROM issue_status_history WHERE left_at IS NULL;        -- 6 (the currently-open intervals)
SELECT COUNT(*) FROM issue_status_history WHERE status = 'Blocked';     -- 6 (5 closed + 1 still open)
```

If `COUNT(DISTINCT issue_id)` comes back less than 28, at least one issue has no history rows — find it:

```sql
SELECT i.issue_key
FROM issues i
LEFT JOIN issue_status_history h ON h.issue_id = i.issue_id
WHERE h.issue_id IS NULL;
```

That query — a `LEFT JOIN` back to the parent looking for `NULL` — is the general pattern for "find the rows on one side with no match on the other." You'll use it constantly once you start working with any parent/child table pair.

## Task 4 — Confirm referential integrity is actually enforced (10 min)

Prove the foreign key is doing its job, don't just assume it:

```sql
-- this MUST fail — issue_id 999 doesn't exist
INSERT INTO issue_status_history (history_id, issue_id, status, entered_at, left_at)
VALUES (999, 999, 'In Progress', '2026-04-06', NULL);
```

You should see a foreign-key-violation error. If it silently succeeds, you forgot `PRAGMA foreign_keys = ON;` (SQLite) or you're not actually connected to a table with the constraint (Postgres enforces it by default — check you ran the `CREATE TABLE` from the README exactly). Clean up if it did insert: `DELETE FROM issue_status_history WHERE history_id = 999;`

## Task 5 — One join to check your understanding (15 min)

Write a query that lists every issue's `issue_key`, its **current** status per the `issues.current_status` column, and its **most recent** status per `issue_status_history` (the row where `left_at IS NULL`) — and confirm the two always agree. They should, on every row: `issues.current_status` was derived directly from the open interval in the history when this data was built.

```sql
SELECT i.issue_key,
       i.current_status AS status_on_issues_table,
       h.status         AS status_from_history
FROM issues i
JOIN issue_status_history h ON h.issue_id = i.issue_id AND h.left_at IS NULL
WHERE i.current_status <> h.status;   -- should return ZERO rows
```

## Done when…

- [ ] `issue_status_history` has exactly 112 rows.
- [ ] Every one of the 28 issues has at least one history row (Task 3's `LEFT JOIN` check returns nothing).
- [ ] You proved the foreign key rejects a bad `issue_id` (Task 4).
- [ ] Task 5's consistency check returns zero rows.
- [ ] You can explain, in one sentence, why `left_at IS NULL` means "this is the issue's current status."

## Stretch

- Write a query counting how many history rows have `left_at IS NULL` **and** `status <> 'Done'` — these are exactly the issues still actively open. Compare the count to the WIP query from Lecture 2 and explain why they're *not* the same count (hint: `To Do` and `Backlog` rows can also have `left_at IS NULL`).
- For one issue of your choice, write a query that reconstructs its full lifecycle as a single readable row: `issue_key`, `first_to_do`, `first_in_progress`, `total_blocked_days`, `resolved_at`.

## Submission

Commit `seed-issue-status-history.sql` plus your verification queries to your portfolio under `c39-week-08/exercise-01/`.
