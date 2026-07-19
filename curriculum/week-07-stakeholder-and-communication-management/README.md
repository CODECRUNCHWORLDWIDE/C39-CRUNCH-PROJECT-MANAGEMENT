# Week 7 — Stakeholder & Communication Management

> **Goal:** by Sunday you can plot any project's stakeholders on a power/interest grid and defend each placement, run a communication plan that gets the right message to the right person on the right cadence without a single broadcast email, write status reports people actually trust because they never hide a slip, escalate with a clear ask instead of a complaint, and process a change request or a stakeholder conflict without drama.

Welcome back to **C39 · Crunch Project Management**. Weeks 1–6 gave you the artifacts that make a project *legible*: a charter, a backlog, sprints, a roadmap, a risk register, a dependency map, a critical path. All of that work is wasted if the people who need to know about it don't — or worse, don't trust what you tell them. Most projects that fail don't fail because the plan was wrong; they fail because someone who mattered found out too late, heard something different from two different people, or stopped believing the status report. This week is about the layer of the job that has nothing to do with tools and everything to do with judgment: who needs to know what, how you tell them, and what you do when the news is bad or two of them want opposite things.

We continue the **Project Atlas** case study from Northlight Software. Atlas is now six sprints into its eight-sprint plan. The Week 6 risk register caught fire early — the third-party real-time API's flaky sandbox has become a real, materializing risk, and the Platform-team dependency you mapped is running two days behind. None of that is a communication problem by itself. What turns it into one is the cast of people who have a stake in Atlas but who you haven't been managing on purpose: **Priya Chen** (VP Product, sponsor), **Elena Cruz** (Product Owner), **Marcus Diallo** (Tech Lead), **Sofia Reyes** (Platform Team Lead, the team you now depend on), **Derek Huang** (Design Systems Lead), **Amara Osei** (VP Sales — high power, has said almost nothing about Atlas so far, which is exactly the problem), **Tom Reilly** (CFO, watching the burn), and **Jamie Okafor** (Customer Success Lead, the voice of the three at-risk enterprise accounts the whole project exists to retain). By the end of this week you'll have mapped every one of them, built a communication plan tailored to each, written a real status report, and rehearsed the two conversations every PM eventually has to have: escalating cleanly, and telling someone senior their timeline just slipped.

**Following this course's data-tooling rule:** a stakeholder map, a communication plan, and a status history are *data* — rows with scores, cadences, dates, and relationships you need to query and re-sort constantly ("who's overdue for an update," "show every stakeholder in the Manage Closely quadrant," "pull every status report where schedule was Red"). That's a database's job. Every artifact this week extends the `atlas_pm` database from Week 6 with new tables (`stakeholders`, `comms_plan`, `status_log`, `change_requests`), queried in SQL, with any light number-crunching done in Python/pandas. No spreadsheet touches this week's work.

## Learning objectives

By the end of this week, you will be able to:

- **Map** stakeholders by power and interest onto a four-quadrant grid, and plan a distinct engagement strategy for each quadrant instead of one broadcast for everyone.
- **Identify** the sponsor, the silent blockers, and the stakeholders whose low visible interest hides high power to derail a project.
- **Build** a communication plan that specifies, per audience: the message, the channel, and the cadence — and defend why each choice fits that stakeholder.
- **Write** status reporting that is honest, scannable in under a minute, and never buries a slip behind good news.
- **Escalate** a blocked or at-risk item cleanly: a fact, an impact, a clear ask, and a recommendation — not a complaint.
- **Run** a lightweight, explicit change-request process instead of letting scope drift in through Slack DMs.
- **Handle** a difficult conversation — two stakeholders who want opposite things — without picking a side by default or avoiding the conversation.

## Prerequisites

- Weeks 1–6 of this course, especially Week 1's charter (you'll need the Atlas cast and constraints) and Week 6's risk register and dependency map (this week's status reports reference both directly).
- **PostgreSQL 16+** (primary) or **SQLite 3.35+** (fallback) — the same `atlas_pm` database you created in Week 6. If you skipped Week 6 or need to recreate it, see [`resources.md`](./resources.md) for a from-scratch setup.
- Comfortable with basic `SELECT` / `INSERT` / `UPDATE` / `WHERE` / `ORDER BY` SQL — this week adds a `JOIN` or two but nothing you haven't seen by Week 6.
- No new Python needed beyond what Week 6 used (plain `pandas`, optional).

## Set up this week's data (do this first)

Everything this week extends the `atlas_pm` database from Week 6 with four new tables. If you don't have `atlas_pm` yet, create it first (`createdb atlas_pm` / `sqlite3 atlas_pm.db`), then run this either way:

```sql
CREATE TABLE stakeholders (
    stakeholder_id     INTEGER PRIMARY KEY,
    name                TEXT    NOT NULL,
    role                TEXT    NOT NULL,
    org_unit            TEXT    NOT NULL,   -- 'Product' | 'Engineering' | 'Platform Team' | 'Design Systems' | 'Sales' | 'Finance' | 'Customer Success' | 'Customer'
    power               INTEGER NOT NULL,   -- 1 (none) to 5 (can kill or fund the project alone)
    interest            INTEGER NOT NULL,   -- 1 (indifferent) to 5 (checks in daily)
    quadrant            TEXT,               -- 'manage_closely' | 'keep_satisfied' | 'keep_informed' | 'monitor'
    preferred_channel   TEXT,
    cadence             TEXT,
    silent_blocker_risk BOOLEAN NOT NULL DEFAULT FALSE,  -- high power, currently low visible engagement
    notes               TEXT
);

CREATE TABLE comms_plan (
    comms_id       INTEGER PRIMARY KEY,
    stakeholder_id INTEGER NOT NULL REFERENCES stakeholders(stakeholder_id),
    message_focus  TEXT    NOT NULL,   -- what THIS audience specifically needs to know
    channel        TEXT    NOT NULL,   -- '1:1' | 'email digest' | 'Slack channel' | 'demo' | 'exec status doc' | 'dashboard'
    cadence        TEXT    NOT NULL,   -- 'weekly' | 'biweekly' | 'sprint-end' | 'on-trigger' | 'monthly'
    owner          TEXT    NOT NULL,
    last_sent      DATE,
    next_due       DATE
);

CREATE TABLE status_log (
    status_id       INTEGER PRIMARY KEY,
    report_date     DATE    NOT NULL,
    overall_rag     TEXT    NOT NULL,   -- 'green' | 'amber' | 'red'
    scope_rag       TEXT    NOT NULL,
    schedule_rag    TEXT    NOT NULL,
    budget_rag      TEXT    NOT NULL,
    headline        TEXT    NOT NULL,   -- the one-sentence, one-glance summary
    top_risk        TEXT,
    decision_needed TEXT,
    author          TEXT    NOT NULL
);

CREATE TABLE change_requests (
    cr_id           INTEGER PRIMARY KEY,
    title           TEXT    NOT NULL,
    requested_by    TEXT    NOT NULL,
    date_requested  DATE    NOT NULL,
    description     TEXT    NOT NULL,
    impact_scope    TEXT,
    impact_schedule TEXT,
    impact_cost     TEXT,
    decision        TEXT    NOT NULL DEFAULT 'pending',  -- 'pending' | 'approved' | 'rejected' | 'deferred'
    decided_by      TEXT,
    date_decided    DATE
);
```

On SQLite, turn on foreign-key enforcement in every session before inserting: `PRAGMA foreign_keys = ON;` — SQLite accepts the `REFERENCES` clause silently but won't enforce it otherwise.

Sanity check:

```sql
SELECT COUNT(*) FROM stakeholders;  -- 0 until Lecture 1 populates it
```

You'll insert the Atlas cast in Lecture 1, populate the communication plan in Lecture 2, log status reports in Lecture 2, and log a change request in Lecture 3.

## How to navigate this week

Work top to bottom. Each piece assumes the ones above it.

| # | File | What's inside | ~Time |
|--:|------|---------------|------:|
| 1 | [lecture-notes/01-stakeholder-mapping.md](./lecture-notes/01-stakeholder-mapping.md) | Power/interest grids, finding the sponsor and the silent blockers, tailored engagement per quadrant | 2h |
| 2 | [lecture-notes/02-communication-plans-and-status.md](./lecture-notes/02-communication-plans-and-status.md) | Audience–message–channel–cadence planning; honest RAG status that people trust | 2h |
| 3 | [lecture-notes/03-escalation-and-change.md](./lecture-notes/03-escalation-and-change.md) | Escalating with a clear ask, a lightweight change-request process, conflicting priorities | 2h |
| 4 | [exercises/exercise-01-build-a-power-interest-grid.md](./exercises/exercise-01-build-a-power-interest-grid.md) | Plot the full Atlas stakeholder cast on a power/interest grid | 1h |
| 5 | [exercises/exercise-02-draft-a-status-report.md](./exercises/exercise-02-draft-a-status-report.md) | Draft a one-page, honest RAG status report from real Atlas data | 1h |
| 6 | [exercises/exercise-03-write-an-escalation.md](./exercises/exercise-03-write-an-escalation.md) | Write a clean escalation with a specific ask for a blocked dependency | 1h |
| 7 | [challenges/challenge-01-align-conflicting-sponsors.md](./challenges/challenge-01-align-conflicting-sponsors.md) | Align two sponsors who each want the opposite scope decision | 1.5h |
| 8 | [challenges/challenge-02-communicate-a-bad-slip.md](./challenges/challenge-02-communicate-a-bad-slip.md) | Communicate a serious schedule slip without losing the room's trust | 1.5h |
| 9 | [mini-project/README.md](./mini-project/README.md) | Ship a stakeholder map, communication plan, and status pack for a project | 3h |
| 10 | [homework.md](./homework.md) | Extra practice, spread across the week | 4h |
| 11 | [quiz.md](./quiz.md) | 15 self-check questions + answer key | 1h |
| 12 | [resources.md](./resources.md) | Curated free/official reading + tools to install | — |

## Weekly schedule

The schedule below adds up to approximately **20.5 hours** for the full-time pace.

| Day | Focus | Lectures | Exercises | Challenges | Quiz/Read | Homework | Mini-Project | Daily Total |
|-----------|--------------------------------------------|---------:|----------:|-----------:|----------:|---------:|-------------:|------------:|
| Monday | Stakeholder mapping; populate the cast | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Tuesday | Communication plans; honest status | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Wednesday | Escalation, change requests, conflict | 2h | 1h | 0h | 0.5h | 1h | 0h | 5h |
| Thursday | Challenges: conflicting sponsors | 0h | 0h | 1.5h | 0.5h | 1h | 1h | 4h |
| Friday | Challenges: communicating a bad slip | 0h | 0h | 1.5h | 0.5h | 0h | 1.5h | 3.5h |
| Saturday | Mini-project — map, plan, status pack | 0h | 0h | 0h | 0h | 0h | 2.5h | 2.5h |
| Sunday | Quiz + review | 0h | 0h | 0h | 1h | 0h | 0h | 1h |
| **Total** | | **6h** | **3h** | **3h** | **3.5h** | **4h** | **5h** | **~25h** |

## By the end of this week you can…

- Look at a project's cast of names and correctly place each one on a power/interest grid, including the quiet ones whose power outweighs their visible interest.
- Build a communication plan where every audience gets a message shaped for them, on a channel and cadence that fits their attention — not one email blasted to everyone.
- Write a status report a skeptical sponsor reads in under a minute and believes, because it never hides the one thing that's actually going wrong.
- Escalate a real blocker with a fact, an impact, and a specific ask — the kind of escalation that gets a decision instead of a shrug.
- Run a change request through a lightweight, explicit process instead of letting it quietly become scope.
- Sit two stakeholders who want opposite things across the table (real or written) and get to a decision without dodging the conflict or picking a side by default.

## Up next

Week 8 — Delivery Metrics & Flow — once the right people trust what you tell them, the next question is whether what you're telling them is actually true: velocity, cycle time, and burndown, computed from real data instead of gut feel.

---

*Part of the Code Crunch Worldwide open curriculum · GPL-3.0 · If you find errors, please open an issue or PR.*
