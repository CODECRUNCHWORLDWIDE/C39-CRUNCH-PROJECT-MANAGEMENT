# Week 10 — Quiz

Fifteen questions. Lectures closed. Aim for 12/15 before starting Week 11. A mix of multiple-choice and short scenario questions — the answer key at the bottom explains the *why*, not just the letter.

---

**Q1.** What's the key difference in *scope* between acceptance criteria and a definition of done?

- A) They're the same thing, just different names
- B) Acceptance criteria apply to one specific story; a DoD applies to every story (or release) of a kind
- C) A DoD is written by QA; acceptance criteria are written by engineering
- D) Acceptance criteria are optional; a DoD is not

---

**Q2.** "Code is peer-reviewed and merged to main" belongs to which DoD category?

- A) Tests
- B) Docs
- C) Code
- D) Sign-off

---

**Q3.** Which of these is a genuine sign that a quality gate has become ceremony rather than real signal?

- A) It has failed at least once for a real, specific reason in the team's recent history
- B) The person checking it has no stake in the outcome either way and no independent standing
- C) Failing it blocks a merge or a deploy, concretely
- D) It's checked by someone other than the person who did the work

---

**Q4.** In Atlas's GA, three DoD lines are `not_met` at once: the load test, QA sign-off, and Platform Team sign-off. What does that pattern most likely indicate?

- A) Three unrelated problems that each need a separate fix
- B) A checklist error — only one line should exist
- C) A single shared root cause surfacing as three separate checklist failures
- D) That the DoD itself is badly designed and should be shortened

---

**Q5.** Whose job is it to personally judge whether a piece of code is good enough to ship?

- A) The PM, always, since they own the release
- B) The delivery team and QA — the PM's job is making sure that question actually gets asked with enough runway to act on it
- C) The sponsor
- D) Nobody — quality is a vibe, not a decision

---

**Q6.** Which of these belongs to the release readiness checklist but **not** the DoD?

- A) Unit test coverage on new code
- B) Peer review of a pull request
- C) Briefing the Support team on what shipped and how to help
- D) API docs for a changed endpoint

---

**Q7.** A feature flag exposes a change to 10%, then 50%, then 100% of users, pausing to check health at each stage. This is:

- A) A canary release
- B) A percentage-based (staged) rollout
- C) A blue-green deployment
- D) A big-bang release

---

**Q8.** Under which of these conditions is a big-bang release still the *defensible* choice?

- A) The failure mode is severe and hard to predict
- B) The team has a real, currently-unresolved dependency risk
- C) The change is small, well-tested, and this kind of change has shipped cleanly many times before
- D) There's no monitoring in place yet to detect a problem in a small slice

---

**Q9.** A go/no-go decision has exactly three honest outcomes. Which of these is **not** one of them?

- A) Go
- B) No-go
- C) Conditional go
- D) "Let's just watch it closely and decide later"

---

**Q10.** For a `conditional_go` decision to actually be useful, the stated condition must be:

- A) Optimistic, to keep morale up
- B) Precise enough that someone could act on it without a follow-up question
- C) Left to each stakeholder's own interpretation
- D) Approved unanimously before it can be written down

---

**Q11.** Which of these is a properly **observable** rollback trigger condition?

- A) "Roll back if things feel bad"
- B) "Roll back if the team loses confidence"
- C) "Roll back if the signature-verification success rate drops below 95% for 5+ consecutive minutes"
- D) "Roll back if a stakeholder asks us to"

---

**Q12.** Why does decoupling a feature flag from a deploy make rollback meaningfully faster?

- A) It doesn't — a flag flip and a redeploy take about the same time
- B) Flipping a flag is a config change measured in seconds; reverting a full deploy takes minutes to tens of minutes
- C) Feature flags are required by law for any production release
- D) Flags eliminate the need for a rollback plan entirely

---

**Q13.** The "expand, contract" pattern for database migrations exists to:

- A) Make migrations run faster
- B) Avoid ever having to write a migration
- C) Keep a migration safely reversible by only adding (never removing) structure until you're confident you'll never need to roll back
- D) Reduce the number of tables in a database

---

**Q14.** What does "blameless" mean in a post-release review?

- A) No individual's actions are ever mentioned in the timeline
- B) The review assumes each person made a reasonable decision given what they knew at the time, and looks for the systemic cause instead of the person to blame
- C) Nobody is allowed to disagree with the team lead's account of events
- D) The review is optional if the release didn't fail badly enough

---

**Q15.** Which of these is an acceptable post-release action item?

- A) "Be more careful with load testing next time."
- B) "Redefine 'peak load' for future load tests using real GA-scale traffic data, owned by Yuki Tanaka, due 2026-05-08."
- C) "Engineering should try harder."
- D) "Note the incident and move on."

---

## Answer key

<details>
<summary>Reveal after attempting</summary>

1. **B** — acceptance criteria are story-specific; a DoD is a fixed checklist applied to every story or release of a given kind, regardless of what it's about.
2. **C** — peer review and merging to main are code-category criteria. (Tests would be coverage/integration/load tests; docs would be API docs/runbooks; sign-off would be QA/PO/security approval.)
3. **B** — a gate checked by someone with no independent standing and no stake in saying "not met" is exactly how ceremony gates form. A (has genuinely failed before), C (real consequence for failing), and D (independent checker) are all signs of a *real* gate.
4. **C** — the load test failure, QA's inability to sign off, and Platform's inability to sign off all trace back to the same signature-verification-under-load problem. Treating them as three separate fixes misses that fixing the shared cause resolves all three.
5. **B** — the delivery team builds it right and QA independently verifies it; the PM's job is protecting the gate itself (making sure the question gets asked on time and "not met" actually stops the release), not personally assessing code quality.
6. **C** — briefing Support is a release-event concern (does the org know this is happening and how to help), not a property of the work itself. A, B, and D are all DoD-category concerns (tests, code, docs respectively).
7. **B** — a percentage-based staged rollout, widening in stages with a health check at each one. A canary uses a small, specific low-risk slice (often internal); blue-green uses two full parallel environments.
8. **C** — big-bang is defensible when the change is small, low-risk, well-tested, and this kind of change has a track record of shipping cleanly. A, B, and D all describe conditions that argue *for* staging, not against it.
9. **D** — "watch closely and decide later" isn't a decision, it's a deferral dressed up as one. The three honest outcomes are go, no-go, and conditional-go (a *narrower*, precisely-scoped go).
10. **B** — an actionable condition is specific enough that the person executing it (e.g., an on-call SRE) doesn't need to come back and ask what it actually meant.
11. **C** — a specific, measurable threshold. A, B, and D all describe a feeling or a social signal, not an observable condition anyone on the team could check independently.
12. **B** — a flag flip is a fast config change; a full deploy revert requires rebuilding, redeploying, and waiting on a pipeline, which takes meaningfully longer. That speed difference is the entire reason flags matter for rollback design.
13. **C** — "expand, contract" ships the additive (safe, reversible) step first and defers the destructive (irreversible) step to a later, separate release once confidence is high — keeping rollback genuinely possible in between.
14. **B** — blameless doesn't mean pretending no one did anything; it means assuming reasonable decisions under the information available at the time, and hunting for the systemic gap that let a reasonable decision produce a bad outcome.
15. **B** — specific, owned by a real person, and dated. A, C, and D are all vague exhortations that produce no actual change and can't be checked off as done.

</details>

**Scoring:** 12+ → start Week 11. 9–11 → re-read the lecture sections behind your misses. <9 → re-read all three lectures from the top; go/no-go and rollback reasoning compound directly into the mini-project.
