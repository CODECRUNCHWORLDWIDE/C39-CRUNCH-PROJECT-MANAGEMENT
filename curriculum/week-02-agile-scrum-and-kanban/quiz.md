# Week 2 — Quiz

Fourteen questions. Lectures closed. Aim for 12/14 before starting Week 3. A mix of multiple-choice, short answer, and "what does this return?" against `atlas_backlog` — the answer key explains the *why*, not just the letter.

---

**Q1.** The Agile Manifesto value "responding to change over following a plan" most accurately means:

- A) Teams should not make plans at all.
- B) A plan is a best guess that should be revised, visibly, as the team learns — not treated as fixed regardless of new information.
- C) Only the Product Owner is allowed to change the plan.
- D) Plans should be replanned every single day.

---

**Q2.** A team skips writing a charter and says "we're Agile, we just start building." Per Lecture 1 §7, this is:

- A) A correct application of the Manifesto's values.
- B) A misuse — nothing in the Manifesto says skip initiation; real Agile teams plan in short, revisable increments, not zero increments.
- C) Only wrong if the project is large.
- D) Fine as long as the Product Owner agrees.

---

**Q3.** In Scrum, who has the authority to decide the order backlog items get built in?

- A) The Scrum Master
- B) The sponsor
- C) The Product Owner
- D) Whoever on the dev team picks it up first

---

**Q4.** During a sprint, what is fixed and what is allowed to flex?

- A) Scope is fixed; the end date flexes.
- B) The end date is fixed; scope inside the sprint may adjust to reality.
- C) Both scope and date are fixed and never change.
- D) Neither is fixed — both are renegotiated daily.

---

**Q5.** A daily standup that consistently runs long because it turns into a live design debate, with stated blockers never followed up on afterward, is showing the failure-mode tell of:

- A) Sprint planning
- B) Daily standup
- C) Sprint review
- D) Retrospective

---

**Q6.** A retrospective ends with "yeah, that was a rough sprint, let's do better" and no other output. Per Lecture 2 §3d, what's missing?

- A) Nothing — that's a healthy, honest retro.
- B) A demo of working software.
- C) A specific, owned, checkable action item.
- D) Sign-off from the sponsor.

---

**Q7.** What should a sprint review primarily show stakeholders?

- A) A slide deck summarizing what shipped.
- B) The next sprint's full backlog.
- C) Working software, demoed live.
- D) The team's individual velocity numbers.

---

**Q8.** Which of Kanban's four core practices is the one that actually forces a behavior change on the team, rather than just adding visibility?

- A) Visualize the workflow
- B) Limit WIP
- C) Make process policies explicit
- D) Manage flow

---

**Q9.** Per Little's Law, if a column's WIP stays the same but its throughput (completion rate) drops, what happens to the average cycle time for items in that column?

- A) It decreases.
- B) It increases.
- C) It's unaffected — cycle time doesn't depend on throughput.
- D) It becomes impossible to measure.

---

**Q10.** In a **pull** system, who decides when a specific piece of work starts?

- A) A manager, based on who looks available.
- B) The person with capacity, when they actually have room for it.
- C) Whoever created the ticket.
- D) It's scheduled automatically by the tool.

---

**Q11.** What's the difference between lead time and cycle time?

- A) They're the same thing, just different names.
- B) Lead time measures from creation to done; cycle time measures only the active-work portion, typically from first pull into progress to done.
- C) Cycle time is always longer than lead time.
- D) Lead time only applies to bugs, cycle time only applies to stories.

---

**Q12.** Why can't you directly compare Scrum's velocity to Kanban's throughput as if they were the same number?

- A) They're actually the same thing, just measured in different units.
- B) Velocity is scoped to a fixed sprint's committed points; throughput is a continuous rate with no batch boundary.
- C) Throughput only counts bugs, not stories.
- D) Velocity can only be calculated in PostgreSQL.

---

**Q13.** Northlight's Support/Platform team receives unpredictable ticket arrivals with no natural batching, and stakeholders care most about individual ticket turnaround time. Per Lecture 3 §6, which fits better?

- A) Scrum, because all Agile work should use Scrum by default.
- B) Kanban, because the work's arrival pattern and the stakeholder need (turnaround time, not batch cadence) both point away from a fixed sprint.
- C) Neither — this team should use a predictive/waterfall approach instead.
- D) Scrum, but with a one-day sprint length.

---

**Q14.** Given `atlas_backlog` in its state from this week's README setup (before Exercise 2's flood rows), what does the following return?

```sql
SELECT status, COUNT(*) AS wip_count
FROM atlas_backlog
WHERE status NOT IN ('backlog', 'done')
GROUP BY status
ORDER BY status;
```

- A) `in_progress: 3, in_review: 2, todo: 2`
- B) `backlog: 8, done: 9`
- C) One row per `item_id`, 20 total
- D) `in_progress: 3, in_review: 8, todo: 2` (the flood already counted)

---

## Answer key

<details>
<summary>Reveal after attempting</summary>

1. **B** — Value 4 is about revising a plan visibly as the team learns, not abandoning planning. "No plan" is a misuse, covered directly in Q2.
2. **B** — nothing in the Manifesto says skip initiation. Real Agile teams plan constantly, just in short, revisable increments (Lecture 1 §7).
3. **C** — the Product Owner owns backlog content and order. The Scrum Master owns process; the sponsor owns why/whether; nobody self-assigns priority.
4. **B** — the sprint's end date is a protected boundary; scope inside it adjusts to reality rather than the date moving (Lecture 2 §2).
5. **B** — that's the standup failure-mode tell named explicitly in Lecture 2 §3b: standup decaying into a design debate with blockers unresolved afterward.
6. **C** — a retro's whole purpose is producing specific, owned, checkable action items (Lecture 2 §3d). "We should do better" isn't one.
7. **C** — working software, demoed live, is what a review is FOR (Lecture 2 §3c and Lecture 1's Value 2). Slides are the review-as-theater failure mode.
8. **B** — limiting WIP is what actually constrains behavior (forces finishing over starting); visualizing and stating policy add clarity but don't by themselves change what anyone does (Lecture 3 §2).
9. **B** — Little's Law: cycle time = WIP ÷ throughput. If WIP holds and throughput drops, cycle time necessarily increases (Lecture 3 §4).
10. **B** — pull means the person with real capacity decides when to take the next item, not a manager assigning it top-down (Lecture 3 §3).
11. **B** — lead time is the full trip (creation to done); cycle time is the active-work portion only (Lecture 3 §5c).
12. **B** — velocity is meaningful only relative to a sprint's fixed boundary and committed points; throughput has no such boundary, so the two answer related but different questions (Lecture 3 §5d and §6).
13. **B** — unpredictable arrival plus a turnaround-time stakeholder need is exactly the profile Lecture 3 §6 points toward Kanban, not a fixed sprint cadence.
14. **A** — per the WIP-per-column query in Lecture 3 §5a, the baseline (pre-flood) result is `in_progress: 3, in_review: 2, todo: 2`. (D) is what you'd get *after* Exercise 2's flood rows are inserted — a good check that you're reasoning about which state of the table the question specifies.

</details>

**Scoring:** 12+ → start Week 3. 9–11 → re-read the lecture sections behind your misses. <9 → re-read all three lectures from the top; ceremonies, WIP, and the Scrum/Kanban fit criteria all compound directly into Week 3's backlog and estimation work.
