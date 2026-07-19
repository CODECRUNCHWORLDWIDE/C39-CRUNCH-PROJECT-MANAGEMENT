# Challenge 1 — Build a Full Backlog from a Raw Feature Brief

**Time:** ~90 minutes. **Difficulty:** Medium-hard. **No single right answer.**

## The scenario

Elena forwards you a raw feature brief she wrote after a round of customer calls following the Team Workspaces launch. It is, deliberately, exactly what a real brief looks like: informal, a little inconsistent, missing some detail, and mixing "must-have" language with "nice idea" language without labeling which is which. Your job is to turn it into a real, ready backlog — the whole pipeline from this week: elicit what's underspecified, write stories, slice them thin, apply INVEST, and draft acceptance criteria for the top few.

## The brief (as Elena wrote it)

> Subject: notifications — customers keep asking
>
> Hey — talked to five accounts this week and notifications keeps coming up. Right now if someone invites you to a workspace, or comments on a dashboard you're watching, or removes you from a workspace, you just... don't find out unless you happen to check. One customer said they missed being invited to a workspace for two weeks because they never open Insights unless someone tells them to.
>
> Things people asked for (not in priority order, just what I heard):
> - Email me when I'm invited to a workspace
> - Email me when someone comments on a dashboard I'm in
> - Some kind of in-app notification bell/icon thing, like every other app has
> - A way to turn notifications off if you don't want them (one customer got annoyed at a competitor's product for spamming them)
> - Someone asked if they could get a daily digest instead of one email per event — said "I don't want 40 emails a day if my team is active"
> - Someone asked about Slack notifications but that felt like a bigger ask, not sure if it's in scope
> - Also — and I almost forgot this — if you get REMOVED from a workspace you should probably know that happened too, not just silently lose access
>
> No fixed deadline on this yet but Priya mentioned it in the context of the same renewal conversations as the original Workspaces work, so I'd treat it as reasonably important. Can you turn this into something the team can actually plan against?

## Your task

Produce a document, `backlog-from-brief.md`, containing:

1. **Elicitation notes** — list at least 4 open questions this brief leaves genuinely unanswered that you'd want to ask Elena before finalizing stories (e.g., what counts as "someone I'm watching" for a dashboard-comment notification — everyone with access, or only people who've interacted with it? Is Slack explicitly out of scope for this pass, or just uncertain?). For each, state the assumption you're making *for this exercise* so you can keep moving, clearly flagged as an assumption, not a fact.
2. **One epic-level card** summarizing the whole feature area, in the same style as Lecture 1 §6's Atlas epic.
3. **A sliced backlog of 6–10 stories**, each in role–goal–benefit form, none sliced by technical layer. Use at least two different slicing patterns (Lecture 2 §4) and name which you used per story.
4. **INVEST check** on each story (brief — pass/fail per letter is fine, doesn't need to be a full paragraph).
5. **Given/When/Then acceptance criteria** for your top 3 highest-priority stories (your call on which 3 — justify the pick in one sentence each), including at least one edge case per story.
6. **A MoSCoW pass** over the full backlog (Lecture 3 §4) — every story in exactly one bucket, one-sentence justification each.

## Constraints

- Don't silently drop anything from the brief — if you decide something (like Slack notifications) belongs in a later release or a separate epic, say so explicitly rather than just not mentioning it.
- At least one story must exist for "notification preferences / opt-out" — a customer explicitly raised annoyance at a competitor over this; treat it as a real requirement, not an afterthought.
- The "daily digest vs. real-time" tension is a genuine product decision, not something you should silently pick one side of — surface it as an open question or write both as separate, independently prioritizable stories.

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|----------------|
| Elicitation | Jumps straight to writing stories | Names real, specific gaps in the brief before writing anything |
| Slicing | Bundles multiple notification types/channels into one story | Thin, independently valuable stories per channel/event/preference |
| INVEST discipline | Stories pass INVEST by accident | Stories are visibly shaped to pass INVEST, with problem stories caught and re-cut |
| Acceptance criteria | Happy-path only | At least one real edge case per story, each observable |
| MoSCoW reasoning | Buckets assigned with no justification | Each bucket choice ties back to the brief's stated urgency/importance signals |

## Submission

Commit `backlog-from-brief.md` to your portfolio under `c39-week-03/challenge-01/`. Challenge 2 reprioritizes this same backlog under a new constraint, so keep it handy.
