# Exercise 1 — Build a Now/Next/Later Roadmap

**Goal:** Practice turning a raw, sized backlog into an outcome-based now/next/later roadmap — the translation skill from Lecture 1, done by you instead of read about.

**Estimated time:** 45–60 minutes.

## Setup

Re-read Lecture 1, §3–6 and Lecture 2, §2 before starting. Load the `roadmap_items` table from Lecture 2, §2 into PostgreSQL or SQLite. Create a file `roadmap.md` in your portfolio at `c39-week-05/exercise-01/`.

## Tasks

1. **Run the query.** Using the running-sum query from Lecture 2, §4, produce the cumulative-points table for all seven `roadmap_items` rows, ordered by `priority_rank`. Save the query and its output at the top of `roadmap.md` inside a fenced SQL block.

2. **Assign buckets.** Using **114 points of total Q3 capacity** (the same figure Lecture 2, §3 derived), determine which items fall into Now, Next, and Later based on the cumulative total — not by eyeballing it, by reading it off your query's output. State the exact cumulative-point cutoff you used for each boundary.

3. **Write the roadmap in outcome language.** For each theme, write the roadmap the way Lecture 1, §5 modeled: **theme → outcome statement → current best-guess initiative(s).** Do **not** just list initiative titles with a bucket label next to them — that's a feature roadmap, not an outcome roadmap, and Lecture 1 was explicit about why the difference matters. Every outcome statement must be something a non-technical stakeholder could evaluate as true or false without reading the backlog.

4. **Answer the honest-communication question.** In 3–4 sentences, write your own answer to: *"Priya asks when Real-Time Presence Indicators will ship. What do you tell her?"* Your answer must name the bucket, state the actual reason (not a vague "we're busy"), and give a concrete signal for when she'll hear more — following the pattern in Lecture 1, §6. Do not just copy the Dark Mode example from the lecture; Presence Indicators has a different reason (it's tied to Atlas adoption data, not raw capacity competition) — your answer should reflect that difference.

## Expected result (spot checks)

- Cumulative total after item 5 (Onboarding revamp) should be **108** — under 114, with 6 points of buffer.
- Cumulative total after item 6 (Presence Indicators) should be **122** — over 114, confirming it does not fit this quarter.
- Items 1, 2, 3 land in **Now**; items 4, 5 land in **Next**; items 6, 7 land in **Later**.
- Your outcome statements should closely resemble — but need not exactly match the wording of — the ones in Lecture 1, §5. If your outcome statement for any theme is just the initiative title restated ("Ship Advanced Permissions & Roles"), rewrite it — that's the exact mistake §3 of the lecture warned against.

## Done when…

- [ ] `roadmap.md` contains the SQL query and its output.
- [ ] Every theme's entry states an outcome, not just a feature name.
- [ ] Buckets match the spot checks above, with your stated cutoff reasoning shown.
- [ ] Your answer to the Presence Indicators question names a *specific* reason distinct from a generic "we're busy" or "no capacity."

## Stretch

Northlight lands an unplanned but genuinely urgent item mid-quarter: a security patch, 6 points, priority-rank 1 (ahead of everything). Recompute the cumulative table with this item inserted at the top and report which bucket boundary shifts as a result, and by how much.

## Submission

Commit `roadmap.md` to your portfolio under `c39-week-05/exercise-01/`.
