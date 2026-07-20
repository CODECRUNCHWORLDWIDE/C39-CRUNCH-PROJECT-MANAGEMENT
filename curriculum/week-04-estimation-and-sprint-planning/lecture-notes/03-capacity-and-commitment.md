# Lecture 3 — Capacity-Based Sprint Planning

> **Duration:** ~2 hours. **Outcome:** You can compute a sprint's real capacity from a leave calendar, meeting load, and focus factor; convert that capacity into story points using the team's actual historical pace; and plan a sprint that states a commitment and a stretch forecast instead of a single number nobody can defend.

Lecture 1 gave you sizes. Lecture 2 gave you a correction factor and a range. This lecture puts them to work: **how much of the pointed backlog can Sprint 3 actually absorb**, given that Dana is on vacation for three of the ten days and Marcus spends two hours a day in meetings that have nothing to do with typing code? "However much fits" is not a plan. A computed number is.

## 1. Why "we'll just see how much we get through" isn't planning

A team that skips capacity math and just pulls stories "until it feels full" is making the same mistake Lecture 1 warned about with hour estimates: substituting a feeling for a number, then being surprised when the feeling was wrong. It under-serves the team (a habitually light sprint quietly wastes capacity nobody accounted for) and it under-serves the stakeholder (a habitually overloaded sprint slips, and slipping without a stated reason reads as a broken promise instead of an already-known risk). Capacity-based planning replaces the feeling with three real inputs you already have sitting in `team_capacity_calendar`.

## 2. The three inputs, and why each one matters

**Leave days.** The most obvious deduction and the one teams remember: PTO, holidays, a conference. Dana K. is out 3 of Sprint 3's 10 working days — that's not "Dana working a little less," it's 30% of her sprint gone before a single line of code.

**Meeting overhead.** The less obvious one, and the one teams *systematically* underestimate, which is exactly why it belongs in a formula instead of memory. Every recurring meeting outside the Scrum ceremonies themselves — 1:1s, cross-team syncs, architecture reviews, on-call handoffs — eats into the working day before "focused work" even starts. Marcus spends 2 hours a day in meetings; on an 8-hour day, that's a quarter of every single day, every day of the sprint, gone before he opens an editor.

**Focus factor.** The most-skipped input, and arguably the most important. Even the hours that remain after leave and meetings aren't 100% heads-down productive time — context-switching, code review for teammates, Slack interruptions, and the simple cognitive cost of picking a task back up all eat real time that a naive calculation would count as "available." A **focus factor** (a fraction, typically 0.6–0.85 for engineers doing real feature work) accounts for this honestly instead of pretending every remaining hour converts 1:1 into shipped work.

## 3. The formula

For each person, in order:

```
available_days  = working_days − leave_days
meeting_days    = available_days × (meeting_hours_per_day / 8)
remaining_days  = available_days − meeting_days
focused_days    = remaining_days × focus_factor
```

```mermaid
flowchart LR
  A["Working days"] --> B["Subtract leave days"]
  B --> C["Available days"]
  C --> D["Subtract meeting days"]
  D --> E["Remaining days"]
  E --> F["Multiply by focus factor"]
  F --> G["Focused days"]
```
*Each person's real capacity, whittled down in order from calendar days to heads-down engineering days.*

As one SQL query against `team_capacity_calendar` for Sprint 3:

```sql
SELECT
    person,
    role,
    working_days - leave_days                                          AS available_days,
    ROUND((working_days - leave_days) * (meeting_hours_per_day / 8.0), 2) AS meeting_days,
    ROUND(
        (working_days - leave_days) * (1 - meeting_hours_per_day / 8.0) * focus_factor,
        2
    ) AS focused_days
FROM team_capacity_calendar
WHERE sprint_id = 3
ORDER BY person;
```

| person | role | available_days | meeting_days | focused_days |
|---|---|---:|---:|---:|
| Marcus Webb | Tech Lead | 10 | 2.50 | 4.50 |
| Priya N. | Senior Engineer | 9 | 1.13 | 6.30 |
| Sam O. | Engineer | 10 | 1.25 | 7.00 |
| Dana K. | Engineer | 7 | 0.88 | 4.90 |
| Wes T. | Engineer | 10 | 1.88 | 5.69 |

```sql
SELECT ROUND(SUM((working_days - leave_days) * (1 - meeting_hours_per_day / 8.0) * focus_factor), 1)
       AS total_focused_days
FROM team_capacity_calendar
WHERE sprint_id = 3;
```

`total_focused_days = 28.4`. **That's Sprint 3's real capacity, in engineer-days of actual heads-down work — not the naive 5 people × 10 days = 50 person-days the calendar alone would suggest.** More than 40% of the naive number was never available in the first place, and no team meeting to "commit to the sprint" would have surfaced that without this calculation.

## 4. Converting focused days into points — two ways, on purpose

This is where Lectures 1–3 connect. You don't convert focused-days to points using the team's *aspirational* "1 point ≈ 1 ideal day" convention — that's exactly the inside-view number Lecture 2 showed overruns by 32%. You convert using the team's **actual measured pace**: `1.32` actual focused-days per point, computed straight from `atlas_story_history`.

```
Commitment (points)  = total_focused_days / actual_pace   = 28.4 / 1.32 ≈ 21.5
Stretch (points)     = total_focused_days / ideal_pace    = 28.4 / 1.00 = 28.4
```

**Commitment ≈ 21–22 points.** This is the number built from what actually happened historically — the number you can stand behind in a sprint review if someone asks "why did you commit to only 21?"

**Stretch ≈ 28 points.** This is the optimistic ceiling — what the team would deliver *if* this sprint ran the way every individual estimate silently assumed it would, with no interruption, no surprise, no context-switch cost beyond what's already budgeted. It's not a fantasy number — it's the real mathematical upper bound of the capacity you've computed — but history says it's the exception, not the default.

**Sanity check against real history:** Sprint 1 delivered 28 points (close to today's stretch number — and Sprint 1 *did* run close to plan). Sprint 2 delivered 22 points (extremely close to today's commitment number of 21.5 — and Sprint 2 *did* run into an unplanned bug that ate capacity, exactly the kind of thing the 1.32 correction factor is built to price in). **The capacity formula isn't inventing a number — it's reproducing the pattern the team has already lived twice.** That's what makes it trustworthy instead of just plausible-sounding.

## 5. Commitment vs. forecast — the vocabulary that matters to a stakeholder

These two words are doing real work, and conflating them is one of the most common (and most damaging) sprint-planning mistakes:

| | Commitment | Forecast |
|---|---|---|
| **What it is** | What the team is accountable for delivering | The team's best-case, upside estimate |
| **Built from** | Capacity ÷ actual historical pace (the conservative number) | Capacity ÷ ideal/aspirational pace (the optimistic number) |
| **What a miss means** | Something genuinely went wrong — worth a retro item | Not hitting it is normal, even expected, most sprints |
| **How to say it to Elena/Priya** | "We will deliver these 21 points." | "If things go unusually smoothly, we might also get to these additional 6–7 points — don't plan around it." |

The single most common failure pattern here: a team states its **stretch** number as if it were the **commitment**, because 28 sounds more impressive than 21 in a planning meeting. Then the sprint runs the way sprints actually run (some meeting overhead, a code review that takes longer than hoped, a small unplanned bug), lands at 22, and it *reads* as a missed commitment — even though 22 was always the realistic number. Stating both, and being explicit about which is which, protects the team from a credibility hit for hitting a number that was accurate all along.

## 6. Planning the sprint: pulling a groomed backlog against the commitment

With capacity in hand, sprint planning is now a straightforward pull, highest priority first, against the commitment number — not the stretch number, which protects the sprint from being loaded past what the team can defend:

```sql
-- Assume sprint3_candidates.story_points has been filled in by Exercise 1's poker session.
-- Pull items in priority order until the running total would exceed the 21-point commitment.
WITH ranked AS (
    SELECT
        item_id, title, story_points, priority_rank,
        SUM(story_points) OVER (ORDER BY priority_rank
                                 ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
    FROM sprint3_candidates
    WHERE story_points IS NOT NULL
)
SELECT item_id, title, story_points, running_total
FROM ranked
WHERE running_total <= 21
ORDER BY priority_rank;
```

Everything the query returns is the **committed** sprint. The next item or two past the 21-point cutoff, if their combined size stays under the stretch number, becomes the explicitly-named **stretch scope** — pulled only if the committed items finish early, never treated as owed.

## 7. Protecting the number once the sprint starts

A computed capacity is only useful if the team actually protects it during the sprint — the two most common ways it gets silently violated:

- **Mid-sprint scope creep.** A "quick" unplanned request gets added without anything coming off the board. This is exactly what happened to Sprint 2 (the `+` address invite bug) — the difference between Sprint 2 and a well-run sprint isn't that interruptions never happen (they will), it's whether the team explicitly re-plans around them (swap something out, or knowingly accept the overrun) instead of just absorbing the hit silently and hoping the number still works out.
- **Confusing "capacity used" with "hours worked."** A team that blows past its focus-factor budget by working late isn't proving the capacity number wrong — it's borrowing against next sprint's energy and setting a precedent that "estimates don't need to be right, people will just work harder." That erodes the entire point of measuring capacity honestly in the first place.

## 8. Check yourself

- Walk through the four-step capacity formula from memory, for one person, using made-up numbers.
- Why does the meeting-overhead deduction happen *before* the focus-factor multiplication, not after? *(Hint: focus factor should apply only to time that was actually available for focused work.)*
- Why does this lecture insist on dividing by the team's *actual* pace (1.32 days/point) for the commitment, rather than the *ideal* pace (1.0 days/point) the team used when it first estimated?
- In your own words, what's the difference between a commitment and a forecast, and why does stating both protect the team's credibility rather than undermine it?
- Sprint 2's real outcome (22 points, 2 days late) sits almost exactly at this lecture's computed Sprint 3 commitment (21.5 points). Is that a coincidence? Why or why not?

If those are automatic, you're ready for the exercises — pointing the real Sprint 3 candidates, computing this exact capacity yourself from the seed table, and turning the result into a stated range.

## Further reading

- **Scrum.org — "What is Velocity in Scrum?":** <https://www.scrum.org/resources/what-velocity-scrum>
- **Mountain Goat Software — "Focus Factor":** <https://www.mountaingoatsoftware.com/blog/focus-factor>
- **Atlassian — "Sprint Planning":** <https://www.atlassian.com/agile/scrum/sprint-planning>
- **PostgreSQL — Window Functions:** <https://www.postgresql.org/docs/current/tutorial-window.html>
