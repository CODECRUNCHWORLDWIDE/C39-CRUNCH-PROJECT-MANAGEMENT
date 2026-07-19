# Challenge 2 — Re-resource a Portfolio After a Headcount Cut

**Time:** ~90 minutes. **Difficulty:** Medium-high. **No single right answer.**

## The scenario

Tom Reilly (CFO) has approved Atlas's Challenge 1 overage — with a condition. Effective **January 1**, Northlight is cutting discretionary and contractor labor spend across the whole portfolio by 20%. Two concrete consequences land on your desk the same afternoon, non-negotiable:

1. **Yuki Tanaka's contract will not be renewed past December 31.** Her 30h/week of frontend capacity is gone from January 1 — not reduced, gone.
2. **Elena Cruz is being pulled onto a new Q1 company priority.** She can give **no more than 50% of her total capacity, combined, to Atlas and Ledger Redesign together** — down from the roughly 90–110% she's been running across both this fall.

Everything else about the portfolio's obligations is unchanged: Atlas has real remaining scope bleeding into January (Challenge 1's forecast made that concrete), Ledger Redesign is still active, and Sofia Reyes's own team (Platform Core) still needs at least **60%** of her time — that floor is not yours to negotiate away; it belongs to her own manager, not to Atlas.

Your job: build a real **January `allocations` plan** for the seven people who remain, across the three active projects, that actually fits inside each person's real capacity — and be honest, in writing, about what doesn't fit and what you're recommending get cut, delayed, or accepted as risk instead.

## The remaining roster (January)

| Person | Weekly capacity | December's typical load (context) |
|---|---:|---|
| Marcus Webb (Tech Lead) | 40h | 100% Atlas |
| Priya N. (Senior Engineer) | 40h | 100% Atlas |
| Sam O. (Engineer) | 40h | 100% Atlas |
| Dana K. (Engineer) | 40h | 100% Atlas (was 70% in Nov for planned leave, back to full) |
| Wes T. (Engineer) | 40h | 100% Atlas |
| Elena Cruz (Product Owner) | 40h | **Capped at 50% combined**, Atlas + Ledger Redesign |
| Sofia Reyes (Platform Team Lead) | 40h | **Floor of 60%** must stay on Platform Core |

*(Yuki Tanaka is gone. PM/You is assumed to continue at your existing near-full-time split across Atlas and portfolio coordination — not part of the reallocation puzzle, but note your own time in your final plan for completeness.)*

## Tasks

1. **Quantify the gap first.** Before touching allocations, state in numbers: how many frontend-capable hours per week did Atlas lose with Yuki gone (30h/week × her January allocation, which was planned at 100%)? How many combined Atlas + Ledger Redesign hours per week does Elena's new cap remove, relative to her typical fall load? Put both numbers in a short table.

2. **Build the January `allocations` rows** (SQL `INSERT`, or an equivalent pandas DataFrame — your choice) for all 7 remaining people across `Atlas`, `Ledger Redesign`, and `Platform Core`, for `month_start = '2026-01-01'`. Every person's total `allocated_pct` across projects must be **≤ 100%**, Sofia's Platform Core row must be **≥ 60%**, and Elena's Atlas + Ledger Redesign rows must sum to **≤ 50%**.

3. **Decide who — if anyone — absorbs frontend work.** Yuki's gap doesn't have to be filled by a new frontend specialist (there isn't one). State explicitly: does an existing engineer flex onto frontend work part-time, does frontend work get deprioritized/descoped for January, or is there a third option? Whatever you choose, it must show up as a real change in your Task 2 allocations, not just a sentence.

4. **Run your own over-allocation check.** Using Lecture 3's SQL or pandas pattern, verify your own January plan has **zero** person-months over 100%, and zero months where Sofia dips below her 60% Platform Core floor. Paste the query/code and its (clean) output as proof.

5. **Write the memo.** In `reallocation-memo.md` (250–350 words), addressed to Priya Chen, cover: what you cut or delayed to make the plan fit, what risk you're accepting instead (be specific — "some risk to the timeline" is not specific; "frontend polish on the notifications panel slips to February" is), and one thing you need from her (a decision, a resource, or a scope call) to make the plan work.

## Constraints

- You cannot invent a new person or restore Yuki's hours. The cut is real and non-negotiable in this scenario.
- Sofia's 60% Platform Core floor and Elena's 50% combined cap are hard constraints, not starting points to negotiate down in your own plan — the negotiation already happened before this challenge starts.
- If the math genuinely doesn't fit — if Atlas, Ledger Redesign, and Platform Core's combined *needs* exceed what 7 people can deliver even at full, respectful allocation — say so plainly in the memo instead of forcing a plan that pretends it fits. That's a legitimate, even likely, outcome of a real 20% cut, and naming it honestly is a stronger answer than fudging the numbers.

## Hints

<details>
<summary>On quantifying the gap (Task 1)</summary>

Yuki was planned at 100% of her 30h/week contract for January — that's the full 30h/week gone, not 40h (she was never a 40-hour person; don't overstate the loss). Elena's fall pattern ran as high as 110% combined (October) and as low as 90% (September); a reasonable baseline for "typical" is closer to 90–100% combined, so her new 50% cap removes somewhere in the 40–50 percentage-point range of her *combined* Atlas + Ledger time — translate that into hours using her 40h/week capacity.

</details>

<details>
<summary>On "does the math even fit" (Constraints)</summary>

Seven people, one of whom (Sofia) is capped at 40% max for Atlas-side work (100% − her 60% Platform Core floor) and one of whom (Elena) is capped at 50% combined across two projects, is genuinely tight against three active projects. A strong submission doesn't need to make everything fit — it needs to show the arithmetic honestly and make a clear, defensible call about what gives.

</details>

## How success is judged

| Signal | Weak answer | Strong answer |
|---|---|---|
| Quantifying the gap | Skips straight to a plan | Task 1's numbers are stated clearly before any reallocation happens |
| Respecting hard constraints | Plan quietly lets Sofia dip below 60% or Elena above 50% "just this once" | Every row in the January plan genuinely respects both floors/caps |
| Frontend gap | Ignored or hand-waved | A specific, visible decision (flex, descope, or named alternative) shows up in the actual allocations |
| Verification | No check, or a check that doesn't match the claimed plan | Task 4's query/code output proves the plan is valid, not just asserts it |
| Honesty about fit | Forces the numbers to look fine | States plainly, with numbers, if and where the portfolio's needs exceed what 7 people can deliver |

## Submission

Commit `january-allocations.sql` (or `.py`), the Task 4 verification output, and `reallocation-memo.md` to your portfolio under `c39-week-09/challenge-02/`.
