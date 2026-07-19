# Week 3 — Quiz

Fifteen questions. Lectures closed. Aim for 12/15 before starting Week 4. A mix of multiple-choice and short scenario questions — the answer key at the bottom explains the *why*, not just the letter.

---

**Q1.** In Ron Jeffries's "Three Cs," what does the **Card** actually represent?

- A) The complete, final requirement for the feature
- B) A short token that represents the promise of a future conversation
- C) A legal contract between the PM and the engineering team
- D) The acceptance criteria for the story

---

**Q2.** Which of these is a properly formed role–goal–benefit user story?

- A) "As a user, I want the app to be better."
- B) "Add a settings page with a toggle switch."
- C) "As an account admin, I want to remove a teammate from a workspace, so that access is revoked the moment they leave the team."
- D) "As a developer, I want to refactor the invite service, so that it's cleaner."

---

**Q3.** A story says: "As a workspace member, I want to comment on a dashboard, so that I can comment on a dashboard." What's wrong with it?

- A) The role is too vague.
- B) The benefit is hollow/circular — it restates the goal instead of giving a real reason.
- C) It's actually a technical story in disguise.
- D) Nothing — this is a well-formed story.

---

**Q4.** Which elicitation technique specifically surfaced that a Northlight customer had built a manual spreadsheet workaround, a detail a hallway comment never would have revealed?

- A) A prioritization workshop
- B) A stakeholder interview
- C) A retrospective
- D) A sprint review

---

**Q5.** What does the **I** in INVEST stand for, and what does it test?

- A) Iterative — the story must be built in stages.
- B) Independent — the story can be built and delivered without being blocked by another unfinished story.
- C) Integrated — the story must connect to every other system.
- D) Immediate — the story must be worked on right away.

---

**Q6.** A team slices an epic into "Build the database schema," "Build the backend API," and "Build the frontend UI." What is the core problem with this approach?

- A) It's technically incorrect — those layers don't actually exist.
- B) None of the three stories is independently demoable or valuable — there's nothing to show a stakeholder until all three are done.
- C) It violates the Definition of Done.
- D) It's actually fine — layered slicing is the recommended default.

---

**Q7.** Which of the following is an example of slicing **by business rule variation**?

- A) Splitting "invite a teammate" into desktop and mobile versions.
- B) Splitting "manage workspaces" into create, rename, and archive.
- C) Splitting "invite a teammate" into "invite someone who already has an account" (common case) vs. "invite someone who doesn't" (edge case, more work).
- D) Splitting a feature into a database story and a UI story.

---

**Q8.** A story reads: "As an admin, I want a dropdown menu with exactly these five options: Rename, Archive, Delete, Duplicate, Export." Which INVEST letter does this most clearly fail, and why?

- A) Testable — there's no way to verify a dropdown exists.
- B) Negotiable — it dictates the exact solution/UI instead of leaving room for a conversation about the best design.
- C) Small — five options is too many for one story.
- D) Estimable — dropdowns are impossible to estimate.

---

**Q9.** In Given/When/Then acceptance criteria, which of the following is an acceptable "Then" clause?

- A) "Then the invite is processed correctly."
- B) "Then the system handles the request properly."
- C) "Then jordan@acme.com receives an invite email, and the member list shows them as Pending."
- D) "Then everything works as expected."

---

**Q10.** What is the key difference between **Definition of Ready** and **Definition of Done**?

- A) They're the same checklist used at different times.
- B) Definition of Ready gates a story *entering* a sprint; Definition of Done gates a story being considered *finished*.
- C) Definition of Ready only applies to epics; Definition of Done only applies to stories.
- D) Definition of Done is written by the PM alone; Definition of Ready is written by the whole team.

---

**Q11.** Why did Atlas's team add "any third-party integration must be smoke-tested against the real sandbox" to their Definition of Done partway through the project?

- A) It was part of the original template and never changed.
- B) A past incident (the sharing-API sandbox outage from Week 1) revealed a gap, and the DoD was tightened in response.
- C) Priya mandated it as a compliance requirement.
- D) It has nothing to do with past project events — DoD items are chosen arbitrarily.

---

**Q12.** Sort this into the correct MoSCoW bucket: a capability the charter's success criteria cannot be met without.

- A) Should have
- B) Could have
- C) Must have
- D) Won't have (this time)

---

**Q13.** In WSJF, what does the formula actually compute, in plain language?

- A) The absolute business value of a story, ignoring cost.
- B) The value delivered per unit of time the story occupies the team — cost of delay divided by job size.
- C) The number of story points assigned by the team.
- D) The number of stakeholders who requested the feature.

---

**Q14.** In the Week 3 lecture's WSJF worked example, why did "invite a teammate by email" score higher than the "real-time presence indicator" spike, even though the spike scored well on risk reduction?

- A) The spike's job size was large enough to significantly reduce its WSJF score despite a decent Cost of Delay.
- B) Spikes can never be prioritized under WSJF.
- C) Risk reduction isn't a valid WSJF input.
- D) "Invite a teammate" had zero cost of delay.

---

**Q15.** A backlog was carefully groomed two weeks ago, but at sprint planning half the "ready" stories turn out to be stale or no longer accurate. What does this most directly indicate?

- A) The team should stop using INVEST.
- B) The backlog wasn't being regularly refined/groomed — readiness isn't a one-time event, it decays as the product and business context change.
- C) User stories are fundamentally flawed as a format.
- D) The PM should switch to a full up-front requirements spec instead.

---

## Answer key

<details>
<summary>Reveal after attempting</summary>

1. **B** — the card is a placeholder for a future conversation, not the requirement itself; detail lives in the conversation and gets locked in by acceptance criteria.
2. **C** — it has a specific role, a single clear goal, and a real, non-circular benefit. A is too vague on every axis; B has no role/benefit and dictates a UI; D is technical work wearing a user-story costume.
3. **B** — "so that I can comment on a dashboard" just restates the goal; it gives no real reason the capability matters.
4. **B** — the stakeholder interview (Elena asking "walk me through what you're doing today") is what surfaced the spreadsheet workaround; that kind of concrete detail rarely surfaces from a passing hallway comment.
5. **B** — Independent: the story should be buildable/deliverable without waiting on another unfinished story.
6. **B** — none of the layered stories delivers value or is demoable alone; feedback and partial value are both lost until all layers are complete.
7. **C** — splitting the common case from a genuinely different, harder edge case is business-rule-variation slicing. A is interface variation, B is CRUD/data variation, D is the horizontal-layer trap (not a valid slicing pattern at all).
8. **B** — Negotiable. Prescribing the exact UI removes the team's ability to have a real conversation about the best design; the goal should be stated, not the solution.
9. **C** — it's specific and observable: someone (or a test) can actually check that the email arrived and the list shows "Pending." A, B, and D are all internal/vague and unverifiable.
10. **B** — Definition of Ready answers "can we start this," checked before sprint planning; Definition of Done answers "is this finished," checked at the sprint review.
11. **B** — DoD should evolve based on real lessons learned (the retrospective habit from Week 1), not stay frozen from a template; the sandbox-outage risk directly caused this addition.
12. **C** — Must have: by MoSCoW's definition, this is exactly the bucket for anything the release is not viable, or the charter's success criteria unmeetable, without.
13. **B** — WSJF = Cost of Delay ÷ Job Size, i.e., value created per unit of time the item occupies the team, prioritizing efficient value delivery over raw value alone.
14. **A** — even with a real risk-reduction score, a job size of 8 (vs. 3 for the invite story) drags the spike's WSJF down; the formula weighs cost, not just benefit.
15. **B** — readiness decays over time as context changes; a backlog needs a regular refinement cadence (Lecture 3 §5), not a one-time grooming pass.

</details>

**Scoring:** 12+ → start Week 4. 9–11 → re-read the lecture sections behind your misses. <9 → re-read all three lectures from the top; estimation and sprint planning next week assume this backlog vocabulary is solid.
