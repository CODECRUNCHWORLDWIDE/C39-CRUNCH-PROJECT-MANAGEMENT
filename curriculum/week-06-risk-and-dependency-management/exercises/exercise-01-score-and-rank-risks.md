# Exercise 1 — Score and Rank Risks

**Goal:** Get probability × impact scoring (Lecture 1, §3) into your fingers — read a raw risk description, extract the signal, and score it consistently instead of by gut feeling.

**Estimated time:** 45–60 minutes.

## Setup

Re-read Lecture 1, §3 (the scoring table and Atlas's three worked examples) before starting. Make sure your `risks` table exists (see the week's README setup). Create a file `risk-scoring.sql` in your portfolio at `c39-week-06/exercise-01/`.

## Tasks

Below are 12 raw risks from a fictional project, **"Meridian"** — a small fintech team migrating their transaction-processing service to a new cloud provider. Each description contains enough signal to score both **probability** (1–5) and **impact** (1–5) using Lecture 1's scale — read carefully; the words used ("has already happened twice," "would be catastrophic," "no signal yet") are the signal, not decoration.

1. The new cloud provider's SLA has a documented 99.95% uptime history over the past two years, but Meridian has no experience operating on this provider yet.
2. The lead engineer who designed the current transaction-processing service is the only person who fully understands its retry logic, and he has told the team he's interviewing elsewhere.
3. A required compliance certification (PCI-DSS re-attestation) has an externally fixed deadline that, if missed, would legally block Meridian from processing card payments at all.
4. The team's load-testing environment has shown intermittent latency spikes under the new provider twice in the last three weeks, with no root cause identified yet.
5. A vendor contract renewal for a logging tool is due next quarter; if not renewed in time, historical logs older than 30 days would become inaccessible — an inconvenience for audits, not a service outage.
6. The migration plan assumes zero downtime during cutover, but the new provider's own documentation says a similar migration "typically requires a 5–15 minute maintenance window," which the team has not yet acknowledged or planned around.
7. A single engineer is manually running the data-migration script for now; if they're unavailable on cutover day, no one else currently knows how to run it, though the script itself is simple and could be handed off with a day's notice.
8. Currency-conversion rate data currently comes from a free tier of a third-party API with a hard rate limit; Meridian's transaction volume is projected to exceed that limit within the next 60 days at current growth, and this has already been discussed as a known, expected event.
9. There's a small chance (no signal either way, purely theoretical) that a future regulatory change could require additional data residency guarantees the new provider doesn't currently offer, which would force a second migration.
10. The new provider's support team has been slow to respond to two prior test-environment tickets (3+ business days each), and production issues would need much faster response than that.
11. A junior engineer accidentally deleted a non-production test database last month; it was restored from backup within an hour with no data loss and no production impact.
12. The team has not yet load-tested the new provider under Meridian's actual Black-Friday-equivalent peak volume (their busiest processing day of the year), and that peak is 10 weeks away.

For **each** risk, write a full risk statement (cause, uncertain event, consequence — Lecture 1, §2) and score probability and impact 1–5, then insert all 12 into your `risks` table (`category` your best judgment: `technical`, `schedule`, `vendor`, `people`, or `scope`; use `'Meridian Team'` as a placeholder owner where no specific person is named; `status = 'open'`; `date_logged` = today's date).

Then write **one query** that returns all 12 risks ranked worst-first by score.

## Expected result (spot checks)

Exact splits can reasonably vary by ±1 on either axis — scoring is a judgment call anchored by evidence, not a lookup table — but your **relative ranking** should closely match this:

- **Highest score, near the top:** #3 (PCI-DSS deadline) — severe impact (legally blocks payment processing) and the deadline is fixed and external, making the "might miss it" probability meaningful given the team hasn't confirmed readiness. Expect a score in the high teens to 25.
- **Also near the top:** #12 (untested peak volume, 10 weeks out) — moderate-to-high probability (nothing tested yet, deadline approaching) and severe impact if the transaction service falls over on the busiest day of the year.
- **Middle of the pack:** #2 (key-person risk), #4 (latency spikes with no root cause), #6 (unacknowledged maintenance window), #8 (rate-limit — already known/expected, so treat as high probability but only moderate impact since it's foreseen and likely has a workaround).
- **Low, near the bottom:** #1 (good SLA history, just unfamiliarity — low probability), #5 (audit inconvenience, not an outage — low impact), #9 (theoretical, no real signal — lowest probability), #11 (already happened, already fully resolved with no lasting impact — this one is arguably a *closed issue*, not an open risk at all; flag this distinction in a comment in your SQL file).

If your ranking puts #3 or #12 near the bottom, or #9/#11 near the top, re-read Lecture 1 §2–3 before moving on — that signals you're not yet reading probability and impact from the evidence in the description.

## Done when…

- [ ] `risk-scoring.sql` contains 12 `INSERT` statements, each with a real risk statement in `description`, not just the raw prompt text copied verbatim.
- [ ] Every risk has a probability and impact between 1 and 5, chosen from evidence in the description, not arbitrarily.
- [ ] Your `SELECT ... ORDER BY score DESC` query is included and its output roughly matches the spot checks above.
- [ ] You've noted, in a SQL comment, why #11 is arguably not an open risk at all (tie back to Lecture 1 §5's Issues vs. Risks distinction).

## Stretch

Pick the two risks you found hardest to score confidently, and write 2–3 sentences each on what additional information you'd want before finalizing the score — this is exactly the conversation you'd have with a domain owner in a real project (Lecture 1, §3, "score risks with the person who owns the domain").

## Submission

Commit `risk-scoring.sql` to your portfolio under `c39-week-06/exercise-01/`.
