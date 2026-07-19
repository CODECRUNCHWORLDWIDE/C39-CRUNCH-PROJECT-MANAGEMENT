# Week 1 — PM Foundations & the Delivery Lifecycle

> **Goal:** by Sunday you can walk into any technology team, correctly name what kind of work you're looking at (project, product, or ops), place it on the delivery lifecycle, and write a one-page charter that a skeptical sponsor would actually sign.

Welcome to **C39 · Crunch Project Management**. Before you touch a Gantt chart, a sprint board, or a burndown chart, you need the thing underneath all of them: a clear model of what a project *is*, what a project manager actually *does* on a technology team, and the shape every delivery takes from the first vague idea to the day it ships and the team moves on. Get this foundation solid and every later week — Agile ceremonies, backlogs, estimation, risk, metrics — has somewhere to attach. Skip it, and you'll be running ceremonies without knowing why they exist.

We work against one running case study all week: **Northlight Software**, a small B2B analytics company, and a real initiative called **Project Atlas** — building a "Team Workspaces" feature for their product, Northlight Insights. You'll meet the sponsor, the product owner, the tech lead, and the PM (you) in Lecture 1, walk their delivery end-to-end in Lecture 2, and write Atlas's actual charter in Lecture 3 and the mini-project.

## Learning objectives

By the end of this week, you will be able to:

- **Define** project vs. product vs. operations, and place the PM correctly among the sponsor, product owner, tech lead, and delivery team.
- **Walk** the full delivery lifecycle — initiation, planning, execution, monitoring/control, closure — and name the artifact and decision each phase produces.
- **Write** a defensible one-page project charter: objectives, scope (in and out), constraints, and measurable success criteria.
- **Distinguish** predictive, iterative, and Agile delivery approaches, and choose the right one for a given brief and defend the choice.
- **Set** measurable success criteria and an explicit definition of done for a project, not just a feature.
- **Identify** the triple constraint (scope / time / cost) and explain how quality trades against it when one side moves.

## Prerequisites

- No prior project management experience or certification needed — that's what this course is for.
- Comfortable reading a short business scenario and writing plain English (this week is conceptual and written, not tool-heavy).
- A free [GitHub](https://github.com/join) account — you'll open a GitHub Projects board in Week 2 and reuse the charter you write this week.
- Nothing to install this week. Weeks 8 and 9 use PostgreSQL + Python for delivery metrics and budgeting — see that week's `resources.md` when you get there.

## How this week is organized

Every technology team runs projects whether or not anyone owns the title "project manager." This week gives you the vocabulary and the artifacts to do the job on purpose instead of by accident. Work top to bottom — each piece assumes the ones above it.

| # | File | What's inside | ~Time |
|--:|------|---------------|------:|
| 1 | [lecture-notes/01-what-a-pm-does.md](./lecture-notes/01-what-a-pm-does.md) | The PM's real job: framing, unblocking, communicating. Roles around the PM and the project/product boundary. | 2h |
| 2 | [lecture-notes/02-the-delivery-lifecycle.md](./lecture-notes/02-the-delivery-lifecycle.md) | Initiation → planning → execution → monitoring/control → closure, walked through Project Atlas end-to-end. | 2h |
| 3 | [lecture-notes/03-charter-and-success-criteria.md](./lecture-notes/03-charter-and-success-criteria.md) | Writing a defensible charter, the triple constraint, and choosing a delivery approach. | 2h |
| 4 | [exercises/exercise-01-classify-project-vs-ops.md](./exercises/exercise-01-classify-project-vs-ops.md) | Sort 15 real work items into project, product, or ops. | 1h |
| 5 | [exercises/exercise-02-spot-the-lifecycle-phase.md](./exercises/exercise-02-spot-the-lifecycle-phase.md) | Map 12 real events from Project Atlas to their lifecycle phase. | 1h |
| 6 | [exercises/exercise-03-write-success-criteria.md](./exercises/exercise-03-write-success-criteria.md) | Turn 8 vague goals into measurable success criteria. | 1h |
| 7 | [challenges/challenge-01-rescue-a-scopeless-project.md](./challenges/challenge-01-rescue-a-scopeless-project.md) | Rescue a project that's six weeks in with no defined scope. | 1.5h |
| 8 | [challenges/challenge-02-choose-a-delivery-approach.md](./challenges/challenge-02-choose-a-delivery-approach.md) | Pick predictive vs. Agile for three real briefs and defend it in writing. | 1h |
| 9 | [mini-project/README.md](./mini-project/README.md) | Write a one-page charter and delivery plan for a real build of your choosing. | 3h |
| 10 | [homework.md](./homework.md) | Extra practice, spread across the week. | 4h |
| 11 | [quiz.md](./quiz.md) | 15 self-check questions + answer key. | 1h |
| 12 | [resources.md](./resources.md) | Curated free/official reading + tools to install later. | — |

## Weekly schedule

The schedule below adds up to approximately **16.5 hours** — a conceptual, writing-heavy week rather than a tool-heavy one.

| Day | Focus | Lectures | Exercises | Challenges | Quiz/Read | Homework | Mini-Project | Daily Total |
|-----------|--------------------------------------------|---------:|----------:|-----------:|----------:|---------:|-------------:|------------:|
| Monday | Roles, project vs. product vs. ops | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Tuesday | The delivery lifecycle end-to-end | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Wednesday | Charter, scope, success criteria | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Thursday | Triple constraint; delivery approaches | 0h | 0h | 1.5h | 0.5h | 1h | 1h | 4h |
| Friday | Challenges; start the charter | 0h | 0h | 1h | 0.5h | 1h | 1.5h | 4h |
| Saturday | Mini-project — write the charter | 0h | 0h | 0h | 0h | 0h | 2.5h | 2.5h |
| Sunday | Quiz + review | 0h | 0h | 0h | 1h | 0h | 0h | 1h |
| **Total** | | **6h** | **3h** | **2.5h** | **3.5h** | **5h** | **5h** | **~25h**† |

† Totals across the table are additive per column; the course's full-time pace budgets roughly 24 hrs/week, so treat the last row as a ceiling, not a requirement — most learners land closer to 16–18 hours actually spent, since reading and writing overlap.

## By the end of this week you can…

- Look at a piece of work and correctly say whether it's a project, ongoing product work, or operations — and explain why the distinction changes how you'd manage it.
- Name the five lifecycle phases in order, and say what breaks when a team skips initiation and jumps straight to execution.
- Write a one-page charter with real scope boundaries and success criteria someone could actually measure against, not vague aspirations.
- Argue, in writing, for a delivery approach (predictive, iterative, or Agile) given a specific brief — and know when you'd argue the other way.

## Up next

[Week 2 — Agile, Scrum & Kanban](../week-02-agile-scrum-and-kanban/) — once you can frame a project and see its shape end-to-end, we go inside execution and run it week to week.

---

*Part of the Code Crunch Worldwide open curriculum · GPL-3.0 · If you find errors, please open an issue or PR.*
