# Exercise 3 — Facilitate a Retro and Capture Real Actions

**Goal:** Practice the exact skill Lecture 2 §3d names as the retro's whole point — turning raw, messy team sentiment into a small number of specific, owned, checkable action items, instead of a comfortable venting session that changes nothing next sprint.

**Estimated time:** 45–60 minutes.

## Setup

Re-read Lecture 2, §3d (sprint retrospective) before starting. Create a file `sprint-2-retro.md` in your portfolio at `c39-week-02/exercise-03/`.

## Scenario

Sprint 2 just ended. You facilitated a retro using the **Start / Stop / Continue** format (a simple, effective structure: what should the team start doing, stop doing, and continue doing). Below are the raw, unedited sticky notes the team put up during the session — exactly as messy as a real retro produces.

```
- "Standup keeps running 25+ minutes because we debate design decisions live instead of taking them offline"
- "Loved that Elena joined standup Wednesday to unblock the comment-notification question in 2 minutes flat"
- "Item #12 sat in progress for what felt like forever and nobody flagged it as blocked until Thursday"
- "The two-week sprint length still feels right, nobody wants to change it"
- "We keep discovering mid-sprint that a story wasn't as well-refined as we thought going into planning"
- "Pairing on the permissions bug last week was great, we should do that more"
- "Nobody updates the board in real time, so standup is half people saying things the board should already show"
- "Review turnaround is slow — see the WIP flood exercise, this isn't news to anyone at this point"
- "The demo in sprint review went great, Priya was excited about the notification digest"
- "Wes said the standup time is workable but exhausting long-term for him"
```

## Tasks

1. **Sort all ten notes into Start / Stop / Continue.** Some notes are ambiguous or could imply more than one bucket — pick the best fit and note your reasoning in one clause if it's genuinely ambiguous.
2. **Identify the two or three highest-leverage items.** A real retro doesn't try to fix ten things in one sprint — that guarantees none of them actually happen. Pick 2–3 notes (from any bucket) that would have the biggest effect on the team's actual delivery if addressed, and explain why you picked those over the others.
3. **Write a SMART action item for each of your chosen 2–3 issues.** Each action item must have: a specific action (not "communicate better"), an **owner** (a named person from the Atlas team), and a way to check next sprint whether it actually happened. Format:

   > **Action:** [specific behavior change] · **Owner:** [name] · **Check:** [how sprint 3's retro will verify this happened]

4. **Connect one action item back to a concrete Atlas fact.** At least one of your action items should reference something specific from this week's seed data or exercises (e.g., item #12's several days stuck in `in_progress`, or the `in_review` flood from Exercise 2) — not just the retro notes in isolation.

## Expected result (spot checks)

- "Nobody updates the board in real time" and "half people saying things the board should already show" belong in **Stop** (stop treating standup as the board) or **Start** (start updating the board as work happens, live) — either bucket is defensible if justified; putting it in **Continue** is not.
- "Pairing on the permissions bug... we should do that more" belongs in **Start** or **Continue** depending on whether pairing was already a regular practice before that one instance — the note itself doesn't fully resolve this, and saying so is a correct answer.
- "The two-week sprint length still feels right" and "the demo went great" are **Continue** — positive signals about something already working, not problems to fix.
- A strong answer will very likely include an action item addressing the standup running long (tying to Lecture 2's standup failure mode) and one addressing the review bottleneck (tying directly to Exercise 2's flooded `in_review` column) — these are the two notes with the clearest data behind them this week.

## Done when…

- [ ] All ten notes are sorted into Start / Stop / Continue with reasoning for any ambiguous calls.
- [ ] 2–3 action items are written in the exact `Action / Owner / Check` format above, each specific and checkable.
- [ ] At least one action item explicitly references a concrete fact from this week's exercises or seed data, not just the retro notes alone.
- [ ] No action item is a restatement of a problem ("we should communicate better") without a concrete behavior change attached.

## Stretch

Write the one sentence you, as facilitator, would say **out loud in sprint 3's retro** to check whether the standup-length action item actually worked — the literal question you'd ask the team to verify the change stuck, not just "did it get better?"

## Submission

Commit `sprint-2-retro.md` to your portfolio under `c39-week-02/exercise-03/`.
