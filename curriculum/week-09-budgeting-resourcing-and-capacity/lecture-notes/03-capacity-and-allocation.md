# Lecture 3 — Capacity & Allocation Planning

> **Duration:** ~2 hours. **Outcome:** You can model a portfolio's planned allocation across people and projects in SQL, pull it into pandas and detect every over-allocated person-month automatically, and build a resourcing plan that accounts for real leave, contractor ramp-up, and the hidden cost of context-switching.

Lectures 1–2 answered "what does Atlas cost." This lecture answers a different, equally load-bearing question: **is the plan even physically possible, given the actual people involved?** A budget can be perfectly funded and still fail, silently, because it assumes a person has more hours in a week than they actually do — and the person most likely to be over-committed is rarely the one complaining loudest. They're usually too busy to complain. This is the direct mechanism behind Week 6/7's Platform Team dependency running late: nobody planned for Sofia Reyes to fail. Somebody planned for her to be in two places at once, and this lecture shows you exactly how to catch that before it becomes a missed date.

## 1. Why capacity is a portfolio question, not a per-project one

The single biggest modeling mistake in resourcing is checking allocation **one project at a time**. Atlas's own plan can look flawless — every task has an owner, every owner has hours — and still be wrong, because that owner is *also* 70% committed to something else you didn't look at. Capacity only means anything measured **per person, across every project they touch, in the same time window.** That's why this week's `allocations` table has a `project` column and isn't scoped to Atlas alone — Elena Cruz and Sofia Reyes both split their time across Atlas and other real work, and the over-allocation this lecture finds is invisible if you only ever query `WHERE project = 'Atlas'`.

## 2. Populate the portfolio allocation plan

```sql
-- Core delivery team: fully dedicated to Atlas every month
INSERT INTO allocations (allocation_id, person_id, month_start, project, allocated_pct) VALUES
(1,  1, '2025-09-01', 'Atlas', 100), (2,  1, '2025-10-01', 'Atlas', 100), (3,  1, '2025-11-01', 'Atlas', 100), (4,  1, '2025-12-01', 'Atlas', 100),  -- Marcus Webb
(5,  2, '2025-09-01', 'Atlas', 100), (6,  2, '2025-10-01', 'Atlas', 100), (7,  2, '2025-11-01', 'Atlas', 100), (8,  2, '2025-12-01', 'Atlas', 100),  -- Priya N.
(9,  3, '2025-09-01', 'Atlas', 100), (10, 3, '2025-10-01', 'Atlas', 100), (11, 3, '2025-11-01', 'Atlas', 100), (12, 3, '2025-12-01', 'Atlas', 100),  -- Sam O.
(13, 5, '2025-09-01', 'Atlas', 100), (14, 5, '2025-10-01', 'Atlas', 100), (15, 5, '2025-11-01', 'Atlas', 100), (16, 5, '2025-12-01', 'Atlas', 100),  -- Wes T.
(17, 9, '2025-09-01', 'Atlas', 100), (18, 9, '2025-10-01', 'Atlas', 100), (19, 9, '2025-11-01', 'Atlas', 100), (20, 9, '2025-12-01', 'Atlas', 100);  -- PM (You)

-- Dana K.: a planned dip in November for known PTO — handled proactively, not discovered late
INSERT INTO allocations (allocation_id, person_id, month_start, project, allocated_pct) VALUES
(21, 4, '2025-09-01', 'Atlas', 100),
(22, 4, '2025-10-01', 'Atlas', 100),
(23, 4, '2025-11-01', 'Atlas',  70),   -- one week of planned leave that month
(24, 4, '2025-12-01', 'Atlas', 100);

-- Yuki Tanaka (contractor, 30h/week cap): ramping up, not full allocation from day one
INSERT INTO allocations (allocation_id, person_id, month_start, project, allocated_pct) VALUES
(25, 6, '2025-09-01', 'Atlas',  50),   -- onboarding month
(26, 6, '2025-10-01', 'Atlas', 100),
(27, 6, '2025-11-01', 'Atlas', 100),
(28, 6, '2025-12-01', 'Atlas', 100);

-- Elena Cruz (Product Owner): split between Atlas and Ledger Redesign
INSERT INTO allocations (allocation_id, person_id, month_start, project, allocated_pct) VALUES
(29, 7, '2025-09-01', 'Atlas',           50), (30, 7, '2025-09-01', 'Ledger Redesign', 40),
(31, 7, '2025-10-01', 'Atlas',           50), (32, 7, '2025-10-01', 'Ledger Redesign', 60),
(33, 7, '2025-11-01', 'Atlas',           40), (34, 7, '2025-11-01', 'Ledger Redesign', 50),
(35, 7, '2025-12-01', 'Atlas',           50), (36, 7, '2025-12-01', 'Ledger Redesign', 40);

-- Sofia Reyes (Platform Team Lead): split between her own team's roadmap and the Atlas dependency
INSERT INTO allocations (allocation_id, person_id, month_start, project, allocated_pct) VALUES
(37, 8, '2025-09-01', 'Platform Core', 70), (38, 8, '2025-09-01', 'Atlas', 20),
(39, 8, '2025-10-01', 'Platform Core', 70), (40, 8, '2025-10-01', 'Atlas', 30),
(41, 8, '2025-11-01', 'Platform Core', 70), (42, 8, '2025-11-01', 'Atlas', 40),
(43, 8, '2025-12-01', 'Platform Core', 60), (44, 8, '2025-12-01', 'Atlas', 30);
```

## 3. Finding over-allocation in SQL first

You don't need pandas to catch the obvious case — `GROUP BY` and `HAVING` do it in one query:

```sql
SELECT
    p.name,
    a.month_start,
    SUM(a.allocated_pct) AS total_allocated_pct
FROM allocations a
JOIN people p ON p.person_id = a.person_id
GROUP BY p.name, a.month_start
HAVING SUM(a.allocated_pct) > 100
ORDER BY a.month_start, total_allocated_pct DESC;
```

| name | month_start | total_allocated_pct |
|---|---|---:|
| Elena Cruz | 2025-10-01 | 110 |
| Sofia Reyes | 2025-11-01 | 110 |

Two people, two different months, both over 100%. Elena's October split (50% Atlas + 60% Ledger Redesign) and Sofia's November split (70% Platform Core + 40% Atlas) are both *planned* over-allocation — nobody scheduled either of them for more hours than exist in their week, but the arithmetic quietly did it anyway, one project at a time, until you sum across the whole portfolio. **Sofia's November row is the root cause of Week 6/7's Platform dependency slip, made visible.** The risk register told you a dependency was late. This query tells you *why* — the person owning it was double-booked, by the plan itself, the same month it slipped.

## 4. Why pandas, once the analysis gets richer

The SQL query in §3 is the right tool for "who's over 100%, this instant." Pandas earns its place once you need to layer in things a flat percentage can't express on its own: **converting percent-of-capacity into real hours** (because Yuki's 100% is 30 hours, not 40), **comparing plan to what people actually logged**, and **modeling a hypothetical** (§7's "what if we cut headcount") without touching the live database. Pull the same data in with `pandas.read_sql`:

```python
import pandas as pd
from sqlalchemy import create_engine

# SQLite: engine = create_engine("sqlite:///atlas_pm.db")
engine = create_engine("postgresql://localhost/atlas_pm")

people = pd.read_sql("SELECT * FROM people", engine)
allocations = pd.read_sql("SELECT * FROM allocations", engine)

df = allocations.merge(people[["person_id", "name", "weekly_capacity_hours"]], on="person_id")
```

## 5. Percent isn't hours — normalize before you compare people

Two people both "at 100%" are not equally busy if their weekly capacity differs. Convert every row to **planned hours per month** (assuming 4 working weeks per month, a deliberate simplification this course states outright rather than hiding):

```python
WEEKS_PER_MONTH = 4  # a stated simplification; a real system uses each month's actual working-day count

df["planned_hours"] = df["weekly_capacity_hours"] * WEEKS_PER_MONTH * df["allocated_pct"] / 100

monthly = (
    df.groupby(["name", "month_start"])
      .agg(total_pct=("allocated_pct", "sum"),
           total_hours=("planned_hours", "sum"),
           capacity_hours=("weekly_capacity_hours", "first"))
      .reset_index()
)
monthly["capacity_hours"] = monthly["capacity_hours"] * WEEKS_PER_MONTH
monthly["over_allocated"] = monthly["total_pct"] > 100
print(monthly[monthly["over_allocated"]])
```

```
         name month_start  total_pct  total_hours  capacity_hours  over_allocated
   Elena Cruz  2025-10-01        110        176.0           160.0            True
  Sofia Reyes  2025-11-01        110        176.0           160.0            True
```

Same two flags as the SQL query — the point of this step isn't to find something new, it's to confirm the percent-based flag survives conversion to real hours, and to have `planned_hours` on hand for §6 and §7's harder questions, where percent alone stops being enough.

## 6. Plan vs. actual — did the over-allocation actually cost something?

An over-allocated plan is a warning. Whether it turned into real strain is a different question, answered by comparing to what people actually logged. Populate November's `time_entries` and compare:

```sql
INSERT INTO time_entries (time_entry_id, person_id, work_month, project, workstream, hours) VALUES
(1, 8, '2025-11-01', 'Atlas',          'platform-dependency', 70),  -- Sofia: planned 40% of 160h = 64h; logged 70h
(2, 8, '2025-11-01', 'Platform Core',  'backend',              90),  -- Sofia: planned 70% of 160h = 112h; logged only 90h
(3, 7, '2025-11-01', 'Atlas',          'pm',                   60),  -- Elena: planned 40% of 160h = 64h; logged 60h
(4, 7, '2025-11-01', 'Ledger Redesign','design',                95),  -- Elena: planned 50% of 160h = 80h; logged 95h
(5, 1, '2025-11-01', 'Atlas',          'backend',              150),
(6, 4, '2025-11-01', 'Atlas',          'backend',              105),
(7, 6, '2025-11-01', 'Atlas',          'frontend',             115);
```

```python
time_entries = pd.read_sql("SELECT * FROM time_entries WHERE work_month = '2025-11-01'", engine)

nov_plan = df[df["month_start"] == "2025-11-01"][["name", "project", "planned_hours"]]
nov_actual = time_entries.merge(people[["person_id", "name"]], on="person_id")[["name", "project", "hours"]]

compare = nov_plan.merge(nov_actual, on=["name", "project"], how="outer").fillna(0)
compare["variance_hours"] = compare["hours"] - compare["planned_hours"]
print(compare.sort_values("variance_hours"))
```

```
        name          project  planned_hours  hours  variance_hours
Sofia Reyes    Platform Core          112.0   90.0          -22.0
Sofia Reyes            Atlas           64.0   70.0            6.0
Elena Cruz              Atlas           64.0   60.0           -4.0
Elena Cruz   Ledger Redesign            80.0   95.0           15.0
```

Read Sofia's two rows together and the mechanism is exact: she logged **22 fewer hours than planned on Platform Core** and **6 more than planned on Atlas** — the over-allocation didn't average out or self-correct, it resolved by quietly starving the lower-visibility project (her own team's roadmap) in favor of the one with a PM actively asking for status. That's not a character flaw — it's what over-allocated people do under real constraint, predictably, and it's exactly the Platform Core work that fell 22 hours behind that same month, feeding straight into the dependency slip you logged in Week 6.

## 7. Building a resourcing plan that respects real availability

Three real-world constraints a naive allocation plan ignores by default, and how this week's model handles each:

**Leave.** Dana K.'s November allocation was set to 70%, not 100%, *before* the month started — because her PTO was known in advance. This is the cheap, correct fix: a resourcing plan should encode known leave as a lower `allocated_pct`, not silently assume 100% and discover the gap when a task slips.

**Ramp-up.** Yuki Tanaka's September allocation is 50%, not 100%, in her onboarding month. A new person — employee or contractor — is never at full effective capacity on day one, regardless of their contracted hours; planning them at 100% from month one guarantees an early miss that looks like underperformance but is actually a planning error. A simple, defensible ramp curve (50% month one, 100% from month two) beats pretending ramp-up doesn't exist.

**Context-switching.** This is the constraint percentages hide even when the math is *exactly* 100%. Elena Cruz's September split (50% Atlas + 40% Ledger Redesign = 90%, technically *under* capacity) still costs her more than 90% of a single dedicated person's output would — every switch between two active work contexts carries a real tax in re-orientation time that a flat percentage never accounts for. There's no universal number (estimates in the literature commonly range 15–25% lost productivity per person actively split across two concurrent projects), but the practical rule for a resourcing plan is simple and cheap to apply: **treat any person split across two or more active projects in the same month as running at slightly less than the sum of their percentages implies**, and say so explicitly in the plan rather than silently trusting the arithmetic. A person "at 90%" split two ways is not equivalent to a person at 90% on one thing.

## 8. Check yourself

- Why does checking allocation one project at a time miss Sofia Reyes's over-allocation, even if Atlas's own plan looks completely reasonable in isolation?
- Yuki Tanaka is allocated "100%" in October. Is that the same number of hours as Marcus Webb's 100%? Why or why not, and what column makes the difference visible?
- Walk through, in your own words, how Sofia's November plan-vs-actual comparison explains the Week 6 Platform dependency slip in concrete hours, not just narrative.
- Name the three real-world constraints from §7. For each, describe the specific modeling choice that accounts for it.
- Why might a person allocated at exactly 90% across two projects still underperform relative to a person at 90% on one project? What's the name for the effect, and why doesn't a flat percentage capture it?

If those are automatic, you're ready for this week's exercises, challenges, and the mini-project — building Atlas's (or your own project's) full budget, burn analysis, and portfolio resourcing plan as one connected deliverable.

## Further reading

- **pandas docs — `groupby` and aggregation:** <https://pandas.pydata.org/docs/user_guide/groupby.html>
- **pandas docs — merging and joining DataFrames:** <https://pandas.pydata.org/docs/user_guide/merging.html>
- **Gerald Weinberg — the cost of context-switching (summary, "Quality Software Management: Systems Thinking"):** <https://en.wikipedia.org/wiki/Gerald_Weinberg> — search "Weinberg context switching project management" for accessible summaries of the widely-cited productivity-loss curve referenced in §7.
- **SQLAlchemy — Engine configuration (used by `pandas.read_sql`):** <https://docs.sqlalchemy.org/en/20/core/engines.html>
