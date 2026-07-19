# Challenge 2 — Communicate a Bad Slip

**Time:** ~90 minutes. **Difficulty:** Hard. **No single right answer.**

## The scenario

Bad news, worse than anything Atlas has faced so far. It's Monday morning, the day GA was supposed to happen. Over the weekend:

- The sharing-API sandbox came back up Friday night, later than the "Friday at the earliest" estimate — but when Marcus's team started integration testing Saturday, they found the API's actual behavior didn't match its documentation in a way that breaks permission checks (a user can, in a specific edge case, see a workspace's comments before being formally added as a member). This is a **real security/privacy defect**, not a cosmetic bug — it cannot ship.
- Fixing it properly is a 4–6 day estimate from Marcus, not a quick patch — the permission-check logic needs to move earlier in the request path, which touches code shared with the dashboard-viewing feature that already shipped and is in production.
- This means: GA cannot happen today. The realistic new GA date is **7–10 calendar days out**, and even that has real uncertainty because the fix touches already-shipped code that needs its own regression testing.
- This is now the **second** slip on a date that was already under pressure last week (Lecture 2/Exercise 2's Amber status) — the first time schedule risk was flagged as Amber with a credible mitigation; this time there is no credible way to hold the original date at all.
- Amara has already told two enterprise accounts a specific date. Jamie's Customer Success team has fielded direct questions from account contacts asking "is it still Monday?" this morning.
- Tom Reilly will notice a schedule slip affects the quarter's roadmap commitments he reported to the board three weeks ago.

This is exactly the situation Lecture 2, §5 describes as the hardest test of trust: genuinely bad news, on a date people were already watching closely, after a prior status update that said things were merely "Amber."

## Your task

Produce a communication package in `bad-slip-package.md` with the following, in order:

1. **The internal-first move.** Before any external-facing communication goes out, what do you do first, with whom, and why — per Lecture 3, §3's escalate-vs-handle judgment? Who needs to be aligned before Amara and Jamie hear anything, and what's the risk of skipping that alignment step and going straight to a broad announcement?

2. **A status-log entry** using the `status_log` table format from Lecture 2, with the correct RAG values (justify each against Lecture 2 §3's observable criteria — this is unambiguously Red on schedule; don't soften it) and a headline that states the real situation in one sentence without either panicking or minimizing it.

3. **The message to Priya** (Manage Closely) — full detail: the security defect, the real fix estimate with its uncertainty, the new date range, and a clear ask (per Lecture 3, §1–2's four-part template) about what she needs to decide or approve right now (e.g., authorizing the message to Amara/Jamie, any resourcing decision to speed the fix).

4. **The message to Amara** (Keep Satisfied, and now directly affected because of her account commitments) — this is the hardest one to write. She's about to have to walk back a specific date with real customers. What does she need from you to have that conversation credibly, and how do you deliver genuinely bad news to her without either burying it in caveats or being so blunt it reads as indifferent to the position it puts her in?

5. **The message to Jamie** (Keep Informed) — plain language her team can use directly with customers *today*, given that account contacts are already asking. This needs to be usable within the hour, not a document she has to translate herself.

6. **A one-paragraph reflection**: what, if anything, from Weeks 1–6's practices (the charter's constraints, the Week 6 risk register, last week's Amber status) could have surfaced this specific risk earlier, and what — if anything — genuinely couldn't have been predicted? Be honest here; not every bad outcome was foreseeable, and claiming this should have been caught in the risk register when it's a genuinely novel discovery (undocumented API behavior found only during real integration testing) would be dishonest self-flagellation, not a real reflection.

## Constraints

- **Do not minimize.** No message may use softening language that would let a reader come away thinking this is smaller than it is (Lecture 2, §5's "buries it" failure mode, at higher stakes). This is a genuine, unplanned, meaningful date slip caused by a real defect — say so plainly in every version.
- **Do not panic either.** The messages must stay factual and composed — real numbers, a real plan, a real ask — not alarmist. Trust is lost two ways: hiding bad news, and delivering it in a way that reads as you having lost control of the situation.
- **Every external-facing message (Amara, Jamie) must be something they could act on within the hour**, given that real customers are already asking questions this morning.
- Use the real numbers given in the scenario (4–6 day fix estimate, 7–10 day new GA range) — don't invent a smaller, easier gap.

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|----------------|
| Sequencing | Sends the same broadcast to everyone at once | Aligns internally first, then tailors outward, in a defensible order |
| Honesty | Softens the defect or the timeline uncertainty | States the real defect, the real range, and the real uncertainty plainly |
| Composure | Reads as alarmed or defensive | Reads as factual, in control, with a real plan attached |
| Audience-fit | Same message reused verbatim for Priya, Amara, and Jamie | Each message is shaped for what that specific person needs to do next |
| Actionability | Amara/Jamie have to ask follow-up questions before they can act | Both can act — have a real customer conversation — within the hour |
| Reflection honesty | Claims everything should have been caught, or claims nothing could have | A fair, specific account of what earlier practice could and couldn't have surfaced this |

## Submission

Commit `bad-slip-package.md` to your portfolio under `c39-week-07/challenge-02/`.
