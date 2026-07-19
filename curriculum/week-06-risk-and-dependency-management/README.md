# Week 6 — Risk & Dependency Management

> **Goal:** by Sunday you can stand up a live, queryable risk register (RAID log) with real owners and triggers, map the cross-team dependencies threatening your schedule, and point at the exact chain of tasks — the critical path — where a single day's slip becomes a missed launch.

Welcome back to **C39 · Crunch Project Management**. So far you've learned to frame a project (Week 1), run it in Agile ceremonies (Week 2), build a backlog (Week 3), and — assuming Weeks 4–5 are already behind you — estimate the work and lay it into sprints and a roadmap. All of that planning assumes the happy path. This week is about the *unhappy* paths: what could go wrong, who's waiting on whom, and where in your schedule a delay actually costs you a launch date versus where it's absorbed for free. A PM who can't answer "what's our biggest risk right now" and "what's on the critical path right now" in one sentence each isn't managing the project — they're documenting it after the fact.

We continue the **Project Atlas** case study from Northlight Software. Atlas — the Team Workspaces feature — is now mid-execution, roughly five sprints into an eight-sprint plan. You already know from Week 1 that a third-party real-time API had a flaky sandbox; that risk has been sitting in an informal Slack thread instead of a real register, and it just started to bite. You also now depend on two other teams — Platform and Design Systems — who have their own priorities and their own blockers. This week you build the tooling to see both problems coming instead of discovering them in a status meeting.

**Following this course's data-tooling rule:** a risk register and a dependency map are *data* — rows with owners, scores, statuses, and relationships that change over time and that you need to query, filter, and re-rank constantly ("show me every open, high-score risk with no assigned response," "show me every task blocked on Platform"). That's a database's job, not a spreadsheet's. Every artifact this week lives in PostgreSQL (SQLite fallback) and is queried with SQL; the critical-path math is computed with Python (plain Python / pandas, no spreadsheet formulas). You'll see exactly why a spreadsheet actively hides the two things a live risk register needs — concurrent updates from multiple owners, and joins between risks, dependencies, and the schedule — while a database makes both trivial.

## Learning objectives

By the end of this week, you will be able to:

- **Identify** risks from a real project context and **score** them by probability × impact on a consistent, defensible scale.
- **Maintain** a live risk register with owners, triggers, and responses — the RAID model (Risks, Assumptions, Issues, Dependencies) — as a queryable SQL table, not a document.
- **Choose** the right response strategy for a scored risk: avoid, mitigate, transfer, or accept — and explain the trade-off each one makes.
- **Map** dependencies within a team and across teams, and make coupling visible instead of discovering it mid-sprint.
- **Compute** the critical path of a small schedule by hand and in Python, and explain slack/float in plain language.
- **Protect** the critical path deliberately — buffering the right tasks, not every task — instead of reacting to slips after they happen.

## Prerequisites

- Weeks 1–3 of this course (charter, Agile ceremonies, backlog/stories). Weeks 4–5 (estimation, roadmapping) are assumed but not strictly required — this week is self-contained if you skipped ahead.
- **PostgreSQL 16+** (primary) or **SQLite 3.35+** (fallback) — the same install from [C33 Crunch SQL](../../../C33-CRUNCH-SQL/) if you've taken it, or see [`resources.md`](./resources.md) for install steps.
- **Python 3.10+** with `pandas` installed (`pip install pandas`) — used for the critical-path calculations in Lecture 3 and Exercise 3.
- Comfortable with basic `SELECT`/`INSERT`/`WHERE`/`ORDER BY` SQL. If that's shaky, C33 Weeks 1–2 cover it; this week doesn't require joins or window functions, just steady use of single-table queries.

## Set up this week's database (do this first)

Everything this week runs against one small schema: a `risks` table (the RAID risk log) and a `dependencies` table (the cross-team dependency map). Create the database once, before Lecture 1.

**PostgreSQL:**

```bash
createdb atlas_pm
psql atlas_pm
```

**SQLite:**

```bash
sqlite3 atlas_pm.db
```

Then paste this into the shell (works unchanged on both engines):

```sql
CREATE TABLE risks (
    risk_id       INTEGER PRIMARY KEY,
    title         TEXT    NOT NULL,
    description   TEXT,
    category      TEXT    NOT NULL,   -- 'technical' | 'schedule' | 'vendor' | 'people' | 'scope'
    probability   INTEGER NOT NULL,   -- 1 (rare) to 5 (near certain)
    impact        INTEGER NOT NULL,   -- 1 (negligible) to 5 (severe)
    owner         TEXT    NOT NULL,
    trigger_event TEXT,               -- observable signal the risk is materializing
    response_type TEXT,               -- 'avoid' | 'mitigate' | 'transfer' | 'accept'
    response_plan TEXT,
    status        TEXT    NOT NULL DEFAULT 'open',  -- 'open' | 'mitigated' | 'occurred' | 'closed'
    date_logged   DATE    NOT NULL
);

CREATE TABLE dependencies (
    dependency_id     INTEGER PRIMARY KEY,
    depends_on_team    TEXT    NOT NULL,  -- team/vendor that must deliver
    needed_by_team     TEXT    NOT NULL,  -- team waiting on it
    what_is_needed     TEXT    NOT NULL,
    needed_by_date     DATE,
    status              TEXT    NOT NULL DEFAULT 'not_started', -- 'not_started' | 'in_progress' | 'delivered' | 'blocked'
    coordination_owner  TEXT    NOT NULL   -- who is actively chasing this
);
```

You'll populate both tables through this week's lectures and exercises — this is a *live* register, so you keep inserting and updating rows as you go, exactly like you would on a real project.

## How to navigate this week

Work top to bottom. Each piece assumes the ones above it.

| # | File | What's inside | ~Time |
|--:|------|---------------|------:|
| 1 | [lecture-notes/01-risk-registers-and-raid.md](./lecture-notes/01-risk-registers-and-raid.md) | Identifying and scoring risk, response strategies, and running a live RAID log in SQL | 2h |
| 2 | [lecture-notes/02-dependency-mapping.md](./lecture-notes/02-dependency-mapping.md) | Internal and cross-team dependencies, coordination cost, and reducing coupling | 2h |
| 3 | [lecture-notes/03-critical-path-and-slack.md](./lecture-notes/03-critical-path-and-slack.md) | Finding the critical path, float/slack, and where to buffer | 2h |
| 4 | [exercises/exercise-01-score-and-rank-risks.md](./exercises/exercise-01-score-and-rank-risks.md) | Score and rank 12 raw risks by probability × impact | 1h |
| 5 | [exercises/exercise-02-assign-risk-responses.md](./exercises/exercise-02-assign-risk-responses.md) | Assign avoid/mitigate/transfer/accept to each scored risk | 1h |
| 6 | [exercises/exercise-03-find-the-critical-path.md](./exercises/exercise-03-find-the-critical-path.md) | Compute the critical path and float for a 9-task plan | 1.5h |
| 7 | [challenges/challenge-01-live-register-for-a-project.md](./challenges/challenge-01-live-register-for-a-project.md) | Build a full live risk register for a real project | 1.5h |
| 8 | [challenges/challenge-02-untangle-cross-team-deps.md](./challenges/challenge-02-untangle-cross-team-deps.md) | Diagnose and fix a 4-hop blocked dependency chain | 1.5h |
| 9 | [mini-project/README.md](./mini-project/README.md) | Live RAID register + dependency map + critical path for one project | 3h |
| 10 | [homework.md](./homework.md) | Extra practice, spread across the week | 4h |
| 11 | [quiz.md](./quiz.md) | 14 self-check questions + answer key | 1h |
| 12 | [resources.md](./resources.md) | Curated free/official reading + tools to install | — |

## Weekly schedule

The schedule below adds up to approximately **20.5 hours** for the full-time pace.

| Day | Focus | Lectures | Exercises | Challenges | Quiz/Read | Homework | Mini-Project | Daily Total |
|-----------|--------------------------------------------|---------:|----------:|-----------:|----------:|---------:|-------------:|------------:|
| Monday | Risk identification + scoring + RAID log | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Tuesday | Response strategies; dependency mapping | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Wednesday | Coordination cost; critical path & slack | 2h | 1.5h | 0h | 0.5h | 1h | 0h | 5h |
| Thursday | Challenges: live register + untangle deps | 0h | 0h | 1.5h | 0.5h | 1h | 1h | 4h |
| Friday | Challenges continued; start mini-project | 0h | 0h | 1.5h | 0.5h | 0h | 1.5h | 3.5h |
| Saturday | Mini-project — register + map + path | 0h | 0h | 0h | 0h | 0h | 2.5h | 2.5h |
| Sunday | Quiz + review | 0h | 0h | 0h | 1h | 0h | 0h | 1h |
| **Total** | | **6h** | **3.5h** | **3h** | **3.5h** | **4h** | **5h** | **~25h** |

## By the end of this week you can…

- Take a raw list of "things that could go wrong" and turn it into a scored, ranked, owned risk register in SQL — not a static list nobody updates.
- Pick the right response (avoid, mitigate, transfer, accept) for a risk and defend the trade-off out loud.
- Draw the real dependency graph behind a project — inside your team and across teams — and name where coordination cost is highest.
- Find the critical path in a schedule, explain float to a non-PM in one sentence, and say specifically where a buffer belongs and where it's wasted.

## Up next

Week 7 — Stakeholder & Communication Management — once you know what could go wrong and what's blocking what, next you learn to keep the right people informed before either one surprises them.

---

*Part of the Code Crunch Worldwide open curriculum · GPL-3.0 · If you find errors, please open an issue or PR.*
