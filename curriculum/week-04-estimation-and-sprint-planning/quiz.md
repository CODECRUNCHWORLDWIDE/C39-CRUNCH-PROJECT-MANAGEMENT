# Week 4 — Quiz

Fourteen questions. Lectures closed. Aim for 12/14 before starting Week 5. A mix of multiple-choice and short "what would you compute?" — the answer key at the bottom explains the *why*, not just the letter.

---

**Q1.** Which of the following is the strongest reason hour-based estimates fail on knowledge work?

- A) Hours are too small a unit to be useful
- B) People systematically underestimate duration from an optimistic best-case mental scenario, and experience alone doesn't fix it
- C) Engineers refuse to give hour estimates on principle
- D) Hours can't be added together across a sprint

---

**Q2.** In the modified Fibonacci scale (`1, 2, 3, 5, 8, 13, 20, 40, 100`), why do the gaps between numbers widen as the numbers grow?

- A) It's an arbitrary tradition with no real reasoning behind it
- B) Larger numbers are harder to write on physical poker cards
- C) True precision is achievable at small sizes but not large ones, so the scale stops pretending precision is possible at scale
- D) It matches the Scrum ceremony schedule

---

**Q3.** What specific problem does "everyone reveals their planning-poker vote simultaneously" solve?

- A) It speeds up the meeting
- B) It prevents the first spoken estimate from anchoring everyone else's independent judgment
- C) It's required by the Scrum Guide
- D) It ensures every vote is the same number

---

**Q4.** A poker round comes back `2, 3, 3, 13`. What's the correct next step?

- A) Average the four numbers and record `5.25`
- B) Throw out the `13` as an outlier and use the median of the rest
- C) Ask the low and high voters to explain their reasoning — the spread is signal, not noise
- D) Re-run the vote silently until everyone converges on the same number

---

**Q5.** Kahneman's "planning fallacy" specifically describes:

- A) A one-time mistake anyone can avoid by trying harder next time
- B) A bias that goes away once an estimator has enough direct experience
- C) A systematic tendency to underestimate duration/cost even among experienced estimators, because estimates are built from a best-case mental scenario rather than the real distribution of outcomes
- D) A calculation error unique to inexperienced project managers

---

**Q6.** The difference between the "inside view" and the "outside view" in forecasting is:

- A) Inside view = focus on this specific task's details; outside view = ask how comparable efforts actually performed, historically
- B) Inside view = ask your manager; outside view = ask an external consultant
- C) They're two names for the same technique
- D) Inside view is always more accurate than outside view

---

**Q7.** Atlas's `atlas_story_history` shows `SUM(actual_days) / SUM(ideal_days_est) = 1.32`. What does this number mean?

- A) The team's stories average 1.32 points each
- B) Historically, real delivery has taken 32% longer than the team's own ideal-day estimates
- C) The team should stop using story points
- D) 32% of stories were bugs

---

**Q8.** Why does Lecture 2 recommend cross-checking Atlas's own inside-view overrun factor (1.32) against an outside reference class (1.40) rather than trusting the inside number alone?

- A) The outside number is always more accurate
- B) Sixteen stories across two sprints is a small sample; a second, independent estimate that lands in a similar range increases confidence that the pattern is real, not a fluke of Atlas's specific two sprints
- C) Regulations require an external comparison
- D) The inside number can't be computed in SQL

---

**Q9.** In the sprint capacity formula, why does the meeting-hours deduction happen *before* the focus-factor multiplication?

- A) SQL requires operations in a specific order
- B) Focus factor should apply only to time that was actually available for heads-down work, after meetings are already removed — applying it to gross time would double-count the meeting loss
- C) It doesn't matter; both orders give the same answer
- D) Meetings only happen in the morning

---

**Q10.** A team's `total_focused_days` for a sprint is `28.4`, and the team's own historical actual pace is `1.32` days per point. What's the sprint's **commitment**, in points?

- A) 28.4 × 1.32
- B) 28.4 / 1.32 ≈ 21.5
- C) 28.4 points, flat
- D) 1.32 / 28.4

---

**Q11.** Using the same `28.4` focused days, what's the sprint's **stretch** forecast?

- A) 28.4 / 1.32 ≈ 21.5
- B) 28.4 × 1.32 ≈ 37.5
- C) 28.4 / 1.00 = 28.4
- D) There's no way to compute a stretch number

---

**Q12.** What's the key difference between a sprint **commitment** and a sprint **forecast**?

- A) They're interchangeable terms for the same number
- B) Commitment is built from the team's historical actual pace (conservative, accountable); forecast is the optimistic best case built from the ideal pace
- C) Forecast is always higher-priority work; commitment is lower-priority
- D) Only the Scrum Master can state a forecast

---

**Q13.** A team consistently states its stretch number as if it were the commitment, because it sounds more impressive in planning meetings. What's the most likely consequence?

- A) Nothing — stretch and commitment are the same thing anyway
- B) The team will hit its numbers more often
- C) The sprint will land near its realistic (lower) capacity, which will read as a missed commitment even though it was the accurate number all along — damaging credibility for hitting an achievable target
- D) Story points will automatically recalibrate

---

**Q14.** In Team Beacon's inflation scenario (Challenge 1), `points_completed` rose from 18 to 45 over six sprints while `stories_completed` stayed roughly flat (6, 6, 7, 6, 5, 5). What does this pattern most likely indicate?

- A) The team's real productivity more than doubled
- B) The team should be praised for accelerating
- C) The meaning of a "point" drifted upward over time (point inflation) rather than real output changing — the ruler stretched, not the thing being measured
- D) The team started shipping smaller stories

---

## Answer key

<details>
<summary>Reveal after attempting</summary>

1. **B** — the planning fallacy is structural, not a knowledge gap; more experience alone doesn't neutralize it.
2. **C** — the widening gaps encode "precision is possible here, but not up there" directly into the scale.
3. **B** — simultaneous reveal is the specific mechanism that prevents anchoring; sequencing votes reintroduces it.
4. **C** — a wide spread is the whole point of poker: it surfaces a real disagreement about the story's complexity that needs discussion, not averaging-away.
5. **C** — the planning fallacy specifically survives experience because it comes from how estimates are constructed (best-case scenario), not from lack of practice.
6. **A** — inside view = task-specific reasoning; outside view = empirical comparison to a reference class of similar efforts.
7. **B** — `1.32` is the measured ratio of actual to estimated effort across all 16 historical stories: 32% average overrun.
8. **B** — a second, independent data source landing in a similar range is what gives you confidence the pattern is real and not sample noise from just two sprints.
9. **B** — focus factor represents productivity *within* time that's actually available; applying it before removing meeting time would incorrectly discount meeting time as if it were also subject to a focus-factor loss.
10. **B** — commitment divides focused days by the *actual* historical pace (the conservative, defensible number): `28.4 / 1.32 ≈ 21.5`.
11. **C** — stretch divides by the *ideal* 1.0 pace — the optimistic ceiling if everything went as originally, best-case, imagined.
12. **B** — commitment is grounded in what's actually happened before; forecast is the aspirational upside. Conflating them is the single most damaging mistake this week covers.
13. **C** — stating stretch as commitment sets an unrealistic bar; landing at the real (lower) capacity then looks like a miss, even though it was accurate — a credibility cost for an accurate outcome.
14. **C** — flat story count with rising average points per story, with no independent evidence of genuinely bigger work, is the classic signature of point inflation, not real productivity growth.

</details>

**Scoring:** 12+ → start Week 5. 9–11 → re-read the lecture sections behind your misses. <9 → re-read all three lectures from the top; commitment vs. forecast in particular compounds into every later week's planning work.
