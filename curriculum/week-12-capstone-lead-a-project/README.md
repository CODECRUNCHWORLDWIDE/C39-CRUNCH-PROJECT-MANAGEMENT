# Week 12 — Capstone — Lead a Project

> **Goal:** by the end of this week you will have taken a real, scoped technology project from a blank board to a shipped, measured delivery — chartered it, planned it, run it, tracked its risks and stakeholders, forecast its ship date from your own flow data in SQL and pandas, released it through a gate with a rollback plan, and closed it with a data-backed report and a retrospective. No case study. No fictional sponsor. Yours.

Welcome to the last week of **C39 · Crunch Project Management**. Eleven weeks built one skill at a time against **Project Atlas**, the running Northlight Software case study: a charter (Week 1), Agile ceremonies (Week 2), a sliced backlog (Week 3), estimation and sprint planning (Week 4), a roadmap (Week 5), a risk register and dependency map (Week 6), a stakeholder and communication plan (Week 7), a flow dashboard in SQL and pandas (Week 8), a cost and capacity model in PostgreSQL (Week 9), a release gate with a rollback plan (Week 10), and a tooled, AI-assisted board (Week 11). Atlas was a rehearsal space — realistic enough to teach the moves, safe enough that a wrong call cost you nothing but a re-read.

This week there's no script. You pick a real, deliberately small project, run the entire lifecycle against it in the next several days, and ship something real. Every lecture in this course threads one worked example throughout — this week's is **Jordan Vance**, a solo learner building **TaskPing**, a tiny command-line due-date reminder tool, chartered, run, forecast, and shipped start to finish. Jordan's TaskPing isn't a case study you read about after the fact — it's shown *while it's happening*, decision by decision, so you can see exactly what "apply the whole course" looks like in miniature before you do it yourself on your own project. Where Atlas showed you the moves at enterprise scale with a cast of eight stakeholders and a quarter of runway, Jordan shows you the same moves compressed into a project one person can actually finish this week.

**Following this course's data-tooling rule one last time:** your capstone's backlog items, their status history, your risk register, and your stakeholder map are relational, queryable records you'll re-run and re-query as the week progresses — not a slide deck, not a spreadsheet. They live in a `capstone_pm` database (PostgreSQL primary, SQLite fallback), queried with SQL and forecast with pandas, exactly like Atlas's `atlas_pm` in Weeks 6–9. The one genuinely new technique this week — forecasting your ship date from your own historical throughput using Monte Carlo simulation — is taught from scratch in Lecture 2 and is itself a SQL-plus-pandas pipeline, never a spreadsheet macro.

## Learning objectives

By the end of this week, you will be able to:

- **Charter, scope, and plan** a real technology project from initiation — small enough to actually ship this week, real enough to be worth chartering properly.
- **Build and prioritize a backlog** and run delivery in GitHub Projects or Jira, using the sprint or Kanban conventions from Weeks 2–4.
- **Manage risk, dependencies, and stakeholders** through the delivery, keeping a live risk register and a right-sized stakeholder map instead of a one-time document.
- **Forecast the ship date** from your own flow data using a SQL throughput query and a Monte Carlo simulation in pandas — a technique this week teaches from first principles.
- **Run a controlled release** with an explicit definition of done and a tested rollback plan, not a hopeful `git push` to `main`.
- **Report outcomes with data** — a final delivery report built from your charter's success criteria and your own SQL/pandas flow metrics — and run a closing retrospective that tells the honest story of what shipped and what you learned.

## Prerequisites

- **Weeks 1–11 of this course**, all of them. This week doesn't teach new PM theory — it's the week every prior week gets used at once. If any earlier week feels shaky, this is the week that will find the gap; skim back to that week's `README.md` rather than push through confused.
- A free **[GitHub](https://github.com/join)** account with **GitHub Projects** enabled, or a **[Jira](https://www.atlassian.com/software/jira/free)** free-tier workspace — either is fine; the exercises give steps for both.
- **PostgreSQL 16+** (primary) or **SQLite 3.35+** (fallback), and **Python 3.10+ with pandas and numpy** (`pip install pandas numpy`) — the same stack Weeks 8 and 9 used. If you don't have it set up, `resources.md` has from-scratch install steps.
- `git` installed and a GitHub (or equivalent) repo you can push to — your release and rollback plan in Lecture 3 are git-based.
- A little slack in your calendar. This week's mini-project assumes you can put in real, uninterrupted focus time on your own project across several days — read the weekly schedule below and block it out before Monday.

## Set up this week's data (do this first)

Everything this week runs against your own project's data, but it needs somewhere to live before you have any. Create `capstone_pm` now — you'll fill it in as you go through Lecture 1's board setup and Exercise 1's charter.

**PostgreSQL:**

```bash
createdb capstone_pm
psql capstone_pm
```

**SQLite:**

```bash
sqlite3 capstone_pm.db
```

Then run this schema — it mirrors the `atlas_pm` shapes from Weeks 6–8, scaled down to what one person tracking one small project actually needs:

```sql
CREATE TABLE capstone_items (
    item_id        INTEGER PRIMARY KEY,
    item_key       TEXT    NOT NULL UNIQUE,   -- 'TP-01'
    title          TEXT    NOT NULL,
    item_type      TEXT    NOT NULL,          -- 'feature' | 'bug' | 'chore'
    priority       TEXT    NOT NULL,          -- 'must' | 'should' | 'could'  (MoSCoW, Week 3)
    created_at     DATE    NOT NULL,
    started_at     DATE,                      -- NULL until work begins
    done_at        DATE,                      -- NULL until Done
    current_status TEXT    NOT NULL           -- 'Backlog'|'Ready'|'In Progress'|'Blocked'|'In Review'|'Done'
);

CREATE TABLE capstone_status_history (
    history_id  INTEGER PRIMARY KEY,
    item_id     INTEGER NOT NULL REFERENCES capstone_items(item_id),
    status      TEXT    NOT NULL,
    entered_at  DATE    NOT NULL,
    left_at     DATE                          -- NULL = still in this status right now
);

CREATE TABLE capstone_risks (
    risk_id     INTEGER PRIMARY KEY,
    description TEXT    NOT NULL,
    category    TEXT    NOT NULL,             -- 'Risk'|'Assumption'|'Issue'|'Dependency' (RAID, Week 6)
    probability INTEGER NOT NULL,             -- 1-5
    impact      INTEGER NOT NULL,             -- 1-5
    owner       TEXT    NOT NULL,
    status      TEXT    NOT NULL,             -- 'open'|'watching'|'closed'|'materialized'
    response    TEXT    NOT NULL,             -- avoid|mitigate|transfer|accept (Week 6)
    logged_at   DATE    NOT NULL
);

CREATE TABLE capstone_stakeholders (
    stakeholder_id INTEGER PRIMARY KEY,
    name           TEXT    NOT NULL,
    role           TEXT    NOT NULL,
    power          INTEGER NOT NULL,          -- 1-5 (Week 7 power/interest grid)
    interest       INTEGER NOT NULL,          -- 1-5
    notes          TEXT
);
```

On SQLite, run `PRAGMA foreign_keys = ON;` in every session — SQLite parses `REFERENCES` but won't enforce it otherwise.

Leave the tables empty for now. Lecture 1 walks you through populating `capstone_items` as you build your backlog, and by Exercise 2 you'll have several real days of status history to forecast from.

## How to navigate this week

Work top to bottom. Each piece assumes the ones above it — this is the one week where skipping ahead will cost you real project time, not just quiz points.

| # | File | What's inside | ~Time |
|--:|------|---------------|------:|
| 1 | [lecture-notes/01-capstone-kickoff.md](./lecture-notes/01-capstone-kickoff.md) | Choosing a scoped project, writing its charter, standing up the board, setting success criteria and a delivery approach | 2.5h |
| 2 | [lecture-notes/02-running-the-delivery.md](./lecture-notes/02-running-the-delivery.md) | Executing sprints or flow, keeping risk/stakeholder artifacts live, and forecasting your ship date with Monte Carlo simulation | 2.5h |
| 3 | [lecture-notes/03-closing-and-reporting.md](./lecture-notes/03-closing-and-reporting.md) | The release gate and rollback plan, the closing retrospective, and the final data-backed delivery report | 2h |
| 4 | [exercises/exercise-01-capstone-charter.md](./exercises/exercise-01-capstone-charter.md) | Charter and plan your capstone project; stand up the board | 1.5h |
| 5 | [exercises/exercise-02-capstone-forecast.md](./exercises/exercise-02-capstone-forecast.md) | Forecast your capstone ship date from real flow data in SQL + pandas | 1.5h |
| 6 | [exercises/exercise-03-capstone-release-gate.md](./exercises/exercise-03-capstone-release-gate.md) | Define your release gate, definition of done, and rollback plan | 1h |
| 7 | [challenges/challenge-01-capstone-recover-a-slip.md](./challenges/challenge-01-capstone-recover-a-slip.md) | Recover your capstone from a mid-delivery slip, using your own forecast | 1.5h |
| 8 | [challenges/challenge-02-capstone-delivery-report.md](./challenges/challenge-02-capstone-delivery-report.md) | Write a data-backed final delivery report | 1.5h |
| 9 | [mini-project/README.md](./mini-project/README.md) | The capstone itself: blank board to shipped, measured delivery | 8h+ |
| 10 | [homework.md](./homework.md) | Integrative practice tying the whole course together | 3h |
| 11 | [quiz.md](./quiz.md) | 15 self-check questions + answer key | 1h |
| 12 | [resources.md](./resources.md) | Curated free/official reading + tools to install | — |

## Weekly schedule

This week's total runs well above a normal course week on purpose — it's the week you actually build and ship something, not just read about building and shipping. The schedule below assumes a genuinely small capstone scope (see Lecture 1, §2); if your project needs more runway, stretch this schedule across two weeks rather than cutting scope corners under time pressure.

| Day | Focus | Lectures | Exercises | Challenges | Quiz/Read | Homework | Mini-Project | Daily Total |
|-----------|--------------------------------------------|---------:|----------:|-----------:|----------:|---------:|-------------:|------------:|
| Monday | Choose + scope the project; write the charter; stand up the board | 2.5h | 1.5h | 0h | 0.5h | 0h | 1h | 5.5h |
| Tuesday | Start execution; keep risk/stakeholder artifacts live | 0h | 0h | 0h | 0h | 0h | 4h | 4h |
| Wednesday | Continue execution; log flow data daily | 0h | 0h | 0h | 0h | 0h | 4h | 4h |
| Thursday | Forecast the ship date; adjust the plan if needed | 2.5h | 1.5h | 1.5h | 0.5h | 1h | 2h | 9h |
| Friday | Finish execution; define the release gate + rollback | 2h | 1h | 0h | 0.5h | 1h | 3h | 7.5h |
| Saturday | Ship it; run the retrospective; write the delivery report | 0h | 0h | 1.5h | 0h | 1h | 4h | 6.5h |
| Sunday | Quiz + review + course wrap-up | 0h | 0h | 0h | 1h | 0h | 0h | 1h |
| **Total** | | **7h** | **4h** | **3h** | **2.5h** | **3h** | **18h** | **~37.5h** |

## By the end of this week you can…

- Take a project from a blank board to a chartered, scoped, planned delivery — on your own, without a template to fill in.
- Run real delivery execution, keeping a risk register and stakeholder map alive instead of writing them once and forgetting them.
- Query your own project's status history in SQL, and forecast a ship date with a Monte Carlo simulation in pandas — with a stated confidence level, not a guess.
- Gate a release on an explicit definition of done, and have a rollback plan you've actually tested, not just written down.
- Close a project honestly: a retrospective that says what actually happened, and a delivery report backed by the same data you forecast from.

## Course complete

This is the last week of **C39 · Crunch Project Management**. If you've shipped your capstone, run its retrospective, and passed this week's quiz, you've completed the course — from "what is a project" in Week 1 to leading one, measuring it, and closing it honestly in Week 12. If you're a Code Crunch One-Stop member, your certificate is issued on 100% completion; see the [course README](../../README.md) for how completion is tracked.

---

*Part of the Code Crunch Worldwide open curriculum · GPL-3.0 · If you find errors, please open an issue or PR.*
