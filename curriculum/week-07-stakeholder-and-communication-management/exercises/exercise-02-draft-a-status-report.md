# Exercise 2 — Draft a One-Page Status Report

**Goal:** Turn raw project facts into a scannable, honest RAG status report using Lecture 2's one-glance format. By the end, writing a status report that doesn't bury bad news feels automatic.

**Estimated time:** 1 hour.

## Setup

You'll need the `status_log` table from the [week README](../README.md) and Lecture 2's RAG definitions (§3) open for reference.

## The scenario

It's Friday, end of Atlas's sprint 8 (the last sprint before GA). Here are this week's raw facts, unedited and unordered — exactly how they'd actually reach you across five different Slack threads and a standup:

- Workspace creation, teammate invites, and shared dashboard viewing all shipped and passed QA this week — ahead of the original sprint 8 forecast.
- The sharing-API sandbox outage (flagged Amber last week, per Lecture 2 §3) was **not** resolved by Wednesday as hoped. Platform now says Friday at the earliest, possibly Monday.
- Because of the above, the comments feature — the last major piece before GA — cannot start integration testing until the sharing API is stable. Marcus estimates comments integration testing needs 3 full days once the API is stable.
- Sprint 8 ends Sunday. GA was targeted for the Monday after sprint 8 ends.
- The 4 days of critical-path buffer identified in Week 6 has been used down to 1 day, because of the extended sandbox outage.
- Budget: burn is currently tracking almost exactly to forecast (within 2%) — no concerns there.
- Security review (a charter constraint, Week 1) is scheduled for the Monday after GA and cannot be moved earlier — it requires the finished build.
- The change request for bulk CSV invite (Lecture 3, §4) was approved this week by Priya, but explicitly scoped for a **post-GA** fast-follow release, not this GA — so it does not affect the current timeline.

## Tasks

Create `status-report.md` in your `exercise-02/` folder.

1. **Determine each RAG value** — overall, scope, schedule, budget — using Lecture 2 §3's observable-criteria table, not a gut feeling. Write one sentence justifying each of the four colors against the specific criteria (not just "seems risky").

2. **Write the one-glance summary block** exactly in the format from Lecture 2, §4: overall + three sub-RAGs, a headline sentence, top risk, decision needed (if any).

3. **Write the full report below the block**, including:
   - What shipped this sprint (the genuinely good news — it belongs in the report, just not at the very top if schedule is at risk).
   - The sharing-API/comments/buffer situation, with real numbers (days of buffer left, days needed for testing).
   - The security-review constraint and why it can't simply be moved.
   - A one-line note on the change request, clarifying it does **not** affect this GA's timeline (a good status report proactively answers the "wait, does the new CSV feature push the date?" question before someone asks it).

4. **Insert your status into `status_log`** as a new row (`status_id = 8`, `report_date = '2026-08-01'` or a date of your choosing in the following week).

## Expected result (spot checks)

- Schedule should land **Red**, not Amber — the buffer that was absorbing the risk is now down to 1 day against a task that needs 3 days once unblocked, which means the committed date is no longer credible under Lecture 2 §3's Red criteria ("mitigation has failed and the committed date is no longer credible"). If you scored this Amber, re-read the Red row of the RAG table against these specific numbers.
- Budget should be **Green** (within 2%, well inside the 5% Green threshold).
- Scope should be **Green** — the one change request that came in this week was explicitly deferred to post-GA, so it doesn't touch this release's scope.
- Overall should be **Red**, because overall RAG takes the worst of its sub-dimensions (Lecture 2, §3's discipline against a Green headline sitting next to a buried Red).

## Done when…

- [ ] All four RAG values are correct and each is justified against the specific numbers in the scenario, not a vibe.
- [ ] The one-glance block leads with the real status — nothing that reads as "on track" appears above the schedule problem.
- [ ] The shipped-features good news is included, but does not appear before the schedule risk in the report's structure.
- [ ] The security-review constraint is explained clearly enough that a reader understands why it can't just move earlier to save time.
- [ ] The change-request note explicitly answers "does this affect the GA date" so the reader doesn't have to ask.
- [ ] Your new row is inserted into `status_log` correctly.

## Stretch

- Write **two** versions of the same report: one shaped for Priya (Manage Closely — full detail, a clear ask about what happens if the API isn't stable by Monday) and one shaped for Jamie (Keep Informed — plain language, no internal jargon, answers "what do I tell the at-risk accounts"). Keep the underlying facts identical; change only framing and depth, per Lecture 2, §2.
- Add a specific, actionable **decision needed** line: given 1 day of buffer and a 3-day testing need if the API comes back Friday, what's the actual choice facing Priya this weekend? State it as a real either/or, not a vague "we'll see."

## Submission

Commit `status-report.md` to your portfolio under `c39-week-07/exercise-02/`.
