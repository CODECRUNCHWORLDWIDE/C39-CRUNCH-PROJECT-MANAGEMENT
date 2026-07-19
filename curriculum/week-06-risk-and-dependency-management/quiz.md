# Week 6 — Quiz

Fourteen questions. Lectures closed. Aim for 12/14 before starting Week 7. A mix of multiple-choice and short scenario questions — the answer key at the bottom explains the *why*, not just the letter.

---

**Q1.** Per Lecture 1, what are the three required parts of a well-formed risk statement?

- A) A title, a category, and an owner
- B) A cause, an uncertain event, and a consequence
- C) A probability, an impact, and a score
- D) A date logged, a status, and a response type

---

**Q2.** "Pulse.io's sandbox has been down for 6 of the last 10 days" is best classified, per Lecture 1 §5, as:

- A) Still a risk — it hasn't affected production yet
- B) An assumption
- C) An issue — it has already happened and needs an active response, not a probability estimate
- D) A dependency

---

**Q3.** Two risks both score 12 (probability × impact). Risk X is 4×3; Risk Y is 6×2. What's wrong with Risk Y as stated?

- A) Nothing — 6×2 is a valid score
- B) Probability and impact are both scored on a 1–5 scale, so "6" is not a valid input per Lecture 1 §3
- C) Risk Y should always be treated as more urgent than Risk X
- D) The multiplication should be addition instead

---

**Q4.** Per Lecture 1 §4, which response strategy changes *who bears the cost* if a risk occurs, without reducing the chance it happens?

- A) Avoid
- B) Mitigate
- C) Transfer
- D) Accept

---

**Q5.** A PM logs a risk as "accept" with no written reason and no plan. Per Lecture 1 §4, what's the problem?

- A) Accept is never a valid response
- B) An undocumented accept with no stated reason is indistinguishable from simply ignoring the risk — accept must still be a conscious, written decision
- C) Only sponsors are allowed to choose "accept"
- D) Accept can only be used for risks scored below 4

---

**Q6.** What is the key difference between an internal dependency and a cross-team dependency, per Lecture 2 §1?

- A) Internal dependencies are always more dangerous
- B) Internal dependencies live inside one team and are typically fixed by better sequencing/communication; cross-team dependencies require negotiation because you don't control the other team's schedule
- C) Cross-team dependencies never need to be tracked
- D) There is no meaningful difference

---

**Q7.** Per Lecture 2 §3, why does a cross-team dependency typically cost more than its raw task duration suggests?

- A) Other teams always work slower than your own
- B) Discovery cost, alignment cost, interface risk, and schedule coupling all add real time that neither team's individual estimate captures
- C) Cross-team work is billed at a different rate
- D) It doesn't — raw duration is always an accurate estimate

---

**Q8.** Which of Lecture 2 §4's three coupling-reduction moves is illustrated by: "if Design Systems slips, our engineers build a minimal unstyled version ourselves and swap in the polished component later"?

- A) Resequence
- B) Build a fallback
- C) Own the interface
- D) Escalate harder

---

**Q9.** Why are dependency *chains* (Lecture 2 §5) more dangerous than single dependencies?

- A) Chains always involve more total work
- B) A delay several hops away, in a team that may not know your project exists or has a date on the line, is invisible by default unless someone traces the whole chain explicitly
- C) Chains are illegal to track in most organizations
- D) Chains only occur in software projects

---

**Q10.** In Lecture 3's Atlas schedule, task F (QA) depends on tasks C, D, and E. Why is F's earliest start (ES) equal to the *maximum* of C, D, and E's earliest finish times, not the average or the first one to finish?

- A) Averaging is mathematically simpler
- B) F cannot start until every one of its predecessors is done, so it's gated by whichever one finishes last
- C) The maximum is used only for tasks with exactly three predecessors
- D) Earliest finish times are always equal for parallel tasks

---

**Q11.** What does it mean for a task to have zero slack, per Lecture 3 §5?

- A) The task has the longest duration in the plan
- B) The task is the most expensive in the plan
- C) Any slip in that task's duration delays the project's finish date by the same amount — it's on the critical path
- D) The task has no assigned owner yet

---

**Q12.** In Lecture 3's worked example, task E (Pulse.io integration) carries Lecture 1's highest logged risk score but sits on 9 days of slack. What does Lecture 3 §5 conclude from this?

- A) The risk should be removed from the register since it doesn't threaten the schedule at all
- B) The risk isn't today's most urgent schedule threat, but it's not safe to ignore — if it consumes all 9 days of slack, it becomes critical
- C) Slack calculations override risk scores entirely and should replace them
- D) Task E should be deleted from the schedule

---

**Q13.** Per Lecture 3 §7, where should schedule buffer generally be placed?

- A) Spread evenly across every task in the plan
- B) Only on tasks with the longest duration, regardless of slack
- C) On or right before critical-path tasks that also carry real risk — not on tasks that already have slack
- D) Buffer should never be used; it's always considered padding

---

**Q14.** Per this week's data-tooling rule and Lecture 1 §7, what is the strongest reason a live risk register belongs in SQL rather than a shared spreadsheet?

- A) SQL is a newer technology than spreadsheets
- B) Spreadsheets can't display colors
- C) Concurrent multi-owner updates, enforced valid scoring, and the ability to join the register against dependencies and the schedule all require a real database, not a document
- D) Spreadsheets cannot store text, only numbers

---

## Answer key

<details>
<summary>Reveal after attempting</summary>

1. **B** — cause, uncertain event, consequence; a label alone ("Pulse.io risk") isn't a usable risk statement. (Lecture 1, §2)
2. **C** — once it's already happening, it's an issue needing an active response, not a probability estimate. (Lecture 1, §5)
3. **B** — both probability and impact are 1–5 scales per Lecture 1's table; "6" is out of range regardless of the resulting product. (Lecture 1, §3)
4. **C** — transfer shifts who bears the cost (e.g., via an SLA or contract) without changing the underlying probability. (Lecture 1, §4)
5. **B** — accept must be a conscious, documented decision with a stated reason; undocumented silence isn't a response, it's an oversight. (Lecture 1, §4)
6. **B** — internal dependencies are fixed with sequencing/communication inside a team you run; cross-team ones need negotiation because you don't control the other team's calendar. (Lecture 2, §1)
7. **B** — discovery, alignment, interface risk, and schedule coupling are all real costs beyond the task's raw duration. (Lecture 2, §3)
8. **B** — building a fallback (a minimal version you control) converts a hard blocker into a soft one. (Lecture 2, §4b)
9. **B** — a multi-hop chain is invisible by default because teams several hops away typically don't know your project or date exist, unless someone traces the whole chain explicitly. (Lecture 2, §5)
10. **B** — F is gated by its slowest predecessor, since it can't start until *all* of C, D, and E are finished — that's the maximum, not the average. (Lecture 3, §3)
11. **C** — zero slack means the task is on the critical path: any slip there delays the whole project by the same amount. (Lecture 3, §5)
12. **B** — 9 days of slack means it's not the most urgent thing today, but it's still tracked because consuming all of that slack would make it critical. (Lecture 3, §5)
13. **C** — buffer belongs on critical-path tasks carrying real risk; tasks with slack already have built-in room and don't need more. (Lecture 3, §7)
14. **C** — concurrent updates from multiple owners, enforced valid data, and the ability to join against dependencies/schedule are things a spreadsheet structurally can't do safely; the other options aren't the real reasons. (Lecture 1, §7)

</details>

**Scoring:** 12+ → start Week 7. 9–11 → re-read the lecture sections behind your misses. <9 → re-read all three lectures from the top; the RAID/critical-path vocabulary here is load-bearing for Weeks 7–9.
