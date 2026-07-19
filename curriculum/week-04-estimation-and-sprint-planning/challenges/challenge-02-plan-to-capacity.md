# Challenge 2 — Plan a Two-Sprint Delivery to a Fixed Capacity

**Time:** ~90 minutes. **Difficulty:** Medium–hard. **No single right answer.**

## The scenario

Elena has a real question: **"Can we ship notification preferences, the digest email, and the CSV export in the next two sprints?"** She's naming three specific items from `sprint3_candidates` (#22, #23, #30) because a customer asked about them directly. Your job is to answer her with a plan grounded in the capacity numbers this week taught you to compute — not a guess, and not a vague "probably."

This challenge assumes you've completed Exercise 1 (all 10 candidates pointed) and Exercise 2 (Sprint 3 capacity computed, plus the Sprint 4 stretch goal that gave you Sprint 4's roster and capacity). If you skipped the Exercise 2 stretch goal, do it now — Challenge 2 needs Sprint 4's numbers.

## Your task

Produce a written two-sprint plan in `challenge-02.md`, backed by real queries, covering:

1. **State both sprints' capacity.** Commitment and stretch, for Sprint 3 and Sprint 4 separately, each traced to a query (not copied from the lecture — recomputed from your own Exercise 1/2 numbers, which may differ slightly from the lecture's worked example depending on your Exercise 1 point choices).
2. **Build the two-sprint plan.** Using your own `sprint3_candidates.story_points` (from Exercise 1) and priority order, decide which items land in Sprint 3's commitment, which spill to Sprint 3's stretch or Sprint 4, and which don't make either sprint. Show the running-total query you used (Lecture 3, section 6, has the pattern).
3. **Answer Elena's actual question.** Do #22, #23, and #30 fit within the two-sprint **commitment** total? Within the **stretch** total? State clearly which of the three, if any, you'd tell Elena is at risk, and why — referencing priority order (they may be pointed low but ranked behind higher-priority items that eat the commitment first).
4. **Name the single biggest risk to this plan**, using this week's vocabulary — not a generic risk like "requirements might change," but something specific to *this* capacity model: e.g., what happens to the whole plan if Dana K.'s 3 leave days in Sprint 3 turn into 5 (a family emergency, say)? Recompute the affected sprint's capacity under that scenario and say what you'd cut first.
5. **Decide what to tell Elena about the spike (#27).** A time-boxed research spike doesn't deliver a shippable feature — should its points count against the sprint's delivery commitment the same way a story's do? Defend your answer; there's a real argument either way and Lecture 1's exercise flagged this same tension.

## Constraints

- Use your **own** Exercise 1 point values and Exercise 2 capacity numbers — don't just copy the lecture's worked example. If your Exercise 1 pointing was more conservative or more aggressive than the lecture's, your plan should reflect that honestly.
- The plan must state a **commitment** and a **stretch** for each sprint separately, using Lecture 3's vocabulary precisely — a submission that blends the two into one number hasn't actually engaged with the week's core distinction.
- Every claim about what fits or doesn't must be backed by a query result you show, not asserted from memory.

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|----------------|
| Grounding | Numbers asserted, not queried | Every capacity and fit claim traces to a shown query |
| Vocabulary | "We'll probably get it all done" | Explicit commitment vs. stretch stated per sprint |
| Elena's question | Yes/no with no reasoning | Names exactly which item(s) are at risk and why, tied to priority order |
| Risk analysis | Generic ("things could go wrong") | A specific, recomputed what-if grounded in the actual capacity formula |
| Spike judgment | Ducks the question | Takes a real position on counting spike points against delivery capacity, and defends it |

## Submission

Commit `challenge-02.md` to your portfolio under `c39-week-04/challenge-02/`.
