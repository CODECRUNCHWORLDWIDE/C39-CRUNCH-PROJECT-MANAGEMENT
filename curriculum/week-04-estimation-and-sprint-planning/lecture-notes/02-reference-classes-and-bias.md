# Lecture 2 — Reference Classes & Optimism Bias

> **Duration:** ~2 hours. **Outcome:** You can name the planning fallacy and explain why it survives experience, distinguish the "inside view" from the "outside view," query `atlas_story_history` for a bias-correction factor, cross-check it against an outside reference class, and turn a point estimate into a stated range instead of a false-precision number.

Lecture 1 got you a *size* for each story — relative, calibrated against Atlas's own history. This lecture asks the harder question: **given that size, how much real time will it actually take, and how confident should you be?** The answer lives in data Atlas already has, sitting in `atlas_story_history`, and it says something uncomfortable: the team has been underestimating, consistently, in a specific and predictable direction.

## 1. The planning fallacy, precisely

Kahneman and Tversky's **planning fallacy** is the well-replicated finding that people (and teams) systematically underestimate the time, cost, and risk of future actions, *while simultaneously overestimating the benefits* — and, critically, **this doesn't go away with experience.** The Sydney Opera House was estimated at 4 years and AU$7 million; it took 14 years and cost over AU$100 million. That wasn't an inexperienced team — it's a structural feature of how the human mind plans.

The mechanism: when you estimate a task, you naturally construct a **plan-based scenario** — a specific mental story of how the work will go — and then estimate based on that story. The story is almost always the *best-case* path: the dependency arrives on time, the library works as documented, nobody's on leave, the design review doesn't surface a rethink. Real work is a probability distribution over many possible paths, most of them slightly-to-very worse than the best case, and a small number occasionally better. Estimating from one imagined best-case path instead of the real distribution is where the bias comes from — and no amount of "try to imagine what could go wrong" fully fixes it, because you're still the same person doing the imagining, subject to the same bias, one level up.

## 2. Inside view vs. outside view

This is Kahneman's clean framing, and it's the single idea this lecture wants you to leave with.

**The inside view:** estimate a task by focusing on the specifics of *this* task, *this* team, *this* plan — "we know the codebase, we know the CSV format is simple, this should be quick." Every planning conversation defaults to the inside view because it's the natural, concrete way to think about work you understand.

**The outside view:** ignore the specifics for a moment and ask **"how long did comparable efforts actually take, historically?"** — not "how will *this one* go" but "what does the *class* of efforts like this one actually look like, empirically?" This is **reference-class forecasting**, formalized by planning researcher Bent Flyvbjerg for large infrastructure projects, and it works for a two-week sprint exactly the way it works for a bridge: the outside view is less flattering and more accurate, because it's grounded in what actually happened rather than what you're currently imagining will happen.

Atlas has two reference classes available, and using both is stronger than using either alone.

## 3. Reference class 1: Atlas's own inside history

`atlas_story_history` has sixteen stories where you know **both** numbers — the size estimated at planning time (converted to "ideal days" using the team's own 1-point-≈-1-ideal-day convention from Sprint 1) and the actual focused engineering-days it took. That gap, measured, *is* Atlas's personal planning-fallacy correction factor.

```sql
SELECT
    SUM(ideal_days_est)                                   AS total_estimated_days,
    SUM(actual_days)                                      AS total_actual_days,
    ROUND(SUM(actual_days) / SUM(ideal_days_est), 2)      AS overrun_factor
FROM atlas_story_history;
```

Run it and you get `total_estimated_days = 50`, `total_actual_days = 66`, `overrun_factor = 1.32`. **Atlas has historically taken 32% longer than its own ideal-day estimates said it would.** Not because the team is bad at its job — Sprint 1 shipped on time and Sprint 2 shipped a real feature — but because "ideal day" estimates, like all estimates, were built from the inside view: the best-case path, not the realistic distribution of paths.

Break it down further and the pattern isn't uniform — it's worth knowing *where* the bias concentrates:

```sql
SELECT
    story_points,
    COUNT(*)                                              AS n_stories,
    ROUND(AVG(actual_days / ideal_days_est), 2)           AS avg_overrun_factor
FROM atlas_story_history
GROUP BY story_points
ORDER BY story_points;
```

The 5-point stories overrun the hardest (Shared read-only dashboard view: 5 estimated, 7 actual; Workspace activity feed: 5 estimated, 8 actual) — larger stories carry more hidden complexity, which tracks with Lecture 1's point about the Fibonacci scale widening at the top. The 1-point bug fix (`invite email fails for + addresses`) doubled its estimate outright — small, "obviously quick" items are exactly where teams skip the "what could go wrong" step entirely, because the story *feels* too small to warrant it.

## 4. Reference class 2: the outside view

Atlas's own history is real, but sixteen stories across two sprints is a small sample, and it's entirely possible something specific to Atlas (a particularly gnarly two sprints, unlucky timing) is skewing it. That's exactly the situation reference-class forecasting is for: pull data from **comparable projects outside the one you're estimating**, where "comparable" means similar team size and similar stage (a young Agile team, first deliverable, not yet at a mature steady-state velocity).

```sql
SELECT
    project_name,
    team_size,
    planned_weeks,
    actual_weeks,
    ROUND(actual_weeks / planned_weeks, 2) AS overrun_factor
FROM reference_class_projects
ORDER BY overrun_factor DESC;
```

| project_name | team_size | planned_weeks | actual_weeks | overrun_factor |
|---|---:|---:|---:|---:|
| Pulse (usage analytics dashboard) | 4 | 5 | 8 | 1.60 |
| Notify (push notifications) | 3 | 6 | 9 | 1.50 |
| Relay (webhook system) | 3 | 4 | 6 | 1.50 |
| Vault (audit log) | 5 | 8 | 10 | 1.25 |
| Beacon (SSO integration) | 4 | 6 | 7 | 1.17 |

```sql
SELECT ROUND(AVG(actual_weeks / planned_weeks), 2) AS avg_outside_overrun
FROM reference_class_projects;
```

`avg_outside_overrun = 1.40`. **Five unrelated, similarly-staffed Northlight projects overran their own plans by an average of 40%.** That's a strikingly similar number to Atlas's own 32% — two independent estimates of the same underlying phenomenon, converging in the same range. That convergence is what gives you confidence: if the inside view said 32% and the outside view said 140%, you'd have to figure out what's different about Atlas before trusting either number. When they agree, you can use the more specific one (Atlas's own 1.32) with real confidence, and cite the outside 1.40 as a sanity check when a stakeholder asks "how do you know that's not just bad luck?"

## 5. Turning a factor into a range, not a point

The whole reason this lecture exists is that a single corrected number is *still* a false-precision trap if you present it as one figure. The honest move is a **range**, built from the two ends of what your data actually supports:

- **Low end (optimistic / "if things go close to plan"):** the raw ideal-day estimate, `1.0×` — what the team would deliver if this sprint looked like the best case baked into every estimate.
- **High end (realistic / "what history says actually happens"):** the ideal-day estimate multiplied by the measured overrun factor, `1.32×` (Atlas's own, cross-checked against the outside `1.40×`).

For a 21-point sprint's worth of stories (≈21 ideal days at the team's 1-point-≈-1-day convention), that's a range of **21 to 28 ideal-days'-worth of actual effort** — not "21 points, done by Friday." Lecture 3 turns this same range into the language stakeholders actually need: a **commitment** and a **stretch forecast**.

## 6. A quick Python cross-check with pandas

For a sanity check beyond a single SQL aggregate — and because a distribution is more informative than one average — pull the story-level ratios into pandas and look at the spread, not just the mean:

```python
import pandas as pd
import sqlite3  # or psycopg2 / sqlalchemy for Postgres

conn = sqlite3.connect("c39wk4.db")
df = pd.read_sql("SELECT title, ideal_days_est, actual_days FROM atlas_story_history", conn)
df["overrun"] = df["actual_days"] / df["ideal_days_est"]

print(df["overrun"].describe())
# count    16.000000
# mean      1.30...
# min       1.00   <- a couple of stories landed exactly on estimate
# 50%       1.25   <- the median overrun, less pulled by the worst outliers than the mean
# max       1.75   <- the activity feed story: 5 estimated, 8 actual (1.6x) or the invite-email bug (2.0x)
```

The mean (1.32) and the median (1.25) are close but not identical — a useful reminder that a couple of bad-outlier stories pull the mean up. When you present a correction factor to a team, showing both, plus the max, tells a more honest story than the mean alone: "typically 25% over, sometimes as much as 75% over on the worst case" is more useful to a planner than "32% over" stated as if it's a law of physics.

## 7. Check yourself

- In your own words, what's the difference between the inside view and the outside view — and why is the outside view usually less flattering and more accurate?
- Why doesn't experience alone fix the planning fallacy?
- Compute, by hand or by query, Atlas's overrun factor if you exclude the two `NULL`-email bug fix numbers — does the story change much? Why might excluding bugs from this calculation be a bad idea?
- Why does this lecture recommend a **range**, built from two real numbers, instead of a single "corrected" estimate?
- Atlas's inside-view factor (1.32) and the outside reference class (1.40) are close but not identical. What would you do differently if they'd come back wildly different, say 1.3 vs. 3.0?

If those are automatic, Lecture 3 takes the range this lecture produces and turns it into an actual sprint plan — a real capacity number, a commitment, and a forecast.

## Further reading

- **Kahneman & Tversky, "Intuitive Prediction: Biases and Corrective Procedures" (1979)** — the original planning-fallacy paper: <https://apps.dtic.mil/sti/citations/ADA073679>
- **Flyvbjerg, "From Nobel Prize to Project Management: Getting Risks Right" (2006)** — reference-class forecasting, applied: <https://arxiv.org/abs/1302.3642>
- **Kahneman, *Thinking, Fast and Slow*, Ch. 23 ("The Outside View")** — the most readable treatment of inside vs. outside view: <https://www.penguinrandomhouse.com/books/89308/thinking-fast-and-slow-by-daniel-kahneman/>
- **pandas — `DataFrame.describe()`:** <https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.describe.html>
