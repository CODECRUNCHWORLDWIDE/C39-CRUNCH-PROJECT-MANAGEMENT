# Week 10 — Quality & Release Management

> **Goal:** by Sunday you can write a definition of done that actually blocks bad work from shipping, build a release readiness checklist and run a real go/no-go decision — including one with open defects on the table — design a rollback plan before you ever need it, and run a blameless post-release review that turns what went wrong into backlog items instead of blame.

Welcome back to **C39 · Crunch Project Management**. Weeks 1–9 built everything a technology project needs to be planned, run, and measured on purpose: a charter, a backlog, sprints, a roadmap, a risk register, a stakeholder map, a flow dashboard, a budget. All of that converges on one moment every project eventually reaches — the day the work has to leave the team's laptops and become real for a customer. That moment is where most of the trust you've built over nine weeks is won or lost in an afternoon. Good delivery work quietly rots into a bad release when nobody wrote down what "done" means, nobody checked the release was actually ready, nobody planned for the release going wrong, and nobody looked back afterward to ask why it did. This week is about doing all four on purpose.

We close out the **Project Atlas** case study from Northlight Software. Atlas has just finished **Sprint 8** — the final sprint of the original eight-sprint plan (Week 1) — and Team Workspaces is, by every engineer's account, "basically done." **Priya Chen** (VP Product, sponsor) has a GA date circled: **Wednesday, April 29, 2026**, timed so **Jamie Okafor** (Customer Success Lead) can walk into the QBRs with the three at-risk enterprise accounts the following week and show them a shipped feature, not a promise. "Basically done" is not the same thing as *done*, and the gap between those two phrases is the entire subject of this week. Two new people join the cast this week because this is the first time their jobs matter to the story: **Yuki Tanaka** (QA Lead, owns the test and quality sign-off) and **Omar Farouk** (Site Reliability Engineer, owns the cutover, the monitoring, and — if it comes to it — pulling the rollback lever).

**Following this course's data-tooling rule:** a definition of done, a release checklist, a defect list, a rollback runbook, and a sign-off record are *data* — rows with owners, statuses, and timestamps you query constantly under real time pressure ("what's still blocking," "who hasn't signed off," "what do we do if the error rate spikes at 2pm"). A go/no-go meeting run off a printed checklist nobody updates is exactly the kind of "spreadsheet-as-database" this course rejects — by the time it's printed, it's already stale. Every artifact this week extends the `atlas_pm` database from Weeks 6–9 with five new tables (`dod_criteria`, `known_defects`, `release_checklist`, `rollback_steps`, `release_signoffs`, plus `release_events`/`post_release_actions` for the post-release review), queried live in SQL, with any heavier analysis in pandas. No spreadsheet touches this week's work.

## Learning objectives

By the end of this week, you will be able to:

- **Write** a definition of done that spans code, tests, docs, and sign-off — one specific enough to actually block a release, not a vague aspiration nobody checks.
- **Distinguish** a quality gate that adds real signal from one that's become pure ceremony, and place quality responsibility correctly between the PM, the delivery team, and QA.
- **Build** a release readiness checklist separate from (and layered on top of) the definition of done, covering environment, data, monitoring, comms, and support readiness.
- **Compare** staged rollouts (canary, percentage-based, feature-flagged) against big-bang releases, and choose correctly for a given risk profile.
- **Run** a go/no-go decision with a clear decision matrix, named owners, and a documented outcome — including the harder case where open defects are still on the table.
- **Design** a rollback/backout plan before a release, including trigger conditions, concrete steps, an owner with the authority to pull the trigger, and a post-rollback verification step.
- **Coordinate** a release communications plan across engineering, support, and customers, using the stakeholder and comms discipline from Week 7.
- **Run** a blameless post-release review that reconstructs a timeline, asks "why" without assigning fault, and produces real, owned backlog action items.
- **Query and analyze**, in SQL and pandas, a release's defect list, checklist status, and incident timeline — never in a spreadsheet.

## Prerequisites

- Weeks 1–9 of this course, especially Week 1 (the charter's security-review constraint, referenced again this week), Week 6 (the risk register — the flaky third-party sandbox and the Platform-team dependency are back), Week 7 (stakeholder mapping and communication planning, reused for release comms), and Week 8 (the `atlas_pm` issue-tracking schema this week's defect list sits alongside).
- **PostgreSQL 16+** (primary) or **SQLite 3.35+** (fallback) — the same `atlas_pm` database from prior weeks. If you don't have it yet, see [`resources.md`](./resources.md) for a from-scratch setup; this week's tables don't require every prior week's tables to exist.
- Comfortable `SELECT` / `WHERE` / `JOIN` / `GROUP BY` SQL (Weeks 1–9 have used all of these). No new SQL syntax is required this week beyond what you already know, though the pandas lecture section uses `pd.read_sql` and basic time-delta arithmetic.
- **Python 3.10+ with pandas** installed (`pip install pandas`), from Week 8/9.

## Set up this week's data (do this first)

Everything this week extends `atlas_pm` with new tables. If you don't have the database yet, create it first (`createdb atlas_pm` / `sqlite3 atlas_pm.db`), then run this either way:

```sql
-- Every table below carries release_name so one atlas_pm database can hold
-- multiple releases' worth of gates side by side (Atlas's real GA, plus the
-- practice releases you'll add in the exercises) without them colliding.

CREATE TABLE dod_criteria (
    dod_id       INTEGER PRIMARY KEY,
    release_name TEXT    NOT NULL,
    category     TEXT    NOT NULL,   -- 'code' | 'tests' | 'docs' | 'sign-off'
    criterion    TEXT    NOT NULL,
    required     BOOLEAN NOT NULL DEFAULT TRUE,
    owner        TEXT    NOT NULL,
    status       TEXT    NOT NULL DEFAULT 'not_met',  -- 'met' | 'not_met' | 'waived' | 'na'
    evidence     TEXT,
    checked_at   DATE
);

CREATE TABLE known_defects (
    defect_id       INTEGER PRIMARY KEY,
    release_name    TEXT    NOT NULL,
    issue_key       TEXT    NOT NULL UNIQUE,   -- e.g. 'ATLAS-131'
    summary         TEXT    NOT NULL,
    severity        TEXT    NOT NULL,   -- 'blocker' | 'critical' | 'major' | 'minor'
    status          TEXT    NOT NULL,   -- 'open' | 'fixed' | 'deferred' | 'wont_fix'
    discovered_on   DATE    NOT NULL,
    assignee        TEXT    NOT NULL,
    workaround      TEXT,
    customer_facing BOOLEAN NOT NULL DEFAULT TRUE
);

CREATE TABLE release_checklist (
    check_id     INTEGER PRIMARY KEY,
    release_name TEXT    NOT NULL,
    category     TEXT    NOT NULL,   -- 'code freeze' | 'environment' | 'data' | 'monitoring' | 'comms' | 'support' | 'rollback'
    item         TEXT    NOT NULL,
    owner        TEXT    NOT NULL,
    status       TEXT    NOT NULL DEFAULT 'not_started',  -- 'not_started' | 'in_progress' | 'done' | 'blocked'
    due_date     DATE,
    notes        TEXT
);

CREATE TABLE rollback_steps (
    step_id           INTEGER PRIMARY KEY,
    release_name      TEXT    NOT NULL,
    step_no           INTEGER NOT NULL,
    trigger_condition TEXT    NOT NULL,
    action            TEXT    NOT NULL,
    owner             TEXT    NOT NULL,
    est_minutes       INTEGER NOT NULL,
    verification      TEXT    NOT NULL
);

CREATE TABLE release_signoffs (
    signoff_id      INTEGER PRIMARY KEY,
    release_name    TEXT    NOT NULL,
    role            TEXT    NOT NULL,
    name            TEXT    NOT NULL,
    decision        TEXT    NOT NULL DEFAULT 'pending',  -- 'go' | 'no_go' | 'conditional_go' | 'pending'
    condition_notes TEXT,
    decided_at      TIMESTAMP
);

-- release_events and post_release_actions already carried release_name from
-- the start — used for the real GA cutover (Lecture 3) AND the Challenge 2
-- postmortem scenario, distinguished by release_name.
CREATE TABLE release_events (
    event_id     INTEGER PRIMARY KEY,
    release_name TEXT      NOT NULL,
    event_time   TIMESTAMP NOT NULL,
    event_type   TEXT      NOT NULL,  -- 'deploy' | 'alert' | 'customer_report' | 'investigation' | 'mitigation' | 'rollback' | 'resolved'
    description  TEXT      NOT NULL,
    actor        TEXT      NOT NULL
);

CREATE TABLE post_release_actions (
    action_id    INTEGER PRIMARY KEY,
    release_name TEXT NOT NULL,
    action_item  TEXT NOT NULL,
    owner        TEXT NOT NULL,
    due_date     DATE,
    status       TEXT NOT NULL DEFAULT 'open'  -- 'open' | 'in_progress' | 'done'
);
```

On SQLite, run `PRAGMA foreign_keys = ON;` in every session if you add your own `REFERENCES` clauses back to Week 8's `issues` table — this week's tables are deliberately self-contained (plain `issue_key` text) so they work whether or not your Week 8/9 tables survived.

Sanity check:

```sql
SELECT COUNT(*) FROM dod_criteria;   -- 0 until Lecture 1 populates it
```

You'll populate `dod_criteria` in Lecture 1, `known_defects` and `release_checklist` in Lecture 2, and `rollback_steps` / `release_signoffs` / `release_events` in Lecture 3.

## How to navigate this week

Work top to bottom. Each piece assumes the ones above it.

| # | File | What's inside | ~Time |
|--:|------|---------------|------:|
| 1 | [lecture-notes/01-definition-of-done-and-gates.md](./lecture-notes/01-definition-of-done-and-gates.md) | A DoD that actually gates work; quality checks that add signal not ceremony; who owns quality between the PM, team, and QA | 2h |
| 2 | [lecture-notes/02-release-management.md](./lecture-notes/02-release-management.md) | Release readiness criteria, the known-defect list, staged vs. big-bang releases, the go/no-go decision, and a communications plan | 2h |
| 3 | [lecture-notes/03-rollback-and-postmortem.md](./lecture-notes/03-rollback-and-postmortem.md) | Designing a backout plan before you need it, running the release, and a blameless post-release review | 2h |
| 4 | [exercises/exercise-01-write-a-dod.md](./exercises/exercise-01-write-a-dod.md) | Write and load a real definition of done for Atlas's GA | 1h |
| 5 | [exercises/exercise-02-build-a-release-checklist.md](./exercises/exercise-02-build-a-release-checklist.md) | Build and query a release readiness checklist | 1h |
| 6 | [exercises/exercise-03-design-a-rollback.md](./exercises/exercise-03-design-a-rollback.md) | Design a full rollback plan with trigger conditions and owners | 1h |
| 7 | [challenges/challenge-01-gate-a-risky-release.md](./challenges/challenge-01-gate-a-risky-release.md) | Run a real go/no-go on a release with open defects | 1.5h |
| 8 | [challenges/challenge-02-postmortem-a-bad-launch.md](./challenges/challenge-02-postmortem-a-bad-launch.md) | Run a blameless post-release review on a launch that actually failed | 1.5h |
| 9 | [mini-project/README.md](./mini-project/README.md) | Ship a full release: DoD gate, readiness checklist, rollback plan, go/no-go decision, and a post-release review template | 3h |
| 10 | [homework.md](./homework.md) | Extra practice, spread across the week | 4h |
| 11 | [quiz.md](./quiz.md) | 15 self-check questions + answer key | 1h |
| 12 | [resources.md](./resources.md) | Curated free/official reading + tools to install | — |

## Weekly schedule

The schedule below adds up to approximately **21.5 hours** for the full-time pace.

| Day | Focus | Lectures | Exercises | Challenges | Quiz/Read | Homework | Mini-Project | Daily Total |
|-----------|--------------------------------------------|---------:|----------:|-----------:|----------:|---------:|-------------:|------------:|
| Monday | Definition of done & quality gates | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Tuesday | Release management & cutover | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Wednesday | Rollback plans & post-release review | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Thursday | Challenge 1 — gate a risky release | 0h | 0h | 1.5h | 0.5h | 1h | 0.5h | 3.5h |
| Friday | Challenge 2 — postmortem a bad launch | 0h | 0h | 1.5h | 0.5h | 0h | 1h | 3h |
| Saturday | Mini-project — run the release | 0h | 0h | 0h | 0h | 0h | 2.5h | 2.5h |
| Sunday | Quiz + review | 0h | 0h | 0h | 1h | 0h | 0h | 1h |
| **Total** | | **6h** | **3h** | **3h** | **3.5h** | **4h** | **4h** | **~23.5h** |

## By the end of this week you can…

- Write a definition of done specific enough that a reviewer can check each line true/false — no line that only a vibe can evaluate.
- Tell the difference between a quality gate that catches real defects and one that's become theater, and explain what to cut from a gate that's become the latter.
- Build a release readiness checklist that covers more than "the code works" — environment, data, monitoring, comms, and support are all first-class citizens.
- Run an actual go/no-go meeting, including the uncomfortable version where real defects are still open, and land on a documented, owned decision instead of a shrug.
- Write a rollback plan before the release that a stressed, tired engineer at 2am could follow without you in the room.
- Run a post-release review that stays blameless under real pressure and turns every finding into a specific, owned, dated backlog item.

## Up next

Week 11 — Tooling & AI-Assisted PM — once you can gate, ship, and learn from a real release, the next question is which tools do that job for you at scale: GitHub Projects, Jira, and AI-assisted workflows for backlog drafting and status reporting.

---

*Part of the Code Crunch Worldwide open curriculum · GPL-3.0 · If you find errors, please open an issue or PR.*
