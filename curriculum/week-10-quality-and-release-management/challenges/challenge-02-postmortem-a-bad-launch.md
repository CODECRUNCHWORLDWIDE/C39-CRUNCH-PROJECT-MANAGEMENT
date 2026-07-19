# Challenge 2 — Postmortem a Bad Launch

**Time:** ~90 minutes. **Difficulty:** Medium-hard. **No single right answer, but a wrong tone is a real failure mode here.**

## The scenario

Not every Northlight release is Atlas. Back on **Friday, February 6, 2026**, Northlight's **Reporting Squad** — a different team, led by **Diego Ramirez** — shipped a small-sounding feature: **Bulk CSV Export**, letting customers export their entire account's data as one file instead of paging through the UI. It was scoped as low-risk. It shipped **big-bang, on a Friday afternoon, with no feature flag, and no written rollback plan** — three decisions that Lecture 2 and Lecture 3 both specifically warn against, made by a team that hadn't taken this course yet. It went badly. This is the real incident that, six weeks later, is part of why **Omar Farouk** — the same SRE now running Atlas's GA — insisted on a rehearsed rollback plan before he'd sign off on anything.

Your job is not to figure out whose fault it was. Your job is to run the review the Reporting Squad should have run, and didn't, at the time.

## Load the incident timeline

```sql
INSERT INTO release_events (event_id, release_name, event_time, event_type, description, actor) VALUES
(101, 'northlight-bulk-csv-export-2026-02-06', '2026-02-06 16:45:00', 'deploy',        'Bulk CSV Export deployed to 100% of production, no feature flag, no staged rollout', 'Diego Ramirez'),
(102, 'northlight-bulk-csv-export-2026-02-06', '2026-02-06 17:10:00', 'alert',         'Database CPU utilization alert fires: sustained 95%+ for 5 minutes', 'monitoring system'),
(103, 'northlight-bulk-csv-export-2026-02-06', '2026-02-06 17:22:00', 'customer_report','First customer report: "the whole app is timing out," not specifically the export feature', 'Support inbox'),
(104, 'northlight-bulk-csv-export-2026-02-06', '2026-02-06 17:35:00', 'investigation', 'On-call (Omar Farouk) begins investigating; finds Bulk CSV Export queries doing full table scans on large customer accounts, no query limit or pagination', 'Omar Farouk'),
(105, 'northlight-bulk-csv-export-2026-02-06', '2026-02-06 17:50:00', 'customer_report','Three more customer reports; one describes a total outage on their account', 'Support inbox'),
(106, 'northlight-bulk-csv-export-2026-02-06', '2026-02-06 18:05:00', 'mitigation',    'Omar manually kills the longest-running export queries to relieve CPU pressure', 'Omar Farouk'),
(107, 'northlight-bulk-csv-export-2026-02-06', '2026-02-06 18:20:00', 'investigation', 'CPU spikes again within 15 minutes as new export requests queue up behind the same unoptimized query', 'Omar Farouk'),
(108, 'northlight-bulk-csv-export-2026-02-06', '2026-02-06 18:40:00', 'rollback',      'Decision made to roll back the deploy — no feature flag exists, so this requires a full redeploy of the previous release', 'Diego Ramirez'),
(109, 'northlight-bulk-csv-export-2026-02-06', '2026-02-06 19:15:00', 'rollback',      'Previous stable release redeployed; export feature no longer reachable', 'Omar Farouk'),
(110, 'northlight-bulk-csv-export-2026-02-06', '2026-02-06 19:25:00', 'resolved',      'Database CPU back to baseline; app responsive for all customers', 'Omar Farouk');
```

## Your task

Produce `postmortem.md` with these sections. Follow [Lecture 3, §4](../lecture-notes/03-rollback-and-postmortem.md)'s structure exactly — this challenge is as much about the *discipline* as the content.

1. **Reconstruct the timeline in SQL, with computed durations.** Write a query against `release_events` (filtered to this `release_name`, ordered by `event_time`) that also computes the time elapsed since the *previous* event, in minutes. *(Hint: a window function — `event_time - LAG(event_time) OVER (ORDER BY event_time)` in Postgres, or the SQLite-compatible equivalent using `julianday()`.)* Paste the real output.

2. **Compute two headline numbers in SQL or pandas**: time from deploy to first customer-visible symptom (event 101 → 103), and total time from first alert to resolution (event 102 → 110). State both in plain English — "customers were affected for X minutes/hours."

3. **Run the 5 whys.** Starting from "why did the database CPU spike," ask "why" five times, each answer feeding the next question, ending at a systemic cause — not a person. Follow Lecture 3, §4's worked example structure exactly (each "why" is one sentence, staying on one causal thread, not branching into several).

4. **Name at least three separate contributing factors** that all had to be true at once for this to become an outage rather than a minor blip (consider: the query itself, the release strategy, the timing, the absence of a rollback plan, the absence of a load test — Lecture 2 and Lecture 3 both named these risks in the abstract; here they're the reason a real incident happened).

5. **Write 3–4 post-release action items**, each with a real owner and a real due date, following Lecture 3, §4's rule against "be more careful next time." Insert them into `post_release_actions` with `release_name = 'northlight-bulk-csv-export-2026-02-06'`.

6. **Write the one-paragraph summary you'd actually read aloud** at the start of the review meeting — the version that stays blameless under real pressure, when Diego Ramirez (whose team shipped this) is sitting in the room and probably feeling defensive already.

## Constraints

- **Nowhere in `postmortem.md` may a sentence be structured to imply Diego (or any individual) "should have known better" or "made a mistake."** Every finding must be phrased as a gap in the *system* — the process, the checks that didn't exist, the norms that weren't yet in place — even when a specific person's specific action is part of the timeline. Re-read Lecture 3, §4 on what "blameless" actually means before you write this.
- Your 5 whys must stay on **one** causal thread to a systemic root cause — if you find yourself listing three unrelated causes under one "why," you've branched; pick the thread that actually explains why *this* release, specifically, produced *this* outcome.
- At least one action item must address the **process** gap (no rollback plan existed, no flag existed) directly, not just the specific query bug — fixing only the query prevents this exact incident but not the next one shaped like it.

## Hints

<details>
<summary>On staying blameless under real pressure (Task 6)</summary>

Compare: *"Diego's team shipped a release with an unoptimized query and no rollback plan, causing a two-hour outage"* (names a person, implies fault) versus *"a release without a load test, a staged rollout, or a rollback plan reached production on a Friday afternoon, and none of our current process would have caught any one of those three gaps before it did"* (same facts, systemic framing, and — notice — it's actually a stronger indictment of the process than the blame-framed version is of Diego, because it explains why this could have happened to *any* team, not just his).

</details>

<details>
<summary>On finding real contributing factors, not just the query bug (Task 4)</summary>

The query bug alone explains a slow endpoint. It doesn't fully explain a two-hour, whole-app outage — for that, you also need: no staged rollout (100% of traffic hit the bad query at once instead of a small slice), a Friday afternoon deploy (fewer people available to notice and respond quickly, per Lecture 2 §1's warning), and no rollback plan (the team had to improvise a full redeploy under pressure instead of executing a rehearsed, fast step). Each of those, independently, made the outage worse than it had to be.

</details>

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|----------------|
| Timeline | Prose summary only | Real SQL query with computed durations, actual output pasted |
| Headline numbers | Vague ("it took a while") | Precise, computed, stated in plain English |
| 5 whys | Branches into unrelated causes | Stays on one thread to a real systemic root cause |
| Contributing factors | Names only the query bug | Names 3+ independent factors that each made things worse |
| Blamelessness | Slips into "Diego should have..." language | Every finding survives the constraint — systemic framing throughout |
| Action items | "Be more careful" | Specific, owned, dated, and at least one addresses process, not just the bug |

## Submission

Commit `postmortem.md` and your SQL to your portfolio under `c39-week-10/challenge-02/`.
