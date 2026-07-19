# Lecture 3 — Kanban: Flow, WIP Limits, Pull

> **Duration:** ~2 hours. **Outcome:** You can explain Kanban's core mechanics (visualize, limit WIP, manage flow, pull don't push), query `atlas_backlog` for age, lead time, and throughput, and choose between Scrum and Kanban for a given team using real criteria instead of a coin flip.

Scrum organizes work into fixed-length batches. Kanban organizes work as a **continuous pipeline** with no batches at all — items arrive, flow through a small number of stages, and ship whenever they're ready, one at a time, at whatever pace the pipeline can sustain. Both are legitimate ways to run Agile values (Lecture 1) — they just fit different shapes of work, and this lecture teaches you to tell which.

## 1. Kanban's four core practices

Kanban, as a management method (distinct from the Japanese manufacturing term it borrows from Toyota's production system), rests on four practices:

1. **Visualize the workflow.** Every piece of work and its current stage is visible on a shared board — nothing lives only in someone's head or inbox.
2. **Limit work in progress (WIP).** Each active column has a hard cap on how many items may sit in it at once. This is the practice that makes Kanban Kanban — without a real WIP limit, you just have a to-do list with columns.
3. **Manage flow.** The team's attention goes to keeping items *moving* smoothly through the pipeline — not to how much is being started, but to how much is finishing, and where things get stuck.
4. **Make process policies explicit.** What does it mean for an item to be allowed to move from "in review" to "done"? Written down, not tribal knowledge.

Notice none of these mention a fixed time-box. Kanban has no sprint. Work is pulled into the next stage **only when there's capacity for it** — never pushed forward because a deadline arrived.

## 2. Why WIP limits are the whole point

Here's the mechanism, concretely. Imagine a board with columns To Do → In Progress → In Review → Done, and **no WIP limit** on In Progress. A well-meaning team, eager to look productive, starts a new item the moment anyone has spare capacity — five engineers, five items in progress, seems fine. Except:

- Every item now gets **partial attention**, split five ways, instead of full attention until it's actually done.
- Context-switching between five half-finished things has a real cost — each switch costs time re-loading context that a single-item focus never pays.
- Nothing *finishes* faster just because more things started — in fact the opposite: the more items in flight, the longer, on average, each one takes to actually cross the finish line. This is a consequence of queueing theory (formalized as **Little's Law**, section 4), not opinion.
- A blocked item quietly sits for days because nobody's forced to notice — there's always something else to work on instead of unblocking it.

A **WIP limit** forces a different behavior: if In Progress is capped at, say, 4 items and it's full, nobody may start a fifth — the team's next move has to be **helping finish one of the four**, not starting something new. This single constraint is what turns "a board with columns" into an actual flow-management system. It manufactures urgency around *finishing*, which a to-do list with no cap never does.

**Where the limit number comes from:** not a guess — start from real capacity. A common rule of thumb is roughly one to two items per person actively working that stage, then tune down (not up) as the team learns where the real bottleneck is. Exercise 2 this week walks through setting a real limit on a flooded board using this logic plus a query against the seed data.

## 3. Pull, not push

In a push system, work gets assigned to whoever's available, whenever a manager or a backlog decides it should start — the person receiving it didn't choose the timing. In a **pull** system, a team member pulls the next item into a column **only when they have real capacity for it**, respecting the WIP limit. This sounds like a small difference; it isn't. Push systems tend to overload people who look available on paper even when they're mid-context on something else. Pull systems put the timing decision in the hands of the person who actually knows their own capacity.

At Northlight, imagine Northlight's **Support/Platform team** — the people who triage customer bug reports and infrastructure issues (recall Week 1's ops-vs-project distinction: this is genuinely ops work, unpredictable arrival, no fixed end). If a manager pushed every incoming ticket straight into "In Progress" the moment it arrived, the team would be perpetually overloaded and every ticket would move slowly. Instead, tickets sit in a "Ready" column, prioritized, and an engineer **pulls** the next one only when they finish (or hand off) what they're currently on. The WIP limit on "In Progress" is what makes this real rather than aspirational.

## 4. Little's Law, briefly

A useful formal relationship, and worth internalizing even at an intuitive level:

```
Average time an item spends in the system  =  Average number of items in the system  /  Average completion rate
                     (cycle time)          =        (WIP)              /         (throughput)
```

The practical takeaway: for a fixed throughput (how fast the team actually finishes things), **more WIP means longer cycle time** — every item added to the pipeline slows down every other item already in it. This is the mathematical reason "just start more things in parallel" doesn't make a team faster; it's usually the single biggest lever a struggling team has, and it's counterintuitive enough that it's worth saying twice: **lowering** WIP, not raising it, is usually what speeds a flooded pipeline back up.

## 5. Flow metrics you actually query

Three numbers matter for managing a Kanban pipeline, and — per this course's data-tooling rule — you get them by querying `atlas_backlog`, not by eyeballing a board.

### a) Current WIP per column

```sql
SELECT status, COUNT(*) AS wip_count
FROM atlas_backlog
WHERE status NOT IN ('backlog', 'done')
GROUP BY status
ORDER BY status;
```

Against this week's seed data:

| status | wip_count |
|---|---:|
| in_progress | 3 |
| in_review | 2 |
| todo | 2 |

Seven items are actively in flight (out of the ten in sprint 2). That's your current WIP, full stop — the number a WIP limit constrains directly.

### b) Age in current column (spotting a stuck item)

An item's **age** is how long it's sat in its current status. Old age in an active column is the single clearest bottleneck signal a Kanban board gives you — it's more useful than the raw WIP count, because it tells you not just *how much* is in flight but *what's actually stuck*.

**PostgreSQL** (date subtraction returns an integer number of days):

```sql
SELECT item_id, title, status, entered_status_date,
       CURRENT_DATE - entered_status_date AS days_in_status
FROM atlas_backlog
WHERE status NOT IN ('backlog', 'done')
ORDER BY days_in_status DESC;
```

**SQLite** (use `julianday()` and subtract, then cast):

```sql
SELECT item_id, title, status, entered_status_date,
       CAST(julianday('now') - julianday(entered_status_date) AS INTEGER) AS days_in_status
FROM atlas_backlog
WHERE status NOT IN ('backlog', 'done')
ORDER BY days_in_status DESC;
```

Since `CURRENT_DATE` / `'now'` will differ from the fixed narrative date this lecture uses, pin a reference date explicitly when you want reproducible numbers for a write-up:

```sql
-- Portable pattern: replace CURRENT_DATE with a literal date for a reproducible snapshot
SELECT item_id, title, status, entered_status_date,
       DATE('2026-02-03') - entered_status_date AS days_in_status   -- SQLite: use julianday() form above instead
FROM atlas_backlog
WHERE status NOT IN ('backlog', 'done')
ORDER BY days_in_status DESC;
```

As of `2026-02-03`, the todo items (#14 and #15, `@mention` and the activity feed) have been sitting **8 days** without anyone pulling them — the longest age on the board. That's not "in progress," so it's easy to overlook, but it's still a flow signal worth asking about at standup: is the team actually pulling from the top of the backlog, or is something else jumping the queue?

### c) Lead time (for finished work)

**Lead time** measures the full trip: from when an item was created to when it shipped. It's a different number from **cycle time** (time spent in *active* work only, after being pulled) — this table's schema tracks `created_date` and `done_date` for every finished item, which gives you lead time directly; measuring cycle time strictly requires a full transition log (every status change with its own timestamp), which is a natural extension of this schema but out of scope for this week's seed table.

```sql
SELECT item_id, title,
       done_date, created_date,
       (done_date - created_date) AS lead_time_days    -- SQLite: julianday(done_date) - julianday(created_date)
FROM atlas_backlog
WHERE status = 'done'
ORDER BY lead_time_days DESC;
```

Sprint 1's nine shipped stories range from 11 to 15 days of lead time. Tracking this over many sprints (not just one) is what tells a team whether their pipeline is speeding up, slowing down, or holding steady — a single sprint's numbers are too noisy to act on alone.

### d) Throughput (how many finished per week)

```sql
SELECT strftime('%Y-%W', done_date) AS iso_week, COUNT(*) AS items_completed   -- SQLite
FROM atlas_backlog
WHERE done_date IS NOT NULL
GROUP BY iso_week
ORDER BY iso_week;
```

```sql
-- PostgreSQL equivalent
SELECT to_char(done_date, 'IYYY-IW') AS iso_week, COUNT(*) AS items_completed
FROM atlas_backlog
WHERE done_date IS NOT NULL
GROUP BY iso_week
ORDER BY iso_week;
```

Throughput — not velocity (Week 4) — is Kanban's native "how fast are we going" number: a plain count of finished items per unit time, with no story-point weighting required. It's a simpler metric than Scrum's velocity precisely because Kanban doesn't have a sprint boundary to measure velocity *against*.

### e) A quick cumulative flow picture with pandas

A cumulative flow diagram (CFD) stacks WIP-per-column over time and is the classic Kanban visual for spotting a growing bottleneck (a widening band in one column). Full CFDs need a status-change history table; here's the shape of the code once you have one (or once you've built the transitions log in the mini-project's stretch goal):

```python
import pandas as pd
import sqlite3

conn = sqlite3.connect("c39wk2.db")
snapshot = pd.read_sql_query("""
    SELECT status, COUNT(*) AS wip_count
    FROM atlas_backlog
    WHERE status NOT IN ('backlog', 'done')
    GROUP BY status
""", conn)

ax = snapshot.set_index("status").plot.bar(legend=False, title="Atlas sprint 2 — current WIP by column")
ax.set_ylabel("items")
```

That's a single day's snapshot as a bar chart — useful, but a real CFD needs one row per (date, status, count) captured daily. The mini-project's stretch goal asks you to build exactly that.

## 6. Choosing between Scrum and Kanban

Neither framework is "more Agile" than the other — both implement Lecture 1's values. The choice is about fit:

| Dimension | Scrum fits better when… | Kanban fits better when… |
|---|---|---|
| **How work arrives** | Work can be planned in advance, roughly two weeks at a time | Work arrives unpredictably (support tickets, incidents, ad hoc requests) |
| **Team's goal shape** | The team benefits from a shared, time-boxed goal to rally around | There's no natural "batch" — it's a continuous queue |
| **Stakeholder rhythm** | Stakeholders want a predictable cadence of demos and check-ins | Stakeholders care about individual item turnaround time, not a batch cadence |
| **Change tolerance** | Some protection from mid-cycle disruption is valuable (a sprint boundary) | Priorities can shift item-by-item, at any time, without breaking anything |
| **Measuring progress** | Velocity (points completed per sprint) is a meaningful, comparable number | Throughput and cycle time (item-by-item) are more meaningful than any "batch" |

**Project Atlas fits Scrum** — feature work with a known backlog, a product owner actively prioritizing, and stakeholders (Priya, pilot customers) who benefit from a predictable two-week demo rhythm. **Northlight's Support/Platform team fits Kanban** — tickets arrive whenever a customer hits a bug, there's no meaningful way to "sprint-plan" an incident that hasn't happened yet, and what matters is how fast each ticket gets resolved, not how many fit in a fixed two-week box.

Some teams run a hybrid often called **Scrumban** — Kanban's WIP limits and pull mechanics, inside Scrum's sprint cadence and ceremonies (a sprint that limits WIP within it, rather than being a batch of separately-assigned tasks). This week's Challenge 2 asks you to make this exact call, in writing, for three different team shapes.

## 7. Check yourself

- Name Kanban's four core practices, and explain which one is the one that actually changes team behavior (not just visibility).
- Using Little's Law, explain why adding more WIP to a flooded pipeline makes items finish *slower*, not faster.
- What's the difference between lead time and cycle time, and which one does this week's seed table let you measure directly?
- Give one signal — a query result, not a feeling — that would tell you a Kanban column needs a lower WIP limit.
- For Atlas (feature work, prioritized backlog, two-week stakeholder rhythm) versus the Support/Platform team (unpredictable ticket arrival), explain in one sentence each why Scrum fits one and Kanban fits the other.

If those are automatic, you're ready for this week's exercises — designing Atlas's sprint cadence, setting real WIP limits on a flooded board, and running an actual retrospective.

## Further reading

- **Kanban University — "What is Kanban?":** <https://kanban.university/kanban-guide/>
- **Atlassian — "Kanban vs. Scrum":** <https://www.atlassian.com/agile/kanban/kanban-vs-scrum>
- **Wikipedia — "Little's Law" (queueing theory background):** <https://en.wikipedia.org/wiki/Little%27s_law>
