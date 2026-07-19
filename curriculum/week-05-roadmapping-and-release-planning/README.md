# Week 5 — Roadmapping & Release Planning

> **Goal:** by Sunday you can turn a raw, prioritized backlog into an outcome-based now/next/later roadmap and a capacity-aware, dependency-correct release plan — built with real SQL and Python, not a spreadsheet — and you can keep both honest, at the right altitude, when reality inevitably shifts underneath them.

Welcome back to **C39 · Crunch Project Management**. Project Atlas is three weeks from launch, and Priya has stopped asking about it — she's asking a bigger question: what's next for the whole quarter, and can she show the board a roadmap she trusts? This week takes you one altitude above a single project's charter (Week 1) and backlog (Week 3) into roadmapping: communicating direction honestly across *several* competing initiatives, sequencing them into a release plan the team can actually deliver, and managing what happens when — not if — the plan meets reality.

We stay with **Northlight Software**, **Priya** (VP Product, sponsor), **Elena** (Product Owner), and **Marcus** (tech lead). This week's dataset is Northlight Insights' Q3 backlog: seven initiatives across five themes, sized in story points, one with a hard external deadline (a SOC 2 audit window that will not move). Per this course's data-tooling standard, that backlog lives in a real `roadmap_items` table you query with SQL — not a spreadsheet — because sequencing, capacity math, and dependency correctness are things a query can verify and a spreadsheet can only display.

## Learning objectives

By the end of this week, you will be able to:

- **Build** an outcome-based roadmap — theme → outcome → current best-guess initiative — using a now/next/later structure that communicates intent without over-committing dates.
- **Distinguish** a roadmap from a plan from a promise/commitment, and communicate each honestly at its own confidence level.
- **Sequence** backlog items into releases using SQL window functions, respecting milestones and dependencies, not just priority order.
- **Bake** real capacity — historical velocity minus a support/on-call/PTO buffer — and known risk into a release plan instead of planning against an aspirational number.
- **Choose** between fixed-scope and fixed-date release framing, and cut scope deliberately to protect a genuinely immovable date.
- **Manage** roadmap change without whiplash — decide when a slip warrants a re-plan, recompute the plan against reduced capacity, and communicate it early and small.
- **Present** the same roadmap truthfully at two altitudes — executive and delivery — without the two versions ever contradicting each other.

## Prerequisites

- Week 1's charter, delivery-lifecycle, and triple-constraint vocabulary (this week reuses "change log," "monitoring & control," and the triple constraint directly, now applied at roadmap grain instead of single-project grain).
- Comfortable with basic `SELECT`, `WHERE`, and simple joins. This week introduces **window functions** (`SUM() OVER (...)`) — if that's new, Lecture 2, §4 teaches it in context, but a quick skim of [C33 · Crunch SQL's window-functions material](../../../C33-CRUNCH-SQL/) beforehand doesn't hurt.
- PostgreSQL 16+ **or** SQLite 3.35+, installed (see [`resources.md`](./resources.md)). Python 3.10+ with `pandas` for Lecture 3 and Exercise 3.
- No prior roadmapping or release-management experience needed — that's what this week is for.

## Set up this week's dataset

Everything this week runs against one backlog. Create it once — the full schema and seed data are in [`lecture-notes/02-release-planning.md`, §2](./lecture-notes/02-release-planning.md#2-set-up-the-backlog-and-capacity-tables). Copy the `roadmap_items` and `sprints` `CREATE TABLE`/`INSERT` statements from there into `psql` or `sqlite3` before starting Lecture 1's worked example.

Sanity check — this should print `7`:

```sql
SELECT COUNT(*) FROM roadmap_items;
```

## How to navigate this week

Work top to bottom. Each piece assumes the ones above it.

| # | File | What's inside | ~Time |
|--:|------|---------------|------:|
| 1 | [lecture-notes/01-outcome-based-roadmaps.md](./lecture-notes/01-outcome-based-roadmaps.md) | Roadmap vs. plan vs. promise; feature vs. outcome framing; the now/next/later structure, built from Northlight's Q3 backlog. | 2h |
| 2 | [lecture-notes/02-release-planning.md](./lecture-notes/02-release-planning.md) | Capacity math with a real buffer; SQL window-function sequencing; fixed-scope vs. fixed-date releases; dependency-integrity checks. | 2.5h |
| 3 | [lecture-notes/03-managing-roadmap-change.md](./lecture-notes/03-managing-roadmap-change.md) | Deciding when a slip needs a re-plan; recomputing capacity in Python; communicating early; presenting to exec vs. delivery audiences. | 2h |
| 4 | [exercises/exercise-01-build-now-next-later.md](./exercises/exercise-01-build-now-next-later.md) | Build your own now/next/later roadmap from the query output. | 1h |
| 5 | [exercises/exercise-02-sequence-releases.md](./exercises/exercise-02-sequence-releases.md) | Sequence a backlog into three releases under a changed, uneven capacity. | 1.5h |
| 6 | [exercises/exercise-03-replan-a-slip.md](./exercises/exercise-03-replan-a-slip.md) | Re-plan a roadmap after a two-week slip, end to end. | 1.5h |
| 7 | [challenges/challenge-01-roadmap-for-two-audiences.md](./challenges/challenge-01-roadmap-for-two-audiences.md) | Present one roadmap truthfully to execs and to the delivery team. | 1.5h |
| 8 | [challenges/challenge-02-fixed-date-tradeoffs.md](./challenges/challenge-02-fixed-date-tradeoffs.md) | Cut a release plan to hit a fixed date that just moved up on you. | 1.5h |
| 9 | [mini-project/README.md](./mini-project/README.md) | Produce your own quarterly outcome-based roadmap and release plan, capacity-aware and defensible. | 3h |
| 10 | [homework.md](./homework.md) | Extra practice, spread across the week. | 4.5h |
| 11 | [quiz.md](./quiz.md) | 15 self-check questions + answer key. | 1h |
| 12 | [resources.md](./resources.md) | Curated free/official reading + tools to install. | — |

## Weekly schedule

The schedule below adds up to approximately **22 hours**. Treat it as a target, not a requirement — most learners land a bit under, since reading and writing overlap.

| Day | Focus | Lectures | Exercises | Challenges | Quiz/Read | Homework | Mini-Project | Daily Total |
|-----------|--------------------------------------------|---------:|----------:|-----------:|----------:|---------:|-------------:|------------:|
| Monday | Outcome-based roadmaps; now/next/later | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Tuesday | Release planning, capacity, SQL sequencing | 2.5h | 1.5h | 0h | 0.5h | 1h | 0h | 5.5h |
| Wednesday | Fixed-scope vs. fixed-date; dependencies | 0h | 1.5h | 0h | 0.5h | 1h | 0h | 3h |
| Thursday | Managing roadmap change; the re-plan loop | 2h | 1.5h | 0h | 0.5h | 1h | 0h | 5h |
| Friday | Challenges — two audiences, fixed dates | 0h | 0h | 3h | 0.5h | 1.5h | 0h | 5h |
| Saturday | Mini-project — your own roadmap + release plan | 0h | 0h | 0h | 0h | 0h | 3h | 3h |
| Sunday | Quiz + review | 0h | 0h | 0h | 1h | 0h | 0h | 1h |
| **Total** | | **6.5h** | **4.5h** | **3h** | **3.5h** | **4.5h** | **3h** | **~25h** |

## By the end of this week you can…

- Turn a raw backlog into a roadmap that says something true and useful about three different confidence horizons, in the language of outcomes, not features.
- Write a SQL query that sequences a backlog into releases, respects dependencies, and proves it did — instead of hoping a spreadsheet's row order is right.
- Look at a hard external date, quantify exactly how much scope has to give to protect it, and make that trade-off a visible decision instead of a silent scramble.
- Catch a slip early, recompute the plan against real remaining capacity, and tell a sponsor about it in a way that preserves trust instead of costing it.
- Present the same underlying roadmap truthfully to a board and to an engineering team without either version being a different story.

## Up next

Week 6 — Risk & Issue Management — takes the risk register this week's SOC 2 milestone hinted at and makes it the main event: identifying, scoring, and actively managing risk before it becomes the next slip you have to re-plan around.

---

*Part of the Code Crunch Worldwide open curriculum · GPL-3.0 · If you find errors, please open an issue or PR.*
