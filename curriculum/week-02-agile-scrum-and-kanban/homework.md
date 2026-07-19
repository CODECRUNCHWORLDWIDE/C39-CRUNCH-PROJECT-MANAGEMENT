# Week 2 — Homework

Extra practice, spread across the week. These are shorter and more varied than the exercises and challenges — some are quick writing prompts, some are SQL reps against `atlas_backlog`, one is a short outside-reading task. Do them spaced out (a couple a day) rather than all at once; several are meant to be done the day *after* you've sat with the relevant lecture, not immediately after.

Keep answers in a single file, `c39-week-02/homework.md`, in your portfolio — one section per numbered item below.

## 1. Values, in one sentence each (Lecture 1)

Without looking back at the lecture, write each of the four Agile values in your own words, in one sentence each. Then check yourself against Lecture 1 §2–5 and note anything you got meaningfully wrong (not just worded differently — actually wrong).

## 2. Spot the misuse (Lecture 1, §7)

Write two short (2–3 sentence) fictional scenarios — different from the one in Lecture 1 — where a team invokes "we're Agile" to justify skipping something they shouldn't skip. Make each one specific (a named team, a named skipped thing) rather than generic.

## 3. Role boundary drill (Lecture 2, §1 and §5)

For each of the following decisions on Project Atlas, name **which Scrum role** owns it, and justify in one clause:

- Whether commenting ships before or after notifications.
- Whether the team can add an eleventh story to a sprint that already has ten committed.
- Whether standup should move from 9:00am to 9:30am.
- Whether the sprint length should change from two weeks to three.
- Whether a specific engineer pairs with another on a hard bug today.

## 4. Query practice — WIP snapshot (Lecture 3, §5a)

Using `atlas_backlog` (before Exercise 2's flood rows, or after — note which), write and run the WIP-per-column query. Then write one sentence interpreting the result the way you'd say it out loud in a standup: not just the numbers, but what they mean for the team's next move.

## 5. Query practice — oldest items (Lecture 3, §5b)

Write a query that returns only the **single oldest** item in each active status column (not the full sorted list — the actual oldest one per column). Report the result. (Hint: this needs either a `GROUP BY` with `MIN()` on the age expression plus a join back to get the full row, or a window function if you're comfortable with one from C33 — either approach is fine this week.)

## 6. Lead time sanity check (Lecture 3, §5c)

By hand (no query), compute the lead time for item 5 (`Shared read-only dashboard view`) using the seed table's `created_date` and `done_date`. Then write and run the SQL query and confirm your hand calculation matches. If it doesn't, find your arithmetic error before moving on — this is a "trust but verify" habit worth building now, not a real project's first time you catch a subtraction mistake.

## 7. Little's Law, applied (Lecture 3, §4)

Atlas's `in_review` column, after Exercise 2's flood, has roughly 8 items in it with an average age of about 4.9 days. If the team's throughput through `in_review` is roughly 1.5 items/day, use Little's Law (`WIP = throughput × cycle time`, rearranged) to estimate the average cycle time an item spends in that column right now. Then explain in 2–3 sentences what would have to change for that number to drop closer to 1 day.

## 8. Ceremony failure-mode matching (Lecture 2, §3)

Match each real quote below to the ceremony it's describing and state whether it's evidence of a **healthy** ceremony or a **failure-mode tell**:

- "We didn't get through everyone's update because two people started debating the notification design mid-meeting."
- "Elena changed the priority of two backlog items after seeing how confused the pilot customer looked using search-by-comment."
- "Nobody could say what we were even trying to accomplish this sprint when I asked on day 8."
- "Dana said she was blocked, and by the end of the day someone had actually answered her question."

## 9. Outside reading — one real postmortem

Find (search) one publicly written retrospective, postmortem, or "lessons learned" document from a real software team (a blog post, a conference talk writeup, an open-source project's retro notes — many teams publish these). In 150–200 words, summarize: what ceremony or practice from this week does it most resemble, and does the write-up show evidence of the retro producing a **real action item**, or does it read like a vent session with no committed change? Cite the source (link or title + author).

## 10. Scrum vs. Kanban, your own team

Think of a team you're part of or have been part of — work, school, a club, a volunteer effort, anything with recurring collaborative work. In 150–200 words: which framework (or hybrid) would actually fit that team, using Lecture 3 §6's dimensions, and is that close to what the team actually does today or a real departure from it?

---

When you're done, move on to the [quiz](./quiz.md).
