# Challenge 2 — Reprioritize a Backlog After a Scope Cut

**Time:** ~60 minutes. **Difficulty:** Medium. **No single right answer.**

## The scenario

Two weeks after Challenge 1's notifications backlog was groomed and roughly ordered, Priya calls an unscheduled meeting. Northlight just lost a **different** enterprise deal — not for lack of notifications, but because a competitor demoed **exportable audit logs** (who accessed what, when) during a security review, and Northlight didn't have an answer. Priya has decided: the *next* two sprints of Marcus's team need to make room for a fast-tracked **"who changed my workspace's permissions, and when" audit trail** story, because two more enterprise renewals in the pipeline have compliance teams that will ask the same question.

This isn't a hypothetical exercise in prioritization theory — it's the single most common real-world event in backlog management: **an outside event changes what's urgent, and the backlog has to visibly, defensibly reshuffle in response**, not just silently absorb the new thing on top of everything already committed.

## Your task

Using the backlog you built in Challenge 1 (or a fresh 8-story backlog if you're doing this challenge standalone — note which), produce `reprioritized-backlog.md` containing:

1. **The new story**, written properly: *As an account admin, I want to see a log of who changed a workspace's permissions and when, so that I can answer a compliance/security review without manually reconstructing the history.* Run it through INVEST — is it actually ready to slot in as-is, or does it need slicing first (hint: think about what "changed permissions" actually covers — invites, removals, role changes — per Lecture 2 §4's business-rule-variation pattern)?
2. **A WSJF score** for the new audit-trail story and for your existing top 3 stories from Challenge 1, using the method in Lecture 3 §4 (score business value, time criticality, and risk reduction 1–13-ish each, sum for Cost of Delay, divide by a job-size estimate). Show your numbers, not just a final ranking — a reader should be able to see *why* the audit story does or doesn't jump the queue.
3. **A revised priority order** for the whole backlog (Challenge 1's stories plus the new one), with the audit-trail story placed according to your WSJF result — not automatically first just because Priya is anxious about it, and not automatically last just because it's new. Let the numbers argue the case; if your gut disagrees with your own WSJF output, say so and decide which one you trust more, and why.
4. **A one-paragraph message to Priya** explaining the new order in plain, non-PM-jargon language — what's moving, what's slipping as a result (name at least one specific story that now ships later than originally planned), and why. This is the actual deliverable of a real reprioritization: someone senior needs to understand the trade-off in under a minute, not read your WSJF table.
5. **One sentence on what you'd do differently** if this exact scenario (an outside event forcing an unplanned reprioritization) happens again next month — is there a process gap, or is this just the normal cost of doing business in a live backlog?

## Constraints

- You must **name at least one specific story that slips** as a direct result of the audit-trail story moving up — a reprioritization with no visible trade-off is not a real reprioritization, it's just adding work for free.
- Don't let the audit-trail story skip a Definition-of-Ready check just because it's urgent — urgency changes *order*, not *readiness*. If it's not actually sliced/estimable yet, say so and treat "make it ready" as the fast-tracked item, not the unsliced story itself.
- Your WSJF numbers must be internally consistent with your Challenge 1 MoSCoW buckets — a story you called "Could have" two weeks ago shouldn't suddenly score a WSJF that puts it above your "Must have" items without an explanation of what changed.

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|----------------|
| INVEST discipline under pressure | Slots the urgent story in unsliced/unready | Slices or flags it as not-yet-ready before prioritizing it |
| WSJF rigor | Numbers assigned with no visible reasoning | Each score has a one-clause rationale; the math is checkable |
| Honesty about trade-offs | Claims nothing has to slip | Names a specific casualty and defends the choice |
| Communication | Reproduces the WSJF table for Priya | Translates the decision into a plain-language, senior-readable message |
| Process reflection | Skips or hand-waves the final question | Gives a real, specific answer — process gap named, or a defensible "this is normal" |

## Submission

Commit `reprioritized-backlog.md` to your portfolio under `c39-week-03/challenge-02/`.
