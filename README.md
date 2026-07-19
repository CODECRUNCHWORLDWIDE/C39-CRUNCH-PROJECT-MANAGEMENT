# C39 · Crunch Project Management

> A free, open-source 12-week course on leading technology projects with Agile, AI, and industry-standard workflows — planning, estimation, risk, delivery metrics, and stakeholder management, hands-on in GitHub Projects and Jira.

[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](LICENSE)
[![GitHub Projects · Jira](https://img.shields.io/badge/stack-GitHub_Projects_·_Jira-2463EB.svg)](#stack)
[![Built in the open](https://img.shields.io/badge/built-in%20the%20open-2463EB.svg)](https://github.com/CODECRUNCHWORLDWIDE)

C39 takes you from "what does a project manager actually do?" to running a technology project end-to-end — writing a backlog, estimating and planning sprints, managing risk and dependencies, keeping stakeholders aligned, and reading delivery metrics like a senior delivery lead. The management layer the backend, data, and product courses all assume: this course makes you the person on the team who ships on time without the theater.

---

## Pathway summary

- **Full-time:** 12 weeks · ~24 hrs/week · ~288 hours
- **Working-engineer pace:** 6 months · ~12 hrs/week
- **Evening pace:** 12 months · ~6 hrs/week

See [`SYLLABUS.md`](SYLLABUS.md).

---

## What you will be able to do at the end of 12 weeks

- **Run the whole lifecycle:** frame a project from initiation to closure — scope, objectives, success criteria, and a delivery plan you can defend.
- **Work Agile for real:** run Scrum and Kanban properly, choose between them, and stop cargo-culting the ceremonies.
- **Own the backlog:** write user stories with clear acceptance criteria, slice epics, and keep a prioritized, ready backlog.
- **Estimate and plan honestly:** story points, relative sizing, capacity-based planning, and roadmaps that survive contact with reality.
- **Manage risk and dependencies:** build a risk register, model cross-team dependencies, and unblock delivery before it stalls.
- **Keep stakeholders aligned:** map stakeholders, tailor communication, run status that people trust, and manage change without drama.
- **Measure delivery with data:** compute velocity, cycle time, throughput, and burndown/burnup from real Jira/GitHub exports using SQL + Python (never a spreadsheet-as-database).
- **Plan budget and capacity with SQL, not spreadsheets:** model cost, burn, and resourcing in PostgreSQL + pandas — auditable, queryable, versioned.
- **Ship quality releases:** definition of done, release management, and a launch you can roll back.
- **Lead a capstone:** take a technology project from a blank board to a shipped, measured delivery in GitHub Projects and Jira.

---

## Curriculum (12 weeks)

| Week | Topic | You leave able to… |
|------|-------|--------------------|
| 1 | [PM foundations & the delivery lifecycle](curriculum/week-01-pm-foundations-and-delivery-lifecycle/) | Frame a project: scope, goals, success criteria, lifecycle. |
| 2 | [Agile, Scrum & Kanban](curriculum/week-02-agile-scrum-and-kanban/) | Run Scrum and Kanban properly and choose between them. |
| 3 | [Backlog, user stories & requirements](curriculum/week-03-backlog-user-stories-and-requirements/) | Write sliced stories with clear acceptance criteria. |
| 4 | [Estimation & sprint planning](curriculum/week-04-estimation-and-sprint-planning/) | Size work relatively and plan a sprint to capacity. |
| 5 | [Roadmapping & release planning](curriculum/week-05-roadmapping-and-release-planning/) | Build a roadmap and a release plan that hold up. |
| 6 | [Risk & dependency management](curriculum/week-06-risk-and-dependency-management/) | Run a risk register and map cross-team dependencies. |
| 7 | [Stakeholder & communication management](curriculum/week-07-stakeholder-and-communication-management/) | Map stakeholders and run status people trust. |
| 8 | [Delivery metrics & flow](curriculum/week-08-delivery-metrics-and-flow/) | Compute velocity, cycle time, and burndown from real data. |
| 9 | [Budgeting, resourcing & capacity](curriculum/week-09-budgeting-resourcing-and-capacity/) | Model cost and resourcing in SQL + pandas, not Excel. |
| 10 | [Quality & release management](curriculum/week-10-quality-and-release-management/) | Ship a release with a DoD and a rollback plan. |
| 11 | [Tooling & AI-assisted PM](curriculum/week-11-tooling-and-ai-assisted-pm/) | Drive GitHub Projects + Jira and use AI to accelerate PM. |
| 12 | [Capstone — lead a project](curriculum/week-12-capstone-lead-a-project/) | Take a project from blank board to shipped + measured. |

---

## How to navigate a week

Every week folder holds the same structure:

- **`README.md`** — the week overview + how the pieces fit + the week's goal.
- **`lecture-notes/`** — 3 lectures (~2 hrs each), the conceptual core.
- **`exercises/`** — 3 short, guided reps on a real board.
- **`challenges/`** — 2 open-ended problems with no single right answer.
- **`mini-project/`** — one build that ties the week together.
- **`homework.md`**, **`quiz.md`**, **`resources.md`** — practice, self-check, and further reading.

---

## Stack

GitHub Projects and Jira as the primary delivery tools — both have free tiers and run in the browser. Wherever the course needs to store, model, or query delivery data (metrics, budgets, resourcing, risk registers), it uses **SQL (PostgreSQL 16, SQLite for zero-setup) and Python (pandas)** — never a spreadsheet as a database. Spreadsheets are taught only in C41 Crunch Excel; here, business and finance workflows that traditionally live in a spreadsheet are done with SQL + Python so they stay auditable, queryable, and versioned. You'll also use `psql`, an AI assistant, and a seed dataset of exported issues shipped with the course.

---

*Part of the Code Crunch Worldwide open curriculum · GPL-3.0 · [Browse all courses](https://codecrunchglobal.vercel.app/courses)*
