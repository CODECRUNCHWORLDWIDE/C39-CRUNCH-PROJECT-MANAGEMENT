# Lecture 3 — Flow Analysis in pandas

> **Duration:** ~2 hours. **Outcome:** You can pull Atlas's issue data into pandas, compute cycle-time percentiles and explain why they beat an average, measure aging WIP, compute flow efficiency correctly for a mixed population of issues, and read a cumulative flow diagram to say exactly where work is piling up.

SQL is where this data *lives* and where you compute clean, single-number aggregates. pandas is where you go next: distributions, correlations, quick what-if slicing, and anything you'd otherwise reach for a notebook to eyeball. This lecture picks up exactly where Lecture 2 left off — same `atlas_pm` database, same 28 issues — and does the analysis SQL is comparatively clumsy at.

## 1. Getting the data into pandas

```python
import pandas as pd
from sqlalchemy import create_engine

# PostgreSQL
engine = create_engine("postgresql+psycopg2://localhost/atlas_pm")

# SQLite (equivalent, if that's your engine this week)
# engine = create_engine("sqlite:///atlas_pm.db")

issues = pd.read_sql("SELECT * FROM issues", engine, parse_dates=["created_at", "resolved_at"])
history = pd.read_sql("SELECT * FROM issue_status_history", engine, parse_dates=["entered_at", "left_at"])

print(issues.shape)    # (28, 11)
print(history.shape)   # (112, 5)
```

`parse_dates` is doing real work here — without it, pandas reads date columns as plain strings (`object` dtype) and every subtraction below silently fails or, worse, does string comparison instead of date arithmetic. Check with `issues.dtypes` if anything looks off; `resolved_at` and `created_at` should both read `datetime64[ns]`.

`pd.read_sql` is the bridge this whole lecture depends on: **write the filter/join/aggregate in SQL when SQL does it more clearly, then hand the result to pandas for the analysis pandas is better at.** Don't reflexively pull raw tables and redo `WHERE` and `JOIN` in pandas that SQL already does in one readable line — Exercise 3 has you draw that line deliberately.

## 2. Cycle time, computed in pandas

Lecture 2 computed cycle time with a SQL join. Here's the pandas equivalent, useful when you want to keep working with the result as a DataFrame instead of a query result set:

```python
# first entry into "In Progress" per issue
started = (
    history[history["status"] == "In Progress"]
    .groupby("issue_id")["entered_at"]
    .min()
    .rename("started_at")
)

done = issues[issues["current_status"] == "Done"].set_index("issue_id")
done = done.join(started)
done["cycle_time_days"] = (done["resolved_at"] - done["started_at"]).dt.days

print(done[["issue_key", "cycle_time_days"]].sort_values("cycle_time_days", ascending=False).head())
```

```
   issue_key  cycle_time_days
   ATLAS-115                14
   ATLAS-112                11
   ATLAS-119                11
   ATLAS-104                10
   ATLAS-108                10
```

`.dt.days` is the pandas idiom for turning a `Timedelta` column into a plain integer count of days — you'll type it constantly once you start working with date differences.

## 3. Percentiles beat averages — and here's the proof, not the assertion

Take the mean of the 23 completed issues' cycle times and you get something reasonable-looking:

```python
print(done["cycle_time_days"].mean())   # 7.57
print(done["cycle_time_days"].median()) # 8.0
```

Mean and median are close here — `7.57` vs `8.0` — which might tempt you to conclude the mean is fine. It isn't, and the reason is visible the moment you look past the center of the distribution:

```python
percentiles = done["cycle_time_days"].quantile([0.5, 0.85, 0.95, 1.0])
print(percentiles)
```

```
0.50     8.0
0.85    10.0
0.95    11.0
1.00    14.0
```

The mean answers "what's typical." It says nothing about the question a sponsor actually cares about: **"if I hand you a new ticket today, how long before I should worry?"** That's a tail question, and the tail is where the mean is blind. p85 = 10 days tells you *85% of issues finish within 10 days* — a far more useful commitment than "it averages about a week," because a commitment built on the mean is, by construction, broken by roughly half your issues. A commitment built on p85 is broken by only 15%. This is exactly why forecasting tools (and most mature Kanban teams) quote SLAs in percentiles, never means.

**Practical rule:** report p50 (typical case) and p85 (the number you'd actually promise someone) together, and always mention the max as the "what's the worst that's happened" anchor. Never report a bare average of cycle time to a stakeholder — it invites exactly the wrong intuition about risk.

## 4. Flow efficiency — and the segmentation trap inside it

**Flow efficiency** = active time ÷ total cycle time, i.e., the fraction of an issue's cycle time that was actually being worked, versus sitting `Blocked`.

```python
blocked = (
    history[history["status"] == "Blocked"]
    .assign(blocked_days=lambda d: (d["left_at"] - d["entered_at"]).dt.days)
    .groupby("issue_id")["blocked_days"]
    .sum()
)

done["blocked_days"] = done.index.map(blocked).fillna(0)
done["flow_efficiency"] = 1 - (done["blocked_days"] / done["cycle_time_days"])

print(done[["issue_key", "cycle_time_days", "blocked_days", "flow_efficiency"]]
      .sort_values("flow_efficiency"))
```

```
   issue_key  cycle_time_days  blocked_days  flow_efficiency
   ATLAS-122                6             4            0.333
   ATLAS-119               11             7            0.364
   ATLAS-120               10             4            0.600
   ATLAS-112               11             4            0.636
   ATLAS-115               14             5            0.643
   ATLAS-101               10             0            1.000
   ...                    ...           ...              ...
```

Now compute the "one number" everyone will ask for — and watch it lie to you if you compute it the naive way:

```python
naive_average = done["flow_efficiency"].mean()
print(round(naive_average, 3))   # ~0.895 — looks great!
```

`0.895` looks like a healthy team — most work is 89.5% "active." But that number is an **unweighted average across 23 issues, 18 of which were never blocked at all** (flow efficiency exactly `1.0` by definition — there was nothing to be inefficient about). Averaging those 18 perfect scores in with the 5 issues that actually hit a real dependency **dilutes the signal you were looking for down to noise.** Segment first, then look:

```python
was_blocked = done[done["blocked_days"] > 0]
never_blocked = done[done["blocked_days"] == 0]

print(f"Never blocked ({len(never_blocked)} issues): {never_blocked['flow_efficiency'].mean():.1%}")
print(f"Was blocked   ({len(was_blocked)} issues): {was_blocked['flow_efficiency'].mean():.1%}")
```

```
Never blocked (18 issues): 100.0%
Was blocked   (5 issues): 51.5%
```

That's the real story: it isn't that Atlas's flow efficiency is "mostly fine, 89.5%." It's that **18 issues had a perfect, boring, healthy flow, and 5 specific issues — every single one tied to the sandbox or the Platform-team dependency — lost *half* their cycle time to being blocked.** The naive average buries a sharp, specific, fixable problem inside a comfortable-looking overall number. This is the single most important habit in this lecture: **before you average anything, ask whether the population is actually one group or two groups wearing the same column name.**

## 5. Aging WIP — the piece a raw WIP count can't tell you

Lecture 2's WIP count (`3`, as of 2026-04-06) says *how many* things are open. It says nothing about *how long* they've been open — and two issues that have both been "in progress" for a week are a very different situation than two issues that started yesterday. Build the aging view directly from `issues` plus each item's current-status entry date:

```python
SNAPSHOT = pd.Timestamp("2026-04-06")

open_issues = issues[issues["current_status"].isin(["In Progress", "In Review", "Blocked"])].copy()

current_entry = (
    history[history["left_at"].isna()]              # the still-open interval = current status
    .set_index("issue_id")["entered_at"]
)
open_issues = open_issues.set_index("issue_id")
open_issues["entered_current_status"] = current_entry
open_issues["age_in_current_status_days"] = (SNAPSHOT - open_issues["entered_current_status"]).dt.days
open_issues["total_age_days"] = (SNAPSHOT - open_issues["created_at"]).dt.days

print(open_issues[["issue_key", "current_status", "age_in_current_status_days", "total_age_days"]]
      .sort_values("age_in_current_status_days", ascending=False))
```

```
   issue_key  current_status  age_in_current_status_days  total_age_days
   ATLAS-126        Blocked                            3               12
   ATLAS-123    In Progress                            4                12
   ATLAS-125     In Review                            0                5
```

`ATLAS-126` jumps out immediately once you sort by age: it's been sitting `Blocked` for 3 days already, on **the same Platform-team dependency** that cost `ATLAS-119` seven days back in Sprint 6. A raw WIP count of `3` treats this identically to a fresh issue that just started — the age column is what turns "3 things are open" into "one of those three is actively becoming a problem, right now, and it's the same problem as last sprint."

**The rule of thumb worth internalizing:** the older an item gets while still open, the more attention (not less) it deserves — the opposite of how most people's instincts work, which is to focus on what's new and let the old, stuck item fade into the background because it's no longer novel.

## 6. Reading a cumulative flow diagram (CFD)

A CFD plots, for each day, a stacked count of issues in each status — `Backlog` at the bottom, `Done` at the top, everything else stacked between. You don't need a full charting library to *read* one; the numbers alone tell the story. Building the day-by-day counts is a small loop over the interval logic you've now used twice:

```python
def status_counts_on(day, issues, history):
    """How many issues were in each status on `day`, using the open-interval test."""
    snap = history[
        (history["entered_at"] <= day) &
        (history["left_at"].isna() | (history["left_at"] > day))
    ]
    return snap.groupby("status")["issue_id"].nunique()

for day in pd.to_datetime(["2026-03-30", "2026-04-02", "2026-04-06"]):
    print(day.date(), dict(status_counts_on(day, issues, history)))
```

```
2026-03-30 {'Blocked': 1, 'To Do': 4}
2026-04-02 {'Backlog': 1, 'Done': 1, 'In Progress': 4, 'To Do': 1}
2026-04-06 {'Backlog': 1, 'Blocked': 1, 'Done': 2, 'In Progress': 1, 'In Review': 1, 'To Do': 1}
```

Read those three snapshots as a story, not three unrelated tables. At Sprint 7's *start* (03-30), almost nothing had moved — one carryover issue still blocked from Sprint 6. By 04-02, four issues were simultaneously `In Progress` at once — the team pulled in more than it could actively work, a classic overcommitment pattern (see the WIP band in a real CFD: a sudden widening of the "in progress" band is exactly this). By 04-06, the `In Progress`/`In Review`/`Blocked` band (the WIP band) is still holding at 3 issues while `Done` has only reached 2 — **the band never drained back down, it just shuffled which issues occupy it.** That is the visual signature of a stalling team: not "nothing is moving" (things clearly are — statuses keep changing), but "the *middle* of the workflow stays full no matter how much time passes," which is precisely Little's Law again — WIP staying high with throughput this low guarantees cycle time keeps climbing for whatever's still in that band.

## 7. Check yourself

- Why does `parse_dates` matter when calling `pd.read_sql`, and what breaks if you skip it?
- Explain, in one sentence, why p85 is a better number than the mean to put in front of a sponsor.
- Atlas's naive average flow efficiency was 89.5%. What was misleading about reporting that single number, and what two numbers would you report instead?
- What does "age in current status" tell you that a plain WIP count cannot?
- In the CFD reading, what specific pattern in the WIP band is the visual signature of a stalling team — and why does it point back to Little's Law?
- When would you push a calculation back into SQL instead of doing it in pandas?

If those are automatic, you're ready for the exercises — Exercise 3 has you build this week's full cycle-time percentile report as a deliverable, not just a read-along.

## Further reading

- **pandas — `groupby` user guide:** <https://pandas.pydata.org/docs/user_guide/groupby.html>
- **pandas — Time deltas:** <https://pandas.pydata.org/docs/user_guide/timedeltas.html>
- **pandas — `Series.quantile`:** <https://pandas.pydata.org/docs/reference/api/pandas.Series.quantile.html>
- **Daniel Vacanti on cumulative flow diagrams (free article):** <https://actionableagile.com/cumulative-flow-diagrams/>
