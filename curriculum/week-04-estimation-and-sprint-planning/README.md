# Week 4 — Estimation & Sprint Planning

> **Goal:** by Sunday you can size a backlog with planning poker and reference-class forecasting instead of gut-feel hours, compute your team's *real* capacity for a sprint from actual leave/meetings/focus time, and turn a groomed backlog into a sprint plan that states a **commitment**, a **stretch forecast**, and the uncertainty between them — instead of a single confident-sounding number that's quietly a guess.

Welcome back to **C39 · Crunch Project Management**. Week 3 taught you to write and slice a backlog into small, INVEST-clean stories. This week answers the question every one of those stories eventually asks: **how big is it, and how much of it fits in a sprint?** Most teams answer both badly — they estimate in hours (which fails for reasons Lecture 1 makes precise), and they plan sprints by wishful thinking instead of arithmetic. You'll do neither. You'll estimate in **relative size**, forecast with **reference classes** instead of optimism, and plan a sprint against a **capacity number you can actually defend** — computed, not guessed.

We continue with **Project Atlas** at **Northlight Software**. Sprint 1 shipped on time (28 points, the workspace/invite/permissions foundation). Sprint 2 shipped the commenting feature — but ran two days over, because an unplanned bug interruption ate capacity nobody had budgeted for. That overrun is this week's opening data point, not a footnote: it's the gap between what the team's estimates *implied* and what actually happened, and closing that gap — honestly, with numbers — is the whole week. Marcus's team (Priya N., Sam O., Dana K., Wes T.) now has ten new candidate stories for Sprints 3 and 4. You'll point them, compute real capacity for each sprint, and plan the two-sprint delivery Elena is asking about.

## A note on tooling this week

A backlog's points, a team's leave calendar, and a project's estimate-vs-actual history are **records**, not a wall of sticky notes or a spreadsheet tab. As in every week of this course, we never use a spreadsheet as the system of record for that kind of data — spreadsheets are covered on their own terms in **C41 · Crunch Excel**. This week you keep planning-poker results, historical velocity, team capacity, and the sprint plan itself in real tables, and you answer every planning question — "what's our capacity?", "how much did we underestimate by, historically?", "what's a defensible range?" — with **SQL** (PostgreSQL 16 primary, SQLite 3.35+ fallback) and, for the forecasting distribution work, **Python + pandas**. You set the tables up once, below.

## Learning objectives

By the end of this week, you will be able to:

- **Explain** why absolute hour estimates fail on knowledge work — anchoring, the planning fallacy, and hidden complexity that only surfaces once work starts — and why relative sizing (story points) sidesteps those failure modes instead of just hiding them.
- **Run** planning poker with a team: a sizing scale, simultaneous reveal, and a structured discussion of outlier estimates that surfaces disagreement *before* commitment, not during the sprint.
- **Use** reference-class forecasting — your own team's estimate-vs-actual history, and comparable projects outside it — to counter optimism bias instead of estimating from gut feel alone.
- **Calculate** a sprint's real capacity: person-days available after leave and holidays, minus ceremony and meeting overhead, times a realistic focus factor — and convert that into points using the team's actual historical pace, not its aspirational one.
- **Plan** a sprint by pulling a groomed, pointed backlog against real capacity, and explain the difference between a **commitment** (what you're accountable for) and a **forecast** (your best guess at the upside).
- **Communicate** estimate uncertainty honestly, as a stated range with a rationale, instead of a single date that sounds precise and is actually a guess wearing a suit.

## Prerequisites

- Week 3 complete — you should be comfortable writing role–goal–benefit stories, applying INVEST, and slicing an epic vertically. This week assumes you can read a story card and judge its shape.
- **PostgreSQL 16+** or **SQLite 3.35+** installed (same requirement as every prior week). Install steps are in [`resources.md`](./resources.md) if you're missing one.
- **Python 3.10+** with `pandas` (`pip install pandas`) for the reference-class forecasting section of Lecture 2 and the mini-project's range calculation.
- No new accounts needed this week.

## Set up this week's data (do this first)

Four small tables carry the whole week: the team's own history (what got estimated vs. what actually happened), an outside reference class (comparable Northlight projects), the Sprint 3 roster with real leave/meeting data, and the ten new candidate stories waiting to be pointed. Create them once.

**PostgreSQL:**

```bash
createdb c39wk4
psql c39wk4
```

**SQLite:**

```bash
sqlite3 c39wk4.db
```

Then paste this into the shell (portable across both engines):

```sql
-- 1. Atlas's own estimate-vs-actual history: every story shipped in Sprints 1 and 2.
CREATE TABLE atlas_story_history (
    story_id       INTEGER PRIMARY KEY,
    title          TEXT    NOT NULL,
    story_points   INTEGER NOT NULL,   -- sized at planning, using the team's "1 point ~= 1 ideal day" convention
    ideal_days_est NUMERIC NOT NULL,   -- story_points converted to ideal engineering-days at estimate time
    actual_days    NUMERIC NOT NULL,   -- focused engineering-days it actually took, once done
    sprint_id      INTEGER NOT NULL
);

INSERT INTO atlas_story_history VALUES
(1,'Create a workspace',3,3,3.5,1),
(2,'Invite a teammate by email',3,3,4,1),
(3,'Accept a workspace invite',2,2,2,1),
(4,'Remove a teammate from a workspace',2,2,2.5,1),
(5,'Shared read-only dashboard view',5,5,7,1),
(6,'Shared editable dashboard view',5,5,6,1),
(7,'Role: owner permissions',3,3,4,1),
(8,'Role: editor permissions',3,3,3,1),
(9,'Role: viewer permissions',2,2,2,1),
(10,'Add a threaded comment on a dashboard',5,5,6.5,2),
(11,'Reply to a comment thread',3,3,4,2),
(12,'Resolve/close a comment thread',2,2,3,2),
(13,'Email notification on new comment',3,3,5,2),
(14,'@mention a teammate in a comment',3,3,3.5,2),
(15,'Workspace activity feed',5,5,8,2),
(19,'Fix: invite email fails for + addresses',1,1,2,2);

-- 2. The outside reference class: five other Northlight projects, first-stage delivery, planned vs. actual.
CREATE TABLE reference_class_projects (
    project_name        TEXT    PRIMARY KEY,
    team_size            INTEGER NOT NULL,
    planned_weeks         NUMERIC NOT NULL,
    actual_weeks           NUMERIC NOT NULL,
    milestone             TEXT    NOT NULL
);

INSERT INTO reference_class_projects VALUES
('Notify (push notifications)',3,6,9,'MVP launch'),
('Vault (audit log)',5,8,10,'GA'),
('Pulse (usage analytics dashboard)',4,5,8,'MVP launch'),
('Beacon (SSO integration)',4,6,7,'GA'),
('Relay (webhook system)',3,4,6,'MVP launch');

-- 3. Sprint 3 roster: real capacity inputs for the team planning this sprint.
CREATE TABLE team_capacity_calendar (
    person                 TEXT    NOT NULL,
    role                   TEXT    NOT NULL,
    sprint_id              INTEGER NOT NULL,
    working_days           INTEGER NOT NULL,   -- weekdays in the sprint
    leave_days             NUMERIC NOT NULL,   -- PTO / holiday during the sprint
    meeting_hours_per_day  NUMERIC NOT NULL,   -- recurring meetings outside the Scrum ceremonies themselves
    focus_factor           NUMERIC NOT NULL    -- fraction of remaining time actually spent heads-down
);

INSERT INTO team_capacity_calendar VALUES
('Marcus Webb','Tech Lead',3,10,0,2.0,0.6),
('Priya N.','Senior Engineer',3,10,1,1.0,0.8),
('Sam O.','Engineer',3,10,0,1.0,0.8),
('Dana K.','Engineer',3,10,3,1.0,0.8),
('Wes T.','Engineer',3,10,0,1.5,0.7);

-- 4. Ten new candidate stories for Sprints 3 and 4 — groomed (Week 3), not yet pointed.
CREATE TABLE sprint3_candidates (
    item_id       INTEGER PRIMARY KEY,
    title         TEXT    NOT NULL,
    item_type     TEXT    NOT NULL,   -- 'story', 'bug', or 'spike'
    priority_rank INTEGER NOT NULL,   -- Elena's order; 1 = highest
    story_points  INTEGER,            -- NULL until Exercise 1 points it
    notes         TEXT
);

INSERT INTO sprint3_candidates VALUES
(21,'@mention triggers an email notification','story',1,NULL,'Depends on #14 (done, sprint 2) and reuses #13''s email pipeline (done, sprint 2)'),
(22,'Notification preferences page (per-workspace mute)','story',2,NULL,'New settings surface; needs UI + persistence'),
(23,'Daily digest email of unread activity','story',3,NULL,'Batching job; touches the activity feed (#15, done) and email pipeline'),
(24,'Bulk-invite teammates via CSV','story',4,NULL,'Carried over from Sprint 2 backlog; CSV parsing + existing invite flow'),
(25,'Workspace-level usage analytics','story',5,NULL,'Carried over; needs a new aggregation query, no new UI screen yet'),
(26,'Transfer workspace ownership','story',6,NULL,'Carried over; small state-machine change, high blast-radius if wrong'),
(27,'Spike: evaluate real-time delivery for comments','spike',7,NULL,'Time-boxed research, not a shippable feature; carried over'),
(28,'Fix: notification email sent twice on rapid replies','bug',8,NULL,'Race condition in the email queue; reproduced, root cause not yet found'),
(29,'Workspace activity feed filters (by person, by type)','story',9,NULL,'Extends #15 (done); mostly front-end filtering on existing data'),
(30,'Export workspace member list to CSV','story',10,NULL,'Small, well-understood; similar shape to existing export code elsewhere in the app');
```

Sanity check — this should print `16`, `5`, `5`, `10`:

```sql
SELECT COUNT(*) FROM atlas_story_history;
SELECT COUNT(*) FROM reference_class_projects;
SELECT COUNT(*) FROM team_capacity_calendar;
SELECT COUNT(*) FROM sprint3_candidates;
```

Notice, on purpose: `atlas_story_history.actual_days` is consistently **larger** than `ideal_days_est` — every lecture and exercise this week traces exactly how much larger, and why that gap is the most useful number on the team.

## How to navigate this week

Work top to bottom. Each piece assumes the ones above it.

| # | File | What's inside | ~Time |
|--:|------|---------------|------:|
| 1 | [lecture-notes/01-relative-estimation.md](./lecture-notes/01-relative-estimation.md) | Why hour estimates fail; story points as relative size; running planning poker | 2h |
| 2 | [lecture-notes/02-reference-classes-and-bias.md](./lecture-notes/02-reference-classes-and-bias.md) | The planning fallacy; inside vs. outside view; querying history for a bias-correction factor and a range | 2h |
| 3 | [lecture-notes/03-capacity-and-commitment.md](./lecture-notes/03-capacity-and-commitment.md) | Computing real capacity from leave/meetings/focus factor; commitment vs. forecast; planning a sprint | 2h |
| 4 | [exercises/exercise-01-point-a-backlog.md](./exercises/exercise-01-point-a-backlog.md) | Run planning poker on the 10 Sprint 3 candidates | 1h |
| 5 | [exercises/exercise-02-compute-capacity.md](./exercises/exercise-02-compute-capacity.md) | Turn `team_capacity_calendar` into a Sprint 3 capacity number in points | 1h |
| 6 | [exercises/exercise-03-estimate-as-range.md](./exercises/exercise-03-estimate-as-range.md) | Convert a point total into a stated confidence range | 1h |
| 7 | [challenges/challenge-01-recalibrate-inflated-points.md](./challenges/challenge-01-recalibrate-inflated-points.md) | Recalibrate a team whose story points have quietly inflated | 1.5h |
| 8 | [challenges/challenge-02-plan-to-capacity.md](./challenges/challenge-02-plan-to-capacity.md) | Plan a two-sprint delivery to a fixed, computed capacity | 1.5h |
| 9 | [mini-project/README.md](./mini-project/README.md) | Point the backlog, compute capacity, and plan Sprints 3–4 with a commitment and a stretch forecast | 3h |
| 10 | [homework.md](./homework.md) | Extra practice, spread across the week | 4h |
| 11 | [quiz.md](./quiz.md) | 14 self-check questions + answer key | 1h |
| 12 | [resources.md](./resources.md) | Curated free/official reading + tools to install | — |

## Weekly schedule

The schedule below adds up to approximately **20.5 hours**.

| Day | Focus | Lectures | Exercises | Challenges | Quiz/Read | Homework | Mini-Project | Daily Total |
|-----------|--------------------------------------------------|---------:|----------:|-----------:|----------:|---------:|-------------:|------------:|
| Monday | Relative estimation; seed the tables; poker | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Tuesday | Reference classes & bias; querying history | 2h | 0h | 0h | 0.5h | 1h | 0h | 3.5h |
| Wednesday | Capacity & commitment; compute Sprint 3 capacity | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Thursday | Estimate-as-range exercise; recalibration challenge | 0h | 1h | 1.5h | 0.5h | 1h | 0h | 4h |
| Friday | Plan-to-capacity challenge; start mini-project | 0h | 0h | 1.5h | 0.5h | 0h | 1h | 3h |
| Saturday | Mini-project — point, compute, plan two sprints | 0h | 0h | 0h | 0h | 0h | 2h | 2h |
| Sunday | Quiz + review | 0h | 0h | 0h | 1h | 0h | 0h | 1h |
| **Total** | | **6h** | **3h** | **3h** | **3.5h** | **4h** | **3h** | **~22.5h**† |

† Columns are additive; treat the total as a ceiling, not a requirement — estimating, querying, and writing overlap in practice.

## By the end of this week you can…

- Look at a story and size it relative to a known reference story, instead of guessing hours out of thin air.
- Run a planning-poker round that surfaces a 3-vs-13 disagreement *before* the sprint starts, not three days into it.
- Pull your own team's history (or a comparable outside project) and turn "we think it'll take about that long" into a number with a stated correction factor behind it.
- Compute a sprint's real capacity from an actual leave calendar and focus factor — and defend the number if someone asks where it came from.
- Hand a stakeholder a commitment, a stretch forecast, and the honest range between them, instead of a single date you're not sure you believe yourself.

## Up next

Week 5 — Risk, budget, and stakeholder communication: with a sprint plan you can defend, we turn to the things that threaten it — and how to keep a sponsor informed before those threats become surprises.

---

*Part of the Code Crunch Worldwide open curriculum · GPL-3.0 · If you find errors, please open an issue or PR.*
