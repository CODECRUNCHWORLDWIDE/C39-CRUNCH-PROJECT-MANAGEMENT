# Challenge 2 — Diagnose a Stalling Team

**Time:** ~90 minutes. **Difficulty:** Medium–hard. **Your conclusion must be defended with queries, not a hunch.**

## The scenario

Sprint 8 planning is next week, and Priya Chen has asked Marcus Diallo one question in three different phrasings over the last two steering meetings: *"is the team getting slower, and if so, whose fault is that?"* Marcus's honest instinct is "no one's fault — we hit two things we flagged as risks back in Week 6" — but "my honest instinct" is not what he's allowed to walk into a room with. He needs the data to say it, specifically, or he needs to be wrong and find out now instead of in front of the VP of Sales.

You have everything you need already sitting in `atlas_pm`: seven sprints of velocity, cycle time, and blocked-time history, plus the actual risk register from Week 6 and the stakeholder cast from Week 7. Your job is to write the queries that either **confirm** Marcus's instinct with numbers, or **overturn** it.

## Your task

Produce `diagnosis.md` answering each question below. Every claim must cite a specific query and its result — "the data shows X" with no query behind it doesn't count.

1. **Is velocity actually declining, or does it just feel that way?** Query the six closed sprints' velocity. Is the trend real, noisy-but-flat, or something in between? Be precise — "it went down" isn't enough; say by how much, and from where to where.

2. **Is cycle time getting longer over time?** Group completed issues' cycle time by sprint and look at the trend. Does it move in the same direction as velocity, the opposite direction, or independently? What would each of those three outcomes imply about the *cause* of the slowdown?

3. **Where specifically is the time going?** Pull every issue that spent time `Blocked`, with the reason (use the `summary` field and your own read of what each issue was blocked *on* — the data doesn't have a literal "blocked reason" column, so you'll need to infer it from context, the way a real PM reading raw tickets has to). Group the blocked issues by what they were blocked on. Do they cluster around one cause, several unrelated causes, or is it random noise across the sprints?

4. **Is it a people problem or a dependency problem?** Compute completed-issue count and average cycle time **per assignee**. Is one specific engineer's work consistently slower than their teammates', or is the slowdown showing up across multiple engineers regardless of who's assigned? This is the query that would settle an uncomfortable room argument — run it honestly even if you're not sure what it'll show.

5. **Does this match what Week 6 predicted?** Without re-reading Week 6's actual risk register file, state from memory (then verify against your own notes if you have them) which two risks that register flagged as "watching closely." Does this week's blocked-time data land on exactly those two things, on different things, or on a mix?

6. **What's Sprint 7 already telling you, mid-sprint?** Sprint 7 isn't over, but it's not silent either — `ATLAS-126` has been sitting `Blocked` for several days as of the snapshot. Query for it specifically. Is it the same root cause as Sprints 4–6, or something new? What would you recommend Marcus say about it in tomorrow's steering meeting, *before* the sprint even ends?

7. **Write the verdict.** In 150–250 words, answer Priya's actual question: is the team getting slower, and if so, whose fault is it? Your answer must be defensible using only the queries above — no hand-waving, no "it's probably just growing pains."

## Constraints

- Every claim in `diagnosis.md` needs a query beside it (paste the SQL or reference a numbered query in an appendix).
- Don't just reproduce Lecture 2's or Lecture 3's exact numbers and stop — those lectures got you partway; this challenge asks the *next* question each of them left open (was blocking one cause or many? one person or several? is it new or a repeat?).
- Resist the urge to write a diagnosis that dodges an uncomfortable but data-supported conclusion. If the data says "one issue's blocked time doesn't match the sandbox/Platform-team pattern," say so — don't force every result to fit a tidy story.

## Hints

<details>
<summary>On question 3 — inferring what an issue was blocked on</summary>

You don't have a `blocked_reason` column, and that's realistic — most real Jira exports don't either; the reason lives in a comment thread, not a field. Read each blocked issue's `summary` (`"Platform-team webhook integration"`, `"Sandbox failover integration test"`, `"Failover to backup feed provider"`...) and use your own judgment to bucket them into two or three categories. State your bucketing rule explicitly in `diagnosis.md` — that's the ambiguous-judgment-call skill from Week 1's SQL course, back again here.

</details>

<details>
<summary>On question 4 — the people-vs-dependency question</summary>

If the slowdown were a specific person struggling, you'd expect *their* cycle times to be worse across the board, on issues unrelated to the sandbox or Platform team too. If it's a dependency problem, you'd expect the slow issues to cluster on *what the issue touched*, not *who touched it* — the same engineer should look fast on unrelated work and slow specifically on the dependency-adjacent work. Run the per-assignee query and then cross-reference it against your question-3 bucketing before concluding either way.

</details>

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|----------------|
| Evidence discipline | Conclusions with no query attached | Every claim traceable to a specific, reproducible query |
| Root-cause reasoning | "The team is slower" and stops there | Distinguishes people-caused from dependency-caused slowdown with data, not assumption |
| Honesty | Forces a tidy narrative even where the data is messy | Flags any result that doesn't fit the expected pattern |
| Forward-looking read | Only analyzes closed sprints | Uses Sprint 7's in-flight data (question 6) to say something actionable *before* the sprint ends |
| Communication | Verdict buried in query output | A clear, defensible 150–250 word answer to the actual question asked |

## Submission

Commit `diagnosis.md` (with your queries, inline or in an appendix) to your portfolio under `c39-week-08/challenge-02/`.
