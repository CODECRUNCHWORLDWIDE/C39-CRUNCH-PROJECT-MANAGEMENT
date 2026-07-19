# Exercise 3 — Find the Critical Path

**Goal:** Compute a full forward pass, backward pass, slack table, and critical path for a 9-task plan — first by hand, then confirmed in Python (Lecture 3, §3–6).

**Estimated time:** 60–90 minutes.

## Setup

Re-read Lecture 3, §3–6 (forward pass, backward pass, slack, and the Python script) before starting. Copy the Python script from Lecture 3, §6 into a new file `critical_path.py` as your starting point — you'll adapt it, not rewrite it from scratch. Create your work in `c39-week-06/exercise-03/`.

## The plan

Meridian (the fintech team from Exercises 1–2) is cutting over its transaction-processing service to the new cloud provider. Here is the remaining schedule, in working days:

| Task | Description | Duration | Predecessors |
|---|---|:-:|---|
| P | Provision new cloud environment | 3d | — |
| S | Migrate database schema | 4d | P |
| D | Migrate transaction data | 5d | S |
| M | Set up new monitoring/alerting | 2d | P |
| C | Update CI/CD pipelines | 3d | P |
| L | Run load test at peak volume | 4d | D, M |
| R | Security review sign-off | 3d | C |
| H | Cutover rehearsal (dry run) | 2d | L, R |
| Z | Production cutover | 1d | H |

## Tasks

1. **By hand first**, in a file `by-hand.md`: compute ES/EF for every task (forward pass), then LF/LS for every task (backward pass), then slack for every task. Show your work — the intermediate ES/EF/LF/LS values, not just the final slack numbers — the same way Lecture 3 §3–5 laid it out in tables.
2. State the critical path (as a sequence of task letters) and the total project duration.
3. **Then in Python**, adapt `critical_path.py` to this task list and confirm your by-hand answer matches the script's output exactly. If they don't match, find the discrepancy before moving on — don't just trust whichever number looks more official.
4. Answer in a short paragraph at the bottom of `by-hand.md`: if the load test (L) actually takes 6 days instead of 4, does the critical path change? Does the project duration change, and by how much? (Recompute — don't guess.)

## Expected result

<details>
<summary>Reveal only after you've computed it yourself — this is the answer, not a hint</summary>

**Forward pass (ES/EF):** P: 0/3 · S: 3/7 · D: 7/12 · M: 3/5 · C: 3/6 · L: 12/16 · R: 6/9 · H: 16/18 · Z: 18/19

**Backward pass (LS/LF):** Z: 18/19 · H: 16/18 · L: 12/16 · R: 13/16 · D: 7/12 · M: 10/12 · C: 10/13 · S: 3/7 · P: 0/3

**Slack:** P=0, S=0, D=0, L=0, H=0, Z=0 (all critical) · M=7 · C=7 · R=7

**Critical path:** P → S → D → L → H → Z, **total duration 19 working days**.

**The L-slips-to-6-days question:** yes, the critical path changes. L is already on the critical path with zero slack, so any increase in its duration adds directly to the project duration — the new duration is 21 days, and the path itself stays P → S → D → L → H → Z (same sequence of tasks, just longer). This is a useful contrast with Lecture 3 §5's B/D example, where a slip *changed which tasks* were critical — here, a slip on an *already-critical* task just stretches the same path. Both are real outcomes; knowing which one you're looking at depends on whether the slipping task already had zero slack.

</details>

## Done when…

- [ ] `by-hand.md` shows full ES/EF/LS/LF/slack work for all 9 tasks, not just final answers.
- [ ] Your hand-computed critical path and duration match the expected result above.
- [ ] `critical_path.py` runs and its output matches your by-hand table exactly.
- [ ] The L-slips-to-6-days follow-up is answered with a recomputation, not a guess.

## Stretch

M and C both carry 7 days of slack — the most of any task in this plan. Using Lecture 3 §7's buffering guidance, write two sentences: is 7 days of slack itself ever a risk (hint: think about what happens if a task with a lot of slack keeps getting deprioritized because "there's no rush")? Name the failure mode in your own words.

## Submission

Commit `by-hand.md` and `critical_path.py` to your portfolio under `c39-week-06/exercise-03/`.
