# Challenge 1 — Recalibrate a Team Whose Points Have Inflated

**Time:** ~90 minutes. **Difficulty:** Medium–hard. **No single right answer.**

## The scenario

You're consulting for **Team Beacon** — the SSO-integration squad referenced in Week 4's `reference_class_projects` table, a separate four-engineer team elsewhere at Northlight. Their engineering director is worried: "Our velocity chart looks incredible — we've gone from 18 points a sprint to 45 in six sprints, more than doubled, with the exact same four people. But somehow it doesn't *feel* like we're shipping twice as much. What's going on?"

This is one of the most common — and most quietly damaging — failure patterns in story-point estimation: **point inflation**. Nothing about the team's real output changed; the *meaning* of a point drifted. A story that would have been confidently called a "3" in Sprint 1 gets called a "5" by Sprint 4 for the same real-world complexity — often for entirely human, well-intentioned reasons (a team wants its velocity chart to look like it's improving, or new team members never calibrated against the original reference stories and independently invented their own sense of scale). The chart goes up. The real throughput doesn't. And a director reading the chart at face value draws exactly the wrong conclusion.

## The data

```sql
CREATE TABLE beacon_sprint_history (
    sprint_id         INTEGER PRIMARY KEY,
    stories_completed INTEGER NOT NULL,   -- count of distinct shipped items, a size-independent count
    points_completed  INTEGER NOT NULL    -- sum of story points shipped that sprint
);

INSERT INTO beacon_sprint_history VALUES
(1, 6, 18),
(2, 6, 20),
(3, 7, 25),
(4, 6, 32),
(5, 5, 38),
(6, 5, 45);
```

## Your task

1. **Compute the tell.** Write a query for `points_completed / stories_completed` (average points per story) for each sprint. What's the trend, and why is a rising average-points-per-story — with a *flat or falling* story count — the specific signature of point inflation rather than genuine productivity growth?
2. **Rule out the innocent explanations.** Before concluding "inflation," what other honest reasons could explain rising average points per story (e.g., the team deliberately started tackling bigger, more valuable stories; a scope change upstream)? Write one sentence per alternative explanation you can think of, and say what evidence (from this table or elsewhere) would help you tell inflation apart from each one.
3. **Propose a recalibration.** Design a concrete, specific process Team Beacon should run to fix this — not just "be more careful next time." At minimum, address:
   - What existing reference stories (if any) should stay fixed as the calibration anchor, and why anchoring matters (tie this back to Lecture 1).
   - Whether the team should re-point its *entire* remaining backlog against the recalibrated scale, or only apply the fix going forward — and what breaks in the velocity chart if you pick the wrong one.
   - How the director should be told about this without it reading as "the last six sprints of reporting were wrong" (which will land badly) versus "here's what we learned and here's the fix" (which won't).
4. **State the honest cost.** Recalibration isn't free — name at least one real cost (in time, in comparability of historical data, in team morale) of doing this properly, and argue whether it's worth paying anyway.

Put your query, your answers, and your recalibration plan in `challenge-01.md`.

## Hints

<details>
<summary>On telling inflation apart from genuine bigger-story-selection (Q2)</summary>

If the team is genuinely tackling bigger stories, you'd expect the *count* of stories to fall roughly in proportion as their average size rises (fewer, bigger stories, same total real work) — total real output would look flat-to-slightly-up on some independent, size-blind measure (customer-visible features shipped, defect rate, whatever the team can name that points don't measure). If instead the story count barely moves while the points-per-story climbs, and nobody can point to a real change in the kind of work being done, that's the inflation signature: the ruler stretched, the thing being measured didn't.

</details>

<details>
<summary>On the "re-point everything" trap (Q3)</summary>

Re-pointing the whole backlog retroactively fixes the *number* but destroys the team's ability to compare current velocity to historical velocity at all — you'd be comparing a recalibrated ruler to sprints measured with the old one. Most real recalibrations fix the anchor going forward and treat the old chart as "known to be on a different scale before sprint N" rather than silently rewriting history.

</details>

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|----------------|
| Diagnosis | Jumps to "points are inflated" without checking the data | Computes the points-per-story trend and rules out alternatives before concluding |
| Mechanism | Vague ("be more disciplined") | Names a concrete anchoring/recalibration process tied to Lecture 1's reference-story technique |
| Communication | Ignores the director's likely reaction | Addresses how to deliver the news without torching trust in past reporting |
| Honesty | Claims recalibration is free | Names a real cost and argues it's worth paying anyway |

## Submission

Commit `challenge-01.md` to your portfolio under `c39-week-04/challenge-01/`.
