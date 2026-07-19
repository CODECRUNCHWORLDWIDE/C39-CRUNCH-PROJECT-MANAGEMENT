# Mini-Project — Run a Release: DoD Gate, Readiness Checklist, Rollback Plan & Go/No-Go

> Ship the full release package a real release actually needs: a definition of done that gates the work, a readiness checklist that gates the event, a rollback plan written before you need it, a documented go/no-go decision, and a post-release review template ready to fill in the moment the release happens — all backed by live, queryable data, not a static document. This is the week's capstone, and it closes out the Project Atlas case study that's run since Week 1.

**Estimated time:** 3 hours, best done Saturday after the exercises and challenges.

Every artifact in this course so far has led here. The charter (Week 1) named the objective this release finally delivers on. The backlog (Week 3) became the stories that got built. The sprints (Week 4) turned the backlog into working software. The roadmap (Week 5) promised this date. The risk register (Week 6) flagged the exact webhook dependency that's still the live risk today. The stakeholder map (Week 7) named who needs to know what, when. The flow metrics (Week 8) told you whether the team was actually delivering. This week, all of it converges on one afternoon: does the thing you've been managing for nine weeks actually go live, safely, in a way you can defend and recover from if it doesn't go to plan.

---

## Step 1 — Choose your release

Pick **one**:

- **(A) Atlas's real GA**, closing out the case study. Use the actual data from the lectures and Challenge 1 — the `dod_criteria`, `known_defects`, `release_checklist`, and `release_signoffs` rows for `atlas-ga-2026-04-29` already exist in your database. This path asks you to take the go/no-go decision you made in Challenge 1 and carry it all the way through to a full release package, including what you'd do differently based on Challenge 2's postmortem lessons.
- **(B) The real project or brief from your own Week 1 mini-project**, now imagining it's ready to ship. Invent a realistic DoD, checklist, defect list, and rollback plan for it, using everything from Weeks 1–9 you've already built for that project as context (your charter's constraints, your risk register if you built one, your stakeholder map if you built one).

Whichever you pick, name your choice in one sentence at the top of your deliverable.

---

## Step 2 — Finalize the definition of done

In `dod-gate.md`:

1. If you're on path (A): review the three `not_met` DoD rows from the lectures. **Decide, and record, whether each one is now `met`, remains `not_met`, or is explicitly `waived`** — and for any `waived` row, write the one-paragraph justification a skeptical QA lead (Yuki Tanaka) would actually accept, per Lecture 1, §4's ceremony-vs-signal test. Update the rows in `dod_criteria` to match your decision.
2. If you're on path (B): write a full 10–14 line DoD spanning all four categories (Lecture 1, §2), load it into `dod_criteria` with your own `release_name`, and set realistic statuses — not all `met`.
3. Query and paste: the final `not_met` count for your release. If it's not zero, explain in one paragraph why the release can still proceed (or can't).

---

## Step 3 — Finalize the readiness checklist

In `readiness-checklist.md`:

4. If you're on path (A): review the 6 not-`done` checklist items from Lecture 2. Update each one's status to reflect a realistic final state as of the actual GA date (some `done`, at least one still `blocked` or `in_progress` is realistic and fine — a perfect checklist the day of launch is not).
5. If you're on path (B): write a 10–14 item checklist spanning all seven categories (Lecture 2, §1), load it, and set realistic statuses.
6. Query and paste: your release's readiness percentage (Exercise 2, Task 6's query pattern) and a list of everything still not `done`.

---

## Step 4 — Finalize the rollback plan

In `rollback-plan.md`:

7. If you're on path (A): review Atlas's 6-step rollback plan from Lecture 3. Add **at least one additional step or refinement** informed specifically by Challenge 2's postmortem — what would you add to Atlas's plan, having just seen what happens when a rollback isn't rehearsed and a team has to improvise under pressure?
8. If you're on path (B): write a full rollback plan (Lecture 3, §1–2) — trigger conditions, ordered steps with owners and time estimates, verification, and named trigger authority at each cost tier.

---

## Step 5 — Run the go/no-go and record it

In `go-no-go-final.md`:

9. If you're on path (A): this is your **final** decision, informed by everything above — it may match your Challenge 1 answer or differ from it if Steps 2–4 changed the picture. State clearly whether and how it changed.
10. If you're on path (B): run a full go/no-go using Lecture 2, §5's role structure — name real people (your invented stakeholder cast, or Week 7's if you built one for this project) and record each one's decision in `release_signoffs`.
11. Either path: write the final decision (`go` / `no_go` / `conditional_go` with a precise condition) and update `release_signoffs` to reflect it — no row should be left `pending`.

---

## Step 6 — Post-release review template

Even if your release "hasn't happened yet" in-world, prepare the review you'd run the moment it does — this is the artifact that turns a good or bad release into a learning system rather than a one-off event. In `post-release-review-template.md`:

12. Write the **template structure** you'd fill in live during the review — the timeline query (Challenge 2, Task 1's pattern), the headline-numbers calculation, the 5-whys prompt, the contributing-factors prompt, and the blameless-framing reminder — ready to use the moment `release_events` starts filling up with real rows.
13. Pre-populate `release_events` with the **first row only** — the deploy event itself — so the template is genuinely ready to receive the rest live.

---

## Deliverable

A directory in your portfolio `c39-week-10/mini-project/` containing:

1. `dod-gate.md` — final DoD decisions + query results (Step 2).
2. `readiness-checklist.md` — final checklist state + readiness percentage (Step 3).
3. `rollback-plan.md` — final rollback plan, refined by Challenge 2's lessons if on path A (Step 4).
4. `go-no-go-final.md` — final, fully-recorded decision (Step 5).
5. `post-release-review-template.md` — ready-to-use review structure + the deploy event (Step 6).
6. Your SQL (`setup.sql` or inline in the markdown files) showing every `INSERT`/`UPDATE` you ran.

---

## Rubric

| Criterion | Weight | "Great" looks like |
|-----------|------:|--------------------|
| DoD rigor | 15% | Every line specific and checkable; waivers (if any) genuinely justified, not rubber-stamped |
| Checklist completeness | 15% | All relevant categories covered; readiness percentage computed correctly and honestly |
| Rollback plan quality | 20% | Observable triggers, real owners, real time estimates, verification steps; path-A answers show genuine Challenge 2 influence |
| Go/no-go decision | 25% | Every role reasoned through distinctly; final decision precise enough to act on without a follow-up question; no `pending` rows left |
| Post-release review readiness | 15% | Template is genuinely usable, not just described; reminds the user (yourself, later) to stay blameless before the pressure of a real incident makes that hard |
| Data discipline | 10% | Everything lives in queryable SQL tables, correctly scoped by `release_name`; no spreadsheet touches this deliverable |

---

## Stretch goals

- If you're on path (A): write a short `epilogue.md` — three months later, is Team Workspaces still healthy? What would you check in `atlas_pm` to answer that question with data instead of a guess (hint: this is exactly Week 8's flow-metrics toolkit, now applied post-launch)?
- Build one query that joins `dod_criteria`, `release_checklist`, and `known_defects` for your release into a **single readiness dashboard row** — the one query a sponsor would actually want pulled up on screen during the go/no-go meeting.
- If you're on path (B), write the stakeholder map (Week 7) and risk register (Week 6) for your project first, if you haven't already, so this week's release package is grounded in real prior artifacts rather than invented from scratch.

---

## Why this matters

A team can plan brilliantly, build carefully, and communicate honestly for nine straight weeks — and still lose all of it in one badly-run afternoon if the release itself isn't gated, planned, and recoverable. This mini-project is the shape of that afternoon done right: a DoD that actually blocks unfinished work, a checklist that catches what code review can't, a rollback plan that turns a crisis into a procedure, a go/no-go that's a real decision instead of a shrug, and a review that turns whatever happens into something the team is measurably better at next time. Project Atlas closes here — Week 11 shifts to the tools (GitHub Projects, Jira, AI-assisted workflows) that make everything from Weeks 1–10 faster to run at scale.

When done: push, then take the [quiz](../quiz.md) and start Week 11 — Tooling & AI-Assisted PM.
