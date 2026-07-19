# Week 11 — Tooling & AI-Assisted PM

> **Goal:** by Sunday you can configure a GitHub Projects board end to end — views, custom fields, iterations, automations — query a Jira project fluently in JQL, export both into PostgreSQL/SQLite tables you can actually analyze, and use AI to draft a backlog and narrate a status digest that is *grounded* in that data instead of making it up.

Welcome back to **C39 · Crunch Project Management**. For ten weeks you've built the artifacts of running a project — charter, backlog, sprints, roadmap, risk register, stakeholder map, flow dashboard, budget, release gate — mostly with pen-and-paper concepts you could, in principle, track on a whiteboard. This week is different: it's about the two pieces of software that actually hold those artifacts in most technology companies, **GitHub Projects** and **Jira**, and about the one new tool every PM now has sitting on their desk — an AI assistant that can draft, summarize, and (if you let it) confidently lie to you.

We continue the **Project Atlas** case study from Northlight Software. It's **Monday, 2026-04-13**, day 1 of Sprint 8. Sprint 7 closed clean — 8 of the sprint's items shipped — but two threads are now impossible to ignore. First, **Marcus Diallo** (Tech Lead) has been running Atlas's engineering work on a GitHub Projects board he half-configured six weeks ago: no automations, a "Priority" field nobody fills in consistently, and a status column that three different engineers use three different ways. **Priya Chen** (VP Product) wants a trustworthy weekly status digest out of it and is tired of Marcus hand-typing one every Friday night. Second, **Sofia Reyes**'s Platform team — the team that owns the webhook dependency that has bitten Atlas twice already (Weeks 6 and 8) — runs on a legacy **Jira** project, `PLAT`, that predates the company's move to GitHub. Northlight's engineering leadership has now decided every team standardizes on GitHub Projects by end of quarter, which means Marcus needs to (1) get Atlas's own board production-grade *now*, (2) get fluent enough in Jira and JQL to read PLAT's backlog *this week*, because Atlas's SLA dashboard work is blocked on it, and (3) figure out where an AI assistant genuinely saves him time on this — and where it would have quietly handed Priya a fabricated number.

**Following this course's data-tooling rule:** a project board export — GitHub Projects items with custom fields, or a Jira JQL result set — is relational, queryable data with the same shape as the issue exports you loaded in Week 8. It belongs in PostgreSQL or SQLite, queried with SQL and analyzed with pandas, not pasted into a spreadsheet and eyeballed. That discipline matters even more this week, because it's also what keeps the AI honest: the grounded-status pattern in Lecture 3 works by computing every number in SQL *first*, then handing the AI only those computed numbers to narrate — never the raw board, and never a blank page.

## Learning objectives

By the end of this week, you will be able to:

- **Configure** a GitHub Projects (v2) board's views, custom fields (single-select, number, iteration, date), and grouping/sorting/filtering so the board answers questions at a glance instead of requiring a human to scroll it.
- **Wire up** built-in GitHub Projects automations (auto-add, auto-close, item-added/status-changed workflows) so the board updates itself as issues and PRs move through their lifecycle.
- **Configure** a Jira board's workflow (statuses and the transitions between them), and distinguish a Scrum board from a Kanban board in Jira's terms.
- **Write** JQL (Jira Query Language) queries that filter by status, priority, assignee, reporter, sprint, and issue type — the JQL equivalents of the `WHERE` clauses you've written all course.
- **Map** Jira's concepts (Epic, Story, Sprint, Board, Workflow, Story Points field) onto GitHub Projects' concepts (Issue with sub-issues, Iteration field, View, Status field, Custom field) so moving between the two tools is a translation exercise, not a relearn.
- **Export** data cleanly from both tools — via `gh` CLI / GraphQL for GitHub, via JQL + REST/CSV for Jira — into SQL tables you can query and pandas DataFrames you can analyze, without a spreadsheet in the loop.
- **Automate** routine board maintenance — auto-labeling, auto-status transitions, stale-item detection, roll-up counts — instead of doing it by hand every sprint.
- **Use** AI to draft backlog stories from a feature brief, slice epics, summarize status, and surface risk *from data you retrieved first* — with a prompt pattern that grounds every claim in a real query result.
- **Judge**, with specific examples, where AI helps a PM (drafting, summarizing, pattern-spotting across a big export) and where it fabricates or misleads (dates, counts, and "vibes" it has no data for) — and build a habit of checking before you forward anything AI wrote to a stakeholder.

## Prerequisites

- Weeks 1–8 of this course, especially Week 3 (backlog & user stories — you'll reuse INVEST this week) and Week 8 (flow metrics — the SQL/pandas muscle this week extends).
- **PostgreSQL 16+** (primary) or **SQLite 3.35+** (fallback). This week's seed is self-contained — you don't need any table from a previous week, though the narrative continues Atlas's `atlas_pm` database.
- **Python 3.10+ with pandas** (`pip install pandas`) for the analysis sections in Lectures 1 and 3.
- A **free GitHub account** and a **free Jira Cloud trial or sandbox site** (both have no-cost tiers sufficient for this week — see [`resources.md`](./resources.md)). You do not need to pay for anything this week.
- **`gh`, the GitHub CLI**, installed and authenticated (`gh auth login`) — used to script the GitHub Projects export in Lecture 1 and Exercise 1.
- Access to an AI assistant (Claude, ChatGPT, or similar) for Lecture 3 and the AI exercises/challenges. Any capable general-purpose assistant works; the lecture teaches prompt patterns, not a specific product.

## Set up this week's data (do this first)

This week's case study is two exports: Atlas's own **GitHub Projects** board, and the Platform team's **Jira** project `PLAT`. Both live in the `atlas_pm` database as two new, self-contained tables — create (or reuse) `atlas_pm`, then run this.

```sql
CREATE TABLE board_items (
    item_id       INTEGER PRIMARY KEY,
    repo          TEXT    NOT NULL,       -- 'northlight/atlas'
    issue_number  INTEGER,                -- NULL for draft items with no linked issue yet
    item_type     TEXT    NOT NULL,       -- 'Issue' | 'PullRequest' | 'DraftIssue'
    title         TEXT    NOT NULL,
    status        TEXT    NOT NULL,       -- custom field: 'Todo'|'In Progress'|'In Review'|'Blocked'|'Done'
    priority      TEXT    NOT NULL,       -- custom field: 'P0'|'P1'|'P2'|'P3'
    size          TEXT,                   -- custom field: 'XS'|'S'|'M'|'L'|'XL'; NULL if not sized yet
    iteration     TEXT    NOT NULL,       -- custom field: 'Sprint 7'|'Sprint 8'
    assignee      TEXT,                   -- NULL if unassigned
    labels        TEXT,                   -- comma-separated, as GitHub's API returns them
    created_at    DATE    NOT NULL,
    closed_at     DATE,                   -- NULL if still open
    updated_at    DATE    NOT NULL
);

INSERT INTO board_items VALUES
(1, 'northlight/atlas', 41, 'Issue', 'Provider webhook signature verification', 'Done', 'P1', 'M', 'Sprint 7', 'Marcus Diallo', 'backend,webhook', '2026-03-25', '2026-04-09', '2026-04-09'),
(2, 'northlight/atlas', 42, 'Issue', 'Spectator mode pagination', 'Done', 'P2', 'S', 'Sprint 7', 'Nadia Petrova', 'frontend', '2026-03-25', '2026-04-05', '2026-04-05'),
(3, 'northlight/atlas', 43, 'Issue', 'Race condition in presence indicator', 'Done', 'P1', 'XS', 'Sprint 7', 'Fatima Noor', 'bug,frontend', '2026-04-01', '2026-04-08', '2026-04-08'),
(4, 'northlight/atlas', 44, 'Issue', 'Platform-team SLA dashboard', 'Blocked', 'P1', 'L', 'Sprint 8', 'Wei Zhang', 'backend,platform-dependency', '2026-03-25', NULL, '2026-04-20'),
(5, 'northlight/atlas', 45, 'Issue', 'Runbook for sandbox outages', 'Done', 'P3', 'XS', 'Sprint 7', 'Chris Okoye', 'ops,docs', '2026-03-28', '2026-04-09', '2026-04-09'),
(6, 'northlight/atlas', 46, 'Issue', 'Auto-retry policy for the provider client', 'In Review', 'P2', 'M', 'Sprint 8', 'Marcus Diallo', 'backend', '2026-04-02', NULL, '2026-04-19'),
(7, 'northlight/atlas', 47, 'Issue', 'Draft an AI-assisted release-notes generator', 'Todo', 'P2', 'M', 'Sprint 8', 'Fatima Noor', 'tooling,ai', '2026-04-13', NULL, '2026-04-14'),
(8, 'northlight/atlas', 48, 'Issue', 'Backfill Size field on legacy issues', 'Done', 'P3', 'XS', 'Sprint 8', 'Marcus Diallo', 'tooling,automation', '2026-04-13', '2026-04-14', '2026-04-14'),
(9, 'northlight/atlas', 49, 'Issue', 'Scope the Jira-to-GitHub Projects migration for PLAT', 'In Progress', 'P1', 'L', 'Sprint 8', 'Marcus Diallo', 'tooling,migration', '2026-04-13', NULL, '2026-04-20'),
(10, 'northlight/atlas', 50, 'Issue', 'Sandbox rate-limit backoff tuning', 'In Progress', 'P1', 'S', 'Sprint 8', 'Nadia Petrova', 'backend', '2026-04-14', NULL, '2026-04-19'),
(11, 'northlight/atlas', 51, 'Issue', 'Auto-add workflow: label triage into board', 'Done', 'P3', 'XS', 'Sprint 8', 'Marcus Diallo', 'tooling,automation', '2026-04-13', '2026-04-13', '2026-04-13'),
(12, 'northlight/atlas', 52, 'Issue', 'Flaky sandbox integration test keeps failing intermittently', 'Blocked', 'P0', 'M', 'Sprint 8', 'Chris Okoye', 'bug,ci', '2026-04-15', NULL, '2026-04-20'),
(13, 'northlight/atlas', 53, 'Issue', 'Backfill Iteration field via GraphQL script', 'Done', 'P3', 'S', 'Sprint 8', 'Wei Zhang', 'tooling,automation', '2026-04-13', '2026-04-15', '2026-04-15'),
(14, 'northlight/atlas', 54, 'Issue', 'Presence indicator regression test suite', 'In Review', 'P2', 'M', 'Sprint 8', 'Fatima Noor', 'testing,frontend', '2026-04-14', NULL, '2026-04-19'),
(15, 'northlight/atlas', 210, 'PullRequest', 'Fix webhook signature clock-skew tolerance', 'Done', 'P0', 'S', 'Sprint 8', 'Marcus Diallo', 'backend,webhook', '2026-04-16', '2026-04-18', '2026-04-18'),
(16, 'northlight/atlas', NULL, 'DraftIssue', 'Explore an AI backlog-grooming assistant for triage', 'Todo', 'P2', NULL, 'Sprint 8', NULL, 'ai,idea', '2026-04-17', NULL, '2026-04-17');

CREATE TABLE jira_issues (
    issue_key     TEXT PRIMARY KEY,       -- 'PLAT-501'
    project_key   TEXT    NOT NULL,       -- 'PLAT'
    issue_type    TEXT    NOT NULL,       -- 'Story'|'Bug'|'Task'
    summary       TEXT    NOT NULL,
    status        TEXT    NOT NULL,       -- Jira workflow status
    priority      TEXT    NOT NULL,       -- 'Highest'|'High'|'Medium'|'Low'
    story_points  NUMERIC,                -- NULL for most bugs, same convention as Week 8
    sprint        TEXT    NOT NULL,       -- 'PLAT Sprint 14'|'PLAT Sprint 15'
    assignee      TEXT    NOT NULL,
    reporter      TEXT    NOT NULL,
    created       DATE    NOT NULL,
    resolved      DATE,                   -- NULL if not yet Done
    labels        TEXT
);

INSERT INTO jira_issues VALUES
('PLAT-501', 'PLAT', 'Story', 'Webhook delivery retry queue', 'Done', 'High', 5, 'PLAT Sprint 14', 'Sofia Reyes', 'Sofia Reyes', '2026-03-20', '2026-04-02', 'backend'),
('PLAT-502', 'PLAT', 'Story', 'Webhook signing key rotation', 'Done', 'Highest', 8, 'PLAT Sprint 14', 'Omar Haddad', 'Sofia Reyes', '2026-03-20', '2026-04-03', 'security'),
('PLAT-503', 'PLAT', 'Bug', 'Webhook delivery duplicate events under retry storm', 'Done', 'High', NULL, 'PLAT Sprint 14', 'Sofia Reyes', 'Priya Chen', '2026-03-25', '2026-04-01', 'bug'),
('PLAT-504', 'PLAT', 'Task', 'Document webhook contract for downstream teams', 'Done', 'Medium', 2, 'PLAT Sprint 14', 'Yuki Tanaka', 'Sofia Reyes', '2026-03-22', '2026-04-05', 'docs'),
('PLAT-505', 'PLAT', 'Story', 'SLA dashboard data feed contract (Atlas dependency)', 'In Progress', 'Highest', 8, 'PLAT Sprint 15', 'Sofia Reyes', 'Priya Chen', '2026-04-03', NULL, 'atlas-dependency'),
('PLAT-506', 'PLAT', 'Bug', 'Signature verification clock skew on provider callback', 'Done', 'Highest', NULL, 'PLAT Sprint 15', 'Omar Haddad', 'Marcus Diallo', '2026-04-08', '2026-04-17', 'bug,atlas-dependency'),
('PLAT-507', 'PLAT', 'Story', 'Rate limit shared webhook gateway per-tenant', 'In Progress', 'High', 5, 'PLAT Sprint 15', 'Yuki Tanaka', 'Sofia Reyes', '2026-04-06', NULL, 'backend'),
('PLAT-508', 'PLAT', 'Task', 'Migrate PLAT board off legacy Jira workflow scheme', 'To Do', 'Medium', 3, 'PLAT Sprint 15', 'Sofia Reyes', 'Marcus Diallo', '2026-04-13', NULL, 'tooling,migration'),
('PLAT-509', 'PLAT', 'Bug', 'Dashboard feed contract test flakes in shared sandbox', 'Blocked', 'High', NULL, 'PLAT Sprint 15', 'Omar Haddad', 'Sofia Reyes', '2026-04-14', NULL, 'bug,atlas-dependency'),
('PLAT-510', 'PLAT', 'Story', 'Add per-tenant quota alerting', 'To Do', 'Medium', 5, 'PLAT Sprint 15', 'Yuki Tanaka', 'Sofia Reyes', '2026-04-14', NULL, 'backend'),
('PLAT-511', 'PLAT', 'Task', 'Export PLAT issue history for GitHub migration spike', 'In Review', 'Medium', 2, 'PLAT Sprint 15', 'Sofia Reyes', 'Marcus Diallo', '2026-04-15', NULL, 'tooling,migration'),
('PLAT-512', 'PLAT', 'Bug', 'Webhook retry queue backs up under provider outage', 'Done', 'High', NULL, 'PLAT Sprint 15', 'Omar Haddad', 'Sofia Reyes', '2026-04-13', '2026-04-16', 'bug'),
('PLAT-513', 'PLAT', 'Story', 'Deprecate legacy webhook v1 endpoint', 'To Do', 'Low', 3, 'PLAT Sprint 15', 'Yuki Tanaka', 'Sofia Reyes', '2026-04-16', NULL, 'tech-debt'),
('PLAT-514', 'PLAT', 'Task', 'Weekly Platform status digest for Priya', 'Done', 'Medium', 1, 'PLAT Sprint 15', 'Sofia Reyes', 'Priya Chen', '2026-04-13', '2026-04-13', 'reporting');
```

Sanity checks:

```sql
SELECT COUNT(*) FROM board_items;                              -- 16
SELECT COUNT(*) FROM board_items WHERE status = 'Done';        -- 8
SELECT COUNT(*) FROM jira_issues;                               -- 14
SELECT COUNT(*) FROM jira_issues WHERE status = 'Done';        -- 7
```

`board_items` mirrors exactly what `gh api graphql` returns for a GitHub Projects v2 board once you've added custom fields — that's not a coincidence, it's the format you'll reproduce yourself in Exercise 1. `jira_issues` mirrors a JQL search result exported to CSV/JSON — the format Exercise 2 has you both query in Jira directly and re-derive in SQL. Notice the two schemas *aren't* the same shape (Jira has `story_points` and `reporter`; GitHub Projects has `size` and `iteration`) — reconciling that mismatch honestly is the whole point of Challenge 1.

## How to navigate this week

Work top to bottom. Each piece assumes the ones above it.

| # | File | What's inside | ~Time |
|--:|------|---------------|------:|
| 1 | [lecture-notes/01-github-projects-deep-dive.md](./lecture-notes/01-github-projects-deep-dive.md) | Views, custom fields, iterations, and built-in automations; wiring issues into a project and exporting it to SQL | 2h |
| 2 | [lecture-notes/02-jira-and-jql.md](./lecture-notes/02-jira-and-jql.md) | Boards, workflows, JQL, exports, and a concept map from Jira to GitHub Projects | 2h |
| 3 | [lecture-notes/03-ai-assisted-pm.md](./lecture-notes/03-ai-assisted-pm.md) | Drafting backlogs, slicing stories, and grounded status/risk digests — plus where AI lies | 2h |
| 4 | [exercises/exercise-01-automate-a-board.md](./exercises/exercise-01-automate-a-board.md) | Stand up a real GitHub Projects board with automations, and script its export | 1.5h |
| 5 | [exercises/exercise-02-write-jql-queries.md](./exercises/exercise-02-write-jql-queries.md) | Write JQL against PLAT, then the equivalent SQL against `jira_issues` | 1.5h |
| 6 | [exercises/exercise-03-ai-draft-a-backlog.md](./exercises/exercise-03-ai-draft-a-backlog.md) | Use AI to draft and refine a sliced, INVEST-checked backlog from a brief | 1h |
| 7 | [challenges/challenge-01-migrate-jira-to-github.md](./challenges/challenge-01-migrate-jira-to-github.md) | Migrate PLAT's backlog into `board_items`' schema and reconcile the mismatch | 2h |
| 8 | [challenges/challenge-02-ai-status-with-guardrails.md](./challenges/challenge-02-ai-status-with-guardrails.md) | Build an AI status digest that is structurally unable to fabricate a number | 1.5h |
| 9 | [mini-project/README.md](./mini-project/README.md) | Automate a board and ship an AI-assisted backlog + grounded status/risk digest | 3.5h |
| 10 | [homework.md](./homework.md) | Extra practice, spread across the week | 4h |
| 11 | [quiz.md](./quiz.md) | 15 self-check questions + answer key | 1h |
| 12 | [resources.md](./resources.md) | Curated free/official docs + the tools to install | — |

## Weekly schedule

The schedule below adds up to approximately **24 hours** for the full-time pace.

| Day | Focus | Lectures | Exercises | Challenges | Quiz/Read | Homework | Mini-Project | Daily Total |
|-----------|--------------------------------------------------|---------:|----------:|-----------:|----------:|---------:|-------------:|------------:|
| Monday | GitHub Projects deep dive; start automating the board | 2h | 1.5h | 0h | 0.5h | 1h | 0h | 5h |
| Tuesday | Jira, workflows & JQL | 2h | 1.5h | 0h | 0.5h | 1h | 0h | 5h |
| Wednesday | AI across the PM loop | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Thursday | Challenge 1 — migrate Jira to GitHub | 0h | 0h | 2h | 0.5h | 1h | 0.5h | 4h |
| Friday | Challenge 2 — AI status with guardrails | 0h | 0h | 1.5h | 0.5h | 0h | 1h | 3h |
| Saturday | Mini-project — automate + AI-assisted workflow | 0h | 0h | 0h | 0h | 0h | 2h | 2h |
| Sunday | Quiz + review | 0h | 0h | 0h | 1h | 0h | 0h | 1h |
| **Total** | | **4h** | **4h** | **3.5h** | **3.5h** | **4h** | **3.5h** | **~24.5h** |

## By the end of this week you can…

- Stand up a GitHub Projects board with the custom fields and automations a real engineering team needs, without clicking around blind.
- Read and write JQL fluently enough to pull a filtered, sorted set of Jira issues without opening the visual filter builder.
- Translate between Jira's vocabulary and GitHub Projects' vocabulary on sight — useful the moment you join a team that uses the other one.
- Export either tool's data into a SQL table and answer questions about it with `SELECT`, `GROUP BY`, and pandas — the same muscle from Week 8, aimed at a new kind of export.
- Draft a usable first-pass backlog with AI in minutes instead of hours, then tell which stories need a human's judgment before they ship to the team.
- Build (and defend, to a skeptical stakeholder) an AI-generated status digest that cannot state a number it didn't compute from real data.

## Up next

[Week 12 — Capstone](../week-12-capstone/) — blank board to shipped, measured delivery: you run the whole loop this course taught, end to end, on a project of your own.

---

*Part of the Code Crunch Worldwide open curriculum · GPL-3.0 · If you find errors, please open an issue or PR.*
