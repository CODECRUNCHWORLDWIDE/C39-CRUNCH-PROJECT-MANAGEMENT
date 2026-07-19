# Exercise 2 — Forecast Your Capstone Ship Date From Flow Data

**Skill:** Applying Lecture 2, §5 — pulling real throughput history in SQL and running a Monte Carlo simulation in pandas/numpy to forecast your own ship date.

**Estimated time:** 1.5 hours. Do this Thursday, once you have at least 3–4 real logged working days in `capstone_status_history`.

---

## Setup

Confirm you actually have enough data before starting:

```sql
SELECT COUNT(DISTINCT done_at) AS days_with_data
FROM capstone_items
WHERE done_at IS NOT NULL;
```

If this returns fewer than 3, stop and go log a few more days honestly (Lecture 2, §1 and §4) before running a forecast — a forecast built on 1–2 data points is not this exercise, it's a coin flip dressed up in code.

You'll need `pandas` and `numpy` installed (`pip install pandas numpy`) and your database driver (`psycopg2-binary` for Postgres, or the built-in `sqlite3` module).

---

## Part A — Pull daily throughput in SQL (20 min)

Write and run:

```sql
SELECT
    done_at AS day,
    COUNT(*) AS items_done
FROM capstone_items
WHERE done_at IS NOT NULL
GROUP BY done_at
ORDER BY day;
```

Paste the actual result into your notes. Note how many *distinct* days appear versus how many calendar days have actually passed since you started — the gap between those two numbers is your zero-throughput days, and you need them in the simulation (Lecture 2, §5c).

## Part B — Build the full daily series in pandas (25 min)

```python
import pandas as pd
import numpy as np

# adjust the connection for your engine
import sqlite3
conn = sqlite3.connect("capstone_pm.db")

df = pd.read_sql(
    "SELECT done_at AS day, COUNT(*) AS items_done "
    "FROM capstone_items WHERE done_at IS NOT NULL GROUP BY done_at ORDER BY day;",
    conn,
)
df["day"] = pd.to_datetime(df["day"])

full_range = pd.date_range(df["day"].min(), pd.Timestamp.today().normalize(), freq="D")
throughput = df.set_index("day").reindex(full_range, fill_value=0)["items_done"]

print(throughput)
print(f"\nMean daily throughput: {throughput.mean():.2f}")
print(f"Days with zero items done: {(throughput == 0).sum()} / {len(throughput)}")
```

Answer in your notes: does the zero-days count surprise you? Compare it against what your risk register (Lecture 2, §2) says about blockers you hit this week — do they line up?

## Part C — Get your remaining item count (5 min)

```sql
SELECT COUNT(*) AS remaining FROM capstone_items WHERE current_status != 'Done';
```

## Part D — Run the Monte Carlo simulation (30 min)

Using Lecture 2, §5e as your template exactly, run 10,000 simulations and report p50/p85/p95 in **days from today**, then convert to actual calendar dates:

```python
rng = np.random.default_rng(seed=42)
history = throughput.to_numpy()
remaining_items = 6          # from Part C — replace with your real number
n_simulations = 10_000

days_to_finish = []
for _ in range(n_simulations):
    completed, days = 0, 0
    while completed < remaining_items and days < 60:
        completed += rng.choice(history)
        days += 1
    days_to_finish.append(days)

days_to_finish = np.array(days_to_finish)
p50, p85, p95 = np.percentile(days_to_finish, [50, 85, 95])

today = pd.Timestamp.today().normalize()
print(f"p50: {p50:.0f} days  ->  {(today + pd.Timedelta(days=p50)).date()}")
print(f"p85: {p85:.0f} days  ->  {(today + pd.Timedelta(days=p85)).date()}")
print(f"p95: {p95:.0f} days  ->  {(today + pd.Timedelta(days=p95)).date()}")
```

## Part E — Write it up (10 min)

In `forecast.md`, record:

1. The p50/p85/p95 dates.
2. Whether p85 lands on or before your target ship date.
3. If it doesn't: which of the three levers (Lecture 2, §6 — cut scope, extend date, increase throughput) you're pulling, and why, in one honest paragraph.

---

## Expected outcome

A `forecast.py` (or notebook) that runs cleanly against your live `capstone_pm` data, and a `forecast.md` with three dated confidence levels and an honest read of whether you're on track. Keep the raw `days_to_finish` array or a histogram of it — Challenge 2 and the mini-project's delivery report both reuse this exact output.

## Common mistakes

- **Forgetting the zero-throughput days.** If you only pull rows where `items_done > 0` and skip the reindex in Part B, your simulation will be systematically over-optimistic — every simulated day assumes work got done, which isn't true of your real history.
- **Using too few simulations.** 100 simulations gives a noisy, unstable percentile that changes every run. 10,000 is cheap (well under a second) and stable — there's no real reason to use fewer.
- **Reporting only the p50.** The median alone hides exactly the variance the whole technique exists to surface (Lecture 2, §5f) — always report at least p50 and p85 together.
- **Treating the forecast as certain.** If your risk register has an open, high-probability risk that hasn't materialized yet, say so next to the forecast, not instead of it.
