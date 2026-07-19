# Week 2 — Agile, Scrum & Kanban

> **Goal:** by Sunday you can run a real sprint's worth of Scrum ceremonies with actual intent (not ritual), stand up a Kanban board with defensible WIP limits, and choose — and defend — the right one for a given team instead of cargo-culting whichever framework you saw last.

Welcome back to **C39 · Crunch Project Management**. Week 1 gave you the shape of a project and the charter that starts it. This week you go inside **execution** — the phase where most of a PM's actual week-to-week work happens — and learn the two dominant ways technology teams organize that work: **Scrum**, which delivers in fixed-length sprints, and **Kanban**, which manages continuous flow. Both are ways of putting the **Agile Manifesto's** four values into practice, and both get cargo-culted constantly: teams that "do Scrum" by running fifteen-minute status meetings and calling them standups, or teams that "do Kanban" by drawing three columns on a whiteboard with no WIP limit and calling it done. This week teaches you what each ceremony and each practice is actually *for*, so you can tell the difference between the real thing and the theater.

We continue with **Project Atlas** at **Northlight Software** — the "Team Workspaces" feature from Week 1, now in execution. Priya (sponsor), Elena (product owner), Marcus (tech lead), and the four-engineer delivery team chose Agile, in two-week sprints, back in Week 1 Lecture 3. This week you run that choice for real: you build Atlas's actual backlog as a queryable table (not a spreadsheet — see the data-tooling note below), design its sprint cadence, facilitate its ceremonies, and then take that *same* backlog and run it as a Kanban board instead, so you can compare the two on real numbers, not vibes.

## A note on tooling this week

Sprint boards and Kanban boards are, underneath the sticky notes, **records with state and history** — an item has a status, a status has a timestamp, and questions like "how long does work sit in review?" or "how many items are in progress right now?" are queries against that history. This course never uses a spreadsheet as a system of record for that kind of data — spreadsheets are covered on their own terms in **C41 · Crunch Excel**. Here, board state lives in a real table and you answer board questions with **SQL** (PostgreSQL 16 primary, SQLite 3.35+ fallback) and, where a chart helps, **Python + pandas**. You set the table up once, below, and every lecture, exercise, and the mini-project queries it.

## Learning objectives

By the end of this week, you will be able to:

- **Explain** the Agile Manifesto's four values and twelve principles in practice — what they actually change about how a team plans, commits, and responds to change — and name how "Agile" gets misused as an excuse for having no plan at all.
- **Run** the Scrum framework with real intent: the three roles, the sprint as a container, and the four ceremonies (planning, daily standup, review, retrospective) — and recognize each ceremony's failure mode when it decays into a status ritual.
- **Run** Kanban: visualize work on a board, set WIP limits that actually constrain work in progress, and manage a continuous pull-based pipeline instead of a batch of iterations.
- **Compare** Scrum and Kanban on the dimensions that actually matter — cadence, predictability, how the work arrives, how change is absorbed — and choose the right fit for a given team instead of defaulting to whichever one is trendy.
- **Facilitate** a standup, a review, and a retrospective that each produce a decision or an action item, not just an update.
- **Recognize** the common Agile anti-patterns (status-theater standups, sprint-in-name-only, retros with no actions, unlimited "in progress" columns) and explain the mechanism by which each one quietly breaks delivery.

## Prerequisites

- Week 1 complete — you should know the five lifecycle phases, be comfortable with the Project Atlas case study, and understand why Atlas chose an Agile delivery approach.
- **PostgreSQL 16+** or **SQLite 3.35+** installed (same requirement as C33 · Crunch SQL). If you have neither yet, install steps are in [`resources.md`](./resources.md); install PostgreSQL if you're only installing one.
- **Python 3.10+** with `pandas` installed (`pip install pandas`) for the two places this week that plot a chart from a query result. Not required to finish the week, but the Kanban lecture and the mini-project are more useful with it.
- No new accounts needed this week.

## Set up this week's data (do this first)

Everything this week runs against one table: Atlas's backlog, modeled as real rows with real state, not sticky notes. Create it once.

**PostgreSQL:**

```bash
createdb c39wk2
psql c39wk2
```

**SQLite:**

```bash
sqlite3 c39wk2.db
```

Then paste this into the shell (portable across both engines):

```sql
CREATE TABLE atlas_backlog (
    item_id             INTEGER PRIMARY KEY,
    title               TEXT    NOT NULL,
    item_type           TEXT    NOT NULL,   -- 'story', 'bug', or 'spike'
    story_points        INTEGER,            -- NULL until estimated
    priority_rank       INTEGER NOT NULL,   -- Elena's backlog order; 1 = highest
    status              TEXT    NOT NULL,   -- 'backlog','todo','in_progress','in_review','done'
    sprint_id           INTEGER,            -- NULL = not yet pulled into a sprint
    assignee            TEXT,
    created_date        DATE    NOT NULL,
    entered_status_date DATE    NOT NULL,   -- when it entered its CURRENT status
    done_date           DATE
);

INSERT INTO atlas_backlog VALUES
(1,'Create a workspace','story',3,1,'done',1,'Priya N.','2026-01-05','2026-01-16','2026-01-16'),
(2,'Invite a teammate by email','story',3,2,'done',1,'Priya N.','2026-01-05','2026-01-17','2026-01-17'),
(3,'Accept a workspace invite','story',2,3,'done',1,'Sam O.','2026-01-05','2026-01-16','2026-01-16'),
(4,'Remove a teammate from a workspace','story',2,4,'done',1,'Sam O.','2026-01-05','2026-01-17','2026-01-17'),
(5,'Shared read-only dashboard view','story',5,5,'done',1,'Dana K.','2026-01-05','2026-01-19','2026-01-19'),
(6,'Shared editable dashboard view','story',5,6,'done',1,'Dana K.','2026-01-05','2026-01-20','2026-01-20'),
(7,'Role: owner permissions','story',3,7,'done',1,'Wes T.','2026-01-05','2026-01-18','2026-01-18'),
(8,'Role: editor permissions','story',3,8,'done',1,'Wes T.','2026-01-05','2026-01-19','2026-01-19'),
(9,'Role: viewer permissions','story',2,9,'done',1,'Wes T.','2026-01-05','2026-01-19','2026-01-19'),
(10,'Add a threaded comment on a dashboard','story',5,10,'in_review',2,'Sam O.','2026-01-05','2026-02-02',NULL),
(11,'Reply to a comment thread','story',3,11,'in_review',2,'Sam O.','2026-01-05','2026-02-02',NULL),
(12,'Resolve/close a comment thread','story',2,12,'in_progress',2,'Dana K.','2026-01-05','2026-01-30',NULL),
(13,'Email notification on new comment','story',3,13,'in_progress',2,'Wes T.','2026-01-12','2026-01-29',NULL),
(14,'@mention a teammate in a comment','story',3,14,'todo',2,NULL,'2026-01-12','2026-01-26',NULL),
(15,'Workspace activity feed','story',5,15,'todo',2,NULL,'2026-01-12','2026-01-26',NULL),
(16,'Bulk-invite teammates via CSV','story',3,16,'backlog',NULL,NULL,'2026-01-12','2026-01-12',NULL),
(17,'Workspace-level usage analytics','story',5,17,'backlog',NULL,NULL,'2026-01-12','2026-01-12',NULL),
(18,'Transfer workspace ownership','story',2,18,'backlog',NULL,NULL,'2026-01-19','2026-01-19',NULL),
(19,'Fix: invite email fails for + addresses','bug',1,19,'in_progress',2,'Dana K.','2026-01-27','2026-01-28',NULL),
(20,'Spike: evaluate real-time delivery for comments','spike',3,20,'backlog',NULL,NULL,'2026-01-19','2026-01-19',NULL);
```

Sanity check — this should print `20`:

```sql
SELECT COUNT(*) FROM atlas_backlog;
```

Notice on purpose: sprint 1 (10 stories, `status = 'done'`) already shipped the workspace/invite/permissions foundation from Week 1's lifecycle walkthrough. Sprint 2 is **in flight** — some items `in_progress`, some `in_review`, some still `todo` — and eight items are still sitting in `backlog` with no `sprint_id`, unestimated territory. That mix is exactly what a real mid-sprint board looks like, and it's what you'll query all week.

## How to navigate this week

Work top to bottom. Each piece assumes the ones above it.

| # | File | What's inside | ~Time |
|--:|------|---------------|------:|
| 1 | [lecture-notes/01-agile-values-in-practice.md](./lecture-notes/01-agile-values-in-practice.md) | The Manifesto's four values and twelve principles, in practice — and how "Agile" gets used as an excuse for no plan | 2h |
| 2 | [lecture-notes/02-scrum-roles-and-ceremonies.md](./lecture-notes/02-scrum-roles-and-ceremonies.md) | PO, Scrum Master, dev team; the sprint container; planning, standup, review, retro — what each is FOR | 2h |
| 3 | [lecture-notes/03-kanban-flow-and-wip.md](./lecture-notes/03-kanban-flow-and-wip.md) | Visualizing flow, WIP limits, pull; querying `atlas_backlog` for cycle time and throughput; Scrum vs. Kanban | 2h |
| 4 | [exercises/exercise-01-design-a-sprint-cadence.md](./exercises/exercise-01-design-a-sprint-cadence.md) | Design Atlas's sprint length and ceremony schedule | 1h |
| 5 | [exercises/exercise-02-set-wip-limits.md](./exercises/exercise-02-set-wip-limits.md) | Query the flooded board, compute, and set real WIP limits | 1h |
| 6 | [exercises/exercise-03-run-a-retro.md](./exercises/exercise-03-run-a-retro.md) | Facilitate Atlas's sprint 2 retro and capture real action items | 1h |
| 7 | [challenges/challenge-01-diagnose-broken-scrum.md](./challenges/challenge-01-diagnose-broken-scrum.md) | Diagnose a team where every Scrum ceremony has decayed into theater | 1.5h |
| 8 | [challenges/challenge-02-scrum-or-kanban-defense.md](./challenges/challenge-02-scrum-or-kanban-defense.md) | Recommend Scrum or Kanban for three real teams and defend it in writing | 1h |
| 9 | [mini-project/README.md](./mini-project/README.md) | Run Atlas's backlog as a Scrum sprint *and* as a Kanban pipeline; compare on real numbers | 3h |
| 10 | [homework.md](./homework.md) | Extra practice, spread across the week | 4h |
| 11 | [quiz.md](./quiz.md) | 14 self-check questions + answer key | 1h |
| 12 | [resources.md](./resources.md) | Curated free/official reading + tools to install | — |

## Weekly schedule

The schedule below adds up to approximately **18.5 hours**.

| Day | Focus | Lectures | Exercises | Challenges | Quiz/Read | Homework | Mini-Project | Daily Total |
|-----------|--------------------------------------------|---------:|----------:|-----------:|----------:|---------:|-------------:|------------:|
| Monday | Agile values; seed the backlog table | 2h | 0h | 0h | 0.5h | 1h | 0h | 3.5h |
| Tuesday | Scrum roles + ceremonies; sprint cadence | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Wednesday | Kanban flow, WIP, querying the board | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Thursday | Retro exercise; diagnose-broken-Scrum challenge | 0h | 1h | 1.5h | 0.5h | 1h | 0h | 4h |
| Friday | Scrum-or-Kanban challenge; start mini-project | 0h | 0h | 1h | 0.5h | 0h | 1.5h | 3h |
| Saturday | Mini-project — run both boards, compare | 0h | 0h | 0h | 0h | 0h | 1.5h | 1.5h |
| Sunday | Quiz + review | 0h | 0h | 0h | 1h | 0h | 0h | 1h |
| **Total** | | **6h** | **3h** | **2.5h** | **3.5h** | **4h** | **3h** | **~22.5h**† |

† Columns are additive; treat the total as a ceiling, not a requirement — reading, querying, and writing overlap in practice.

## By the end of this week you can…

- Explain, without reciting the poster, what each Agile value actually changes about how a team commits to and adjusts a plan.
- Run a sprint planning session, a daily standup, a review, and a retrospective that each produce something concrete — a plan, an unblocked path, a demoed increment, an action item — instead of a status update dressed up as a ceremony.
- Set a WIP limit you can defend with a number, not a gut feeling, and query a board's real cycle time and throughput instead of guessing.
- Look at a team's shape and its work's shape and recommend Scrum, Kanban, or a blend — and say exactly why, using this week's vocabulary.

## Up next

Week 3 — Backlogs, user stories, and estimation: now that you can run the ceremonies and the board, you go inside the backlog itself and learn to write the work that fills it.

---

*Part of the Code Crunch Worldwide open curriculum · GPL-3.0 · If you find errors, please open an issue or PR.*
