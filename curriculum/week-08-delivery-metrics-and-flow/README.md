# Week 8 — Delivery Metrics & Flow

> **Goal:** by Sunday you can take a raw Jira/GitHub issue export, load it into PostgreSQL or SQLite, and answer "is this team actually delivering?" with numbers you computed yourself — velocity, throughput, cycle-time percentiles, WIP, and a burndown — and you can explain exactly what each metric hides as well as what it shows.

Welcome back to **C39 · Crunch Project Management**. Weeks 1–7 built the artifacts of running a project on purpose: a charter, a backlog, sprints, a roadmap, a risk register, a dependency map, a stakeholder map, a communication plan. This week we point the same rigor at a question every PM eventually gets asked in a tone that is not friendly: **"why is this taking so long?"** The honest answer is never a vibe. It's a number, computed from the team's own history, that tells you whether delivery is actually slowing down, speeding up, or just *feels* different because last Tuesday was loud.

We continue the **Project Atlas** case study from Northlight Software. Atlas has now run seven two-week sprints. Sprint 6's risk register (Week 6) flagged two risks as "watching closely" — the third-party real-time API's flaky sandbox, and the Platform team's webhook dependency owned by **Sofia Reyes**'s team. By Week 7 both had started to bite. This week you get the receipts: a full export of Atlas's 28 issues and their status history, from Sprint 1 through the middle of the currently-open Sprint 7. **Marcus Diallo** (Tech Lead) pulled the export this morning — 2026-04-06, a Monday, day 8 of Sprint 7's 12 calendar days — because **Priya Chen** (VP Product, sponsor) asked the question above in yesterday's steering meeting, and "it feels slower" was not going to be an acceptable answer twice.

**Following this course's data-tooling rule:** an issue export with per-issue fields and a full status-change history is *relational, queryable, versioned data* — dozens of rows you'll filter, group, join against a sprint calendar, and re-run every week as the export refreshes. That is a database's job, not a spreadsheet's. Every artifact this week lives in the `atlas_pm` database you've been building since Week 6, in two new tables (`sprints`, `issues`, `issue_status_history`), queried with SQL and analyzed with pandas. No spreadsheet touches this week's work — and Lecture 2 spends a paragraph on exactly why a spreadsheet would quietly lie to you here.

## Learning objectives

By the end of this week, you will be able to:

- **Define** velocity, throughput, cycle time, lead time, and WIP precisely — not as buzzwords, but as things you can compute from timestamps, and explain how each differs from the others.
- **State** Little's Law (`WIP = Throughput × Cycle Time`) and use it to sanity-check whether a team's flow numbers are internally consistent.
- **Load** an exported issue set and its status-change history into PostgreSQL/SQLite and model the relationship between issues, statuses, and sprints correctly.
- **Compute** velocity, throughput, and a sprint burndown/burnup directly in SQL with `GROUP BY` and window functions — no pivot table, no macro.
- **Analyze** cycle-time distributions, aging WIP, and flow efficiency in pandas, and explain why percentiles (p50/p85/p95) tell you more than an average.
- **Read** a cumulative flow diagram (CFD) and diagnose *where* in the workflow a team is stalling from the shape of the bands, not a guess.
- **Interpret** every metric in this week as a **signal**, not a target — and name the specific way each one gets gamed when it's used as one.
- **Explain**, specifically, why delivery data belongs in SQL/pandas and what breaks the moment it lives in a spreadsheet-as-database instead.

## Prerequisites

- Weeks 1–7 of this course, especially Week 6 (risk register — the sandbox and Platform-team risks this week's data brings to life) and the `atlas_pm` database you've been extending since then.
- **PostgreSQL 16+** (primary) or **SQLite 3.35+** (fallback). If you don't have `atlas_pm` yet, see [`resources.md`](./resources.md) for a from-scratch setup — this week's seed is self-contained and doesn't require the Week 6/7 tables.
- Comfortable `SELECT` / `WHERE` / `GROUP BY` / `JOIN` SQL (Weeks 1–7 have used all of these). This week adds window functions (`SUM(...) OVER (...)`) — the SQL lecture teaches them from scratch, assuming no prior exposure.
- **Python 3.10+ with pandas** installed (`pip install pandas`). No prior pandas experience required; the pandas lecture starts from `pd.read_sql`.

## Set up this week's data (do this first)

This week's case study is **Atlas's issue tracker export** — the kind of CSV you'd get from Jira's "Export issues to CSV" or GitHub's issue/PR API, joined with a changelog of status transitions. Everything this week runs against three tables. Create (or reuse) `atlas_pm`, then run this:

```sql
CREATE TABLE sprints (
    sprint_name TEXT PRIMARY KEY,
    start_date  DATE NOT NULL,
    end_date    DATE NOT NULL,
    goal        TEXT NOT NULL
);

INSERT INTO sprints VALUES
('Sprint 1','2026-01-05','2026-01-16','Stand up the real-time API auth + connection layer'),
('Sprint 2','2026-01-19','2026-01-30','Live state sync and presence'),
('Sprint 3','2026-02-02','2026-02-13','Spectator mode and API hardening'),
('Sprint 4','2026-02-16','2026-02-27','Feed-provider failover'),
('Sprint 5','2026-03-02','2026-03-13','Multi-region routing and degraded-mode UX'),
('Sprint 6','2026-03-16','2026-03-27','Platform-team webhook integration'),
('Sprint 7','2026-03-30','2026-04-10','Provider signature verification + SLA dashboard');

CREATE TABLE issues (
    issue_id       INTEGER PRIMARY KEY,
    issue_key      TEXT    NOT NULL UNIQUE,   -- 'ATLAS-101'
    issue_type     TEXT    NOT NULL,          -- 'story' | 'bug' | 'task'
    summary        TEXT    NOT NULL,
    story_points   NUMERIC,                   -- NULL when not sized (most bugs)
    sprint         TEXT    NOT NULL REFERENCES sprints(sprint_name),
    assignee       TEXT    NOT NULL,
    priority       TEXT    NOT NULL,          -- 'low' | 'medium' | 'high' | 'urgent'
    created_at     DATE    NOT NULL,
    resolved_at    DATE,                      -- NULL if not yet Done
    current_status TEXT    NOT NULL           -- 'Backlog' | 'To Do' | 'In Progress' | 'Blocked' | 'In Review' | 'Done'
);

CREATE TABLE issue_status_history (
    history_id  INTEGER PRIMARY KEY,
    issue_id    INTEGER NOT NULL REFERENCES issues(issue_id),
    status      TEXT    NOT NULL,
    entered_at  DATE    NOT NULL,
    left_at     DATE                          -- NULL = still in this status right now
);
```

On SQLite, run `PRAGMA foreign_keys = ON;` in every session before inserting — SQLite accepts `REFERENCES` silently but won't enforce it otherwise.

Now the data. This is the whole export — 28 issues across seven sprints, Sprint 7 still open as of the **2026-04-06** snapshot:

```sql
INSERT INTO issues VALUES
(1,'ATLAS-101','story','Real-time API auth handshake',8,'Sprint 1','Marcus Diallo','medium','2025-12-30','2026-01-15','Done'),
(2,'ATLAS-102','story','Websocket connection manager',5,'Sprint 1','Nadia Petrova','medium','2025-12-30','2026-01-14','Done'),
(3,'ATLAS-103','bug','Fix auth token refresh race',NULL,'Sprint 1','Chris Okoye','high','2026-01-08','2026-01-10','Done'),
(4,'ATLAS-104','story','Live match state sync',8,'Sprint 2','Wei Zhang','medium','2026-01-14','2026-01-29','Done'),
(5,'ATLAS-105','story','Reconnect and backoff logic',5,'Sprint 2','Marcus Diallo','medium','2026-01-14','2026-01-28','Done'),
(6,'ATLAS-106','story','Presence indicators',5,'Sprint 2','Fatima Noor','medium','2026-01-14','2026-01-29','Done'),
(7,'ATLAS-107','bug','Duplicate events on reconnect',NULL,'Sprint 2','Chris Okoye','high','2026-01-22','2026-01-24','Done'),
(8,'ATLAS-108','story','Spectator mode data feed',8,'Sprint 3','Nadia Petrova','medium','2026-01-28','2026-02-12','Done'),
(9,'ATLAS-109','story','Rate limiting for API client',5,'Sprint 3','Wei Zhang','medium','2026-01-28','2026-02-11','Done'),
(10,'ATLAS-110','story','Historical replay endpoint',5,'Sprint 3','Marcus Diallo','medium','2026-01-28','2026-02-12','Done'),
(11,'ATLAS-111','story','Latency monitoring dashboard hooks',3,'Sprint 3','Fatima Noor','medium','2026-01-28','2026-02-10','Done'),
(12,'ATLAS-112','story','Failover to backup feed provider',8,'Sprint 4','Marcus Diallo','high','2026-02-11','2026-02-27','Done'),
(13,'ATLAS-113','story','Client-side event buffering',5,'Sprint 4','Chris Okoye','medium','2026-02-11','2026-02-26','Done'),
(14,'ATLAS-114','task','Load test the real-time pipeline',2,'Sprint 4','Wei Zhang','medium','2026-02-11','2026-02-26','Done'),
(15,'ATLAS-115','story','Circuit breaker for the flaky sandbox',5,'Sprint 5','Nadia Petrova','high','2026-02-18','2026-03-11','Done'),
(16,'ATLAS-116','story','Multi-region read-replica routing',8,'Sprint 5','Wei Zhang','medium','2026-02-25','2026-03-12','Done'),
(17,'ATLAS-117','story','Graceful degradation banner (UI)',4,'Sprint 5','Fatima Noor','medium','2026-02-25','2026-03-11','Done'),
(18,'ATLAS-118','bug','Memory leak in websocket manager',NULL,'Sprint 5','Chris Okoye','high','2026-03-05','2026-03-07','Done'),
(19,'ATLAS-119','story','Platform-team webhook integration',8,'Sprint 6','Marcus Diallo','high','2026-03-11','2026-03-27','Done'),
(20,'ATLAS-120','story','Sandbox failover integration test',5,'Sprint 6','Nadia Petrova','high','2026-03-11','2026-03-27','Done'),
(21,'ATLAS-121','bug','Intermittent 502s from the sandbox',NULL,'Sprint 6','Wei Zhang','urgent','2026-03-19','2026-03-21','Done'),
(22,'ATLAS-122','story','Backfill job for replay data',3,'Sprint 7','Chris Okoye','high','2026-03-20','2026-04-01','Done'),
(23,'ATLAS-123','story','Provider webhook signature verification',5,'Sprint 7','Marcus Diallo','medium','2026-03-25','NULL','In Progress'),
(24,'ATLAS-124','story','Spectator mode pagination',5,'Sprint 7','Nadia Petrova','medium','2026-03-25','2026-04-05','Done'),
(25,'ATLAS-125','bug','Race condition in presence indicator',NULL,'Sprint 7','Fatima Noor','high','2026-04-01','NULL','In Review'),
(26,'ATLAS-126','story','Platform-team SLA dashboard',8,'Sprint 7','Wei Zhang','high','2026-03-25','NULL','Blocked'),
(27,'ATLAS-127','task','Runbook for sandbox outages',2,'Sprint 7','Chris Okoye','low','2026-03-28','NULL','To Do'),
(28,'ATLAS-128','story','Auto-retry policy for the provider client',5,'Sprint 7','Marcus Diallo','medium','2026-04-02','NULL','Backlog');
```

Two engine notes: write literal `NULL` (unquoted) for `story_points` and `resolved_at`, not the string `'NULL'` — copy-paste carefully. And SQLite has no real `NUMERIC` enforcement, so a `NULL` story point round-trips fine on both engines.

The full 112-row status-change history is long, so it lives in its own file — copy it in after the `issues` insert above:

```bash
# from the psql or sqlite3 shell:
\i seed-issue-status-history.sql        -- psql
.read seed-issue-status-history.sql     -- sqlite3
```

Rather than reproduce all 112 rows twice, the exact `INSERT` statements are given in full in [`exercises/exercise-01-load-issue-export.md`](./exercises/exercise-01-load-issue-export.md) — Exercise 1 **is** loading this file, so you'll type or paste it there as your first task. Sanity check once loaded:

```sql
SELECT COUNT(*) FROM issues;               -- 28
SELECT COUNT(*) FROM issue_status_history; -- 112
SELECT COUNT(*) FROM issues WHERE current_status = 'Done';  -- 23
```

Five issues are still open in Sprint 7 as of the snapshot: one `In Progress`, one `In Review`, one `Blocked` (on the Platform team, again), one `To Do`, and one still sitting in `Backlog` — that spread of five is not an accident, and by the end of this week you'll be able to say exactly what it's telling you.

## How to navigate this week

Work top to bottom. Each piece assumes the ones above it.

| # | File | What's inside | ~Time |
|--:|------|---------------|------:|
| 1 | [lecture-notes/01-the-flow-metrics.md](./lecture-notes/01-the-flow-metrics.md) | Velocity vs. throughput, cycle time vs. lead time, WIP, and Little's Law — what each metric tells you and hides | 2h |
| 2 | [lecture-notes/02-metrics-in-sql.md](./lecture-notes/02-metrics-in-sql.md) | Modeling issues + status history in SQL; velocity, throughput, and burndown/burnup with `GROUP BY` and window functions | 2h |
| 3 | [lecture-notes/03-flow-analysis-in-pandas.md](./lecture-notes/03-flow-analysis-in-pandas.md) | Cycle-time distributions, aging WIP, flow efficiency, and reading a cumulative flow diagram in pandas | 2h |
| 4 | [exercises/exercise-01-load-issue-export.md](./exercises/exercise-01-load-issue-export.md) | Load the full issue export + status history into PostgreSQL/SQLite | 1h |
| 5 | [exercises/exercise-02-query-velocity-and-cycle-time.md](./exercises/exercise-02-query-velocity-and-cycle-time.md) | Query velocity, throughput, and cycle time in SQL | 1.5h |
| 6 | [exercises/exercise-03-cycle-time-percentiles.md](./exercises/exercise-03-cycle-time-percentiles.md) | Compute cycle-time percentiles and flow efficiency in pandas | 1.5h |
| 7 | [challenges/challenge-01-build-a-flow-dashboard.md](./challenges/challenge-01-build-a-flow-dashboard.md) | Build a reusable flow-dashboard query set from the raw export | 1.5h |
| 8 | [challenges/challenge-02-diagnose-a-stalling-team.md](./challenges/challenge-02-diagnose-a-stalling-team.md) | Diagnose exactly why Atlas's flow is degrading, from the data alone | 1.5h |
| 9 | [mini-project/README.md](./mini-project/README.md) | Ship a full flow dashboard — velocity, throughput, cycle-time percentiles, burndown, and a written read | 3h |
| 10 | [homework.md](./homework.md) | Extra practice, spread across the week | 4h |
| 11 | [quiz.md](./quiz.md) | 15 self-check questions + answer key | 1h |
| 12 | [resources.md](./resources.md) | Curated free/official reading + tools to install | — |

## Weekly schedule

The schedule below adds up to approximately **24.5 hours** for the full-time pace.

| Day | Focus | Lectures | Exercises | Challenges | Quiz/Read | Homework | Mini-Project | Daily Total |
|-----------|--------------------------------------------|---------:|----------:|-----------:|----------:|---------:|-------------:|------------:|
| Monday | Flow metrics concepts; load the export | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Tuesday | Metrics in SQL — velocity, burndown | 2h | 1.5h | 0h | 0.5h | 1h | 0h | 5h |
| Wednesday | Flow analysis in pandas — percentiles, CFD | 2h | 1.5h | 0h | 0.5h | 1h | 0h | 5h |
| Thursday | Challenge 1 — flow dashboard | 0h | 0h | 1.5h | 0.5h | 1h | 0.5h | 3.5h |
| Friday | Challenge 2 — diagnose the stall | 0h | 0h | 1.5h | 0.5h | 0h | 1h | 3h |
| Saturday | Mini-project — the full dashboard | 0h | 0h | 0h | 0h | 0h | 2.5h | 2.5h |
| Sunday | Quiz + review | 0h | 0h | 0h | 1h | 0h | 0h | 1h |
| **Total** | | **6h** | **4h** | **3h** | **3.5h** | **4h** | **4h** | **~24.5h** |

## By the end of this week you can…

- Define velocity, throughput, cycle time, lead time, and WIP correctly, on the first try, without confusing any two of them.
- Take a raw issue export and status changelog and load it into a queryable relational store — never a spreadsheet — modeling the sprint/issue/status relationship correctly.
- Write the SQL that computes a sprint's velocity, a rolling throughput, and a day-by-day burndown using `GROUP BY` and window functions, from memory.
- Load the same data into pandas and compute cycle-time percentiles (p50/p85/p95), explain why they beat a plain average, and read a cumulative flow diagram to spot exactly where work is piling up.
- Look at a team whose velocity is dropping and, using only the data, tell the difference between "this team got worse" and "this team hit a real, external blocker" — and say which one Atlas is.
- Push back, with specifics, on anyone who wants to turn a flow metric into a target — and explain the gaming behavior that specific target would produce.

## Up next

[Week 9 — Budgeting, resourcing & capacity](../week-09-budgeting-resourcing-and-capacity/) — once you can measure what a team actually delivers, the next question is what it costs to keep delivering it, modeled in SQL and pandas instead of a budget spreadsheet.

---

*Part of the Code Crunch Worldwide open curriculum · GPL-3.0 · If you find errors, please open an issue or PR.*
