# Challenge 2 — Untangle a Blocked Cross-Team Dependency Chain

**Time:** ~90 minutes. **Difficulty:** Medium-hard. **No single right answer.**

## The scenario

It's Sprint 6 of Atlas's eight-sprint plan. You (the PM) go looking for the avatar-stack UI component from Lecture 2 (needed by 2026-07-27) and discover it is nowhere close to ready. Digging in, here's the actual chain you uncover, hop by hop:

1. **Design Systems** hasn't started the avatar-stack component because it needs final approval on the new avatar illustration style — three new default avatar icons, replacing the current placeholder set.
2. Those illustrations are sitting in the **Brand team's** review queue. Brand's designer says she can't sign off yet because the illustrations use a new color treatment that hasn't been checked against Northlight's refreshed brand guidelines, which are still being finalized.
3. The brand guidelines refresh is itself waiting on **Legal**, who needs to confirm the new secondary brand color doesn't conflict with a registered trademark held by an unrelated company in a different industry — a routine check, but Legal's trademark reviewer is currently out on leave and the team hasn't reassigned the queue.
4. Nobody on the Brand or Legal side knows Atlas exists, has a launch date, or is waiting on any of this. As far as they're concerned, this is routine, low-urgency brand housekeeping.

Atlas's presence UI (task D in Lecture 3's schedule) cannot ship its final polish without the avatar-stack component, and Atlas's launch is 6 weeks out. You only discovered this chain because you asked Design Systems directly this week — nobody proactively told you.

## Your task

Produce `dependency-rescue.md` (plus a SQL artifact) containing:

1. **Map the full chain.** Using this week's `dependencies` table schema (extend it with a `blocked_by_dependency_id` self-referencing column if you need to represent the chain explicitly — justify the choice), insert all four hops as rows, and write one query that would surface this entire chain starting from Atlas's need. This is the artifact that should have existed *before* Sprint 6 — build it now, but reflect in your write-up on when it should have been built.

2. **Diagnose, using Lecture 2's language.** In 1–2 paragraphs: why did this chain stay invisible for five sprints? Point to a specific gap (whose job was it to trace dependencies more than one hop deep? was it anyone's?) rather than a vague "communication breakdown."

3. **Pick a real unblocking move**, using Lecture 2 §4's three techniques (resequence, build a fallback, own the interface) — or a combination. For each hop in the chain, decide: escalate directly, work around it, or absorb the wait — and justify each call using what you actually know about the hop (is it truly on the critical path? does Atlas actually need the *final* polished asset by launch, or would a placeholder work for the initial release?). Reference Lecture 3's critical-path thinking if it helps you decide what's actually urgent versus merely blocked.

4. **Write the actual escalation.** A short, real message (3–5 sentences, in `escalation-email.md`) you would send — and to whom, specifically (not "the Brand team," but which role: their manager? the Legal reviewer's manager, given the original reviewer is out?) — that gets this chain moving without being either passive ("just checking in!") or needlessly alarming to teams who had no idea they were on a dependency chain for a launch they've never heard of.

5. **One process fix** you'd put in place so a launch-blocking, four-hop chain doesn't stay invisible for five sprints on the *next* Atlas-sized initiative — concrete and specific, not "communicate more."

## Constraints

- You cannot skip a hop — Legal being out on leave is a real fact of the scenario; your plan has to work with it, not assume it away.
- At least one of your four hop-level decisions must be a genuine **fallback/workaround**, not an escalation — practice the "don't just chase harder" move from Lecture 2 §4b, since chasing a Legal reviewer who is literally out of office won't make them faster.
- Don't invent a fixed Atlas launch date beyond "6 weeks out" — if your plan needs a specific date, state what you'd need confirmed and from whom.

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|----------------|
| Chain mapping | Prose description only | A real, queryable SQL representation of all four hops |
| Diagnosis | "Communication was bad" | Names a specific ownership gap — nobody's job was to trace beyond hop one |
| Unblocking moves | Escalate everything, or work around everything | A mix, each justified against what's actually true about that specific hop |
| Escalation message | Generic "please prioritize this" | Specific, addressed to the right role, proportionate — not alarming teams who didn't create the problem |
| Process fix | Vague ("better tracking") | Concrete and would plausibly have surfaced this by Sprint 2, not Sprint 6 |

## Submission

Commit `dependency-rescue.md`, your SQL artifact, and `escalation-email.md` to your portfolio under `c39-week-06/challenge-02/`.
