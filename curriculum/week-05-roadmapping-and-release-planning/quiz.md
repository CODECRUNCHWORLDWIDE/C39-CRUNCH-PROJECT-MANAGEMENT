# Week 5 — Quiz

Fifteen questions. Lectures closed. Aim for 13/15 before starting Week 6. A mix of multiple-choice and short scenario questions — the answer key at the bottom explains the *why*, not just the letter.

---

**Q1.** Per Lecture 1, §1–2, what is the core difference between a roadmap and a promise?

- A) They're the same thing; "promise" is just a more formal word for "roadmap"
- B) A roadmap communicates direction at appropriate, varying confidence; a promise is a specific, dated commitment someone is accountable for keeping exactly as stated
- C) A roadmap is for executives; a promise is for engineers
- D) A promise never has a date attached

---

**Q2.** Rewrite this feature-framed roadmap item as an outcome, per Lecture 1, §3's test: "Ship the Data Export API." Which of these is a properly outcome-framed version?

- A) "Ship the Data Export API in Q3"
- B) "Unblock the 2 partner integrations Sales has stalled on for a quarter"
- C) "Build a CSV and JSON export feature"
- D) "Data Export API — high priority"

---

**Q3.** In Northlight's Q3 backlog, why does Real-Time Presence Indicators land in the **Later** bucket, per Lecture 1, §5?

- A) Priya personally dislikes the feature
- B) It's technically infeasible
- C) The cumulative story-point total crosses the 114-point capacity line before reaching it, given the priority order — not because it's unimportant
- D) It has no assigned theme

---

**Q4.** A colleague says "Later-bucket items just mean we don't care about them." Per Lecture 1, §4, what's the correct correction?

- A) They're right — Later is where low-priority ideas go
- B) Later means the item is un-scheduled this quarter due to capacity, not that it's unimportant or forgotten — it still has an owner, a theme, and a reason
- C) Nothing goes in Later on a well-run roadmap
- D) Later items should never be discussed with stakeholders

---

**Q5.** Why does Lecture 2 recommend a 15% capacity buffer rather than planning against raw average velocity?

- A) It makes the team look less productive on paper
- B) Raw average velocity assumes zero support load, on-call, or PTO every single sprint — a buffer reflects what the team has actually, historically been able to deliver
- C) PostgreSQL requires a buffer column by convention
- D) It's an industry-standard percentage with no team-specific basis

---

**Q6.** Northlight's team's last four sprints were 19, 24, 21, 24 points. What is `planning_velocity` after applying Lecture 2, §3's buffer?

- A) 22
- B) 19
- C) 24
- D) 17

---

**Q7.** What is the defining difference between a fixed-scope release and a fixed-date release, per Lecture 2, §6?

- A) Fixed-scope releases never slip; fixed-date releases always slip
- B) In a fixed-scope release, the date can flex if needed; in a fixed-date release, the date is immovable and scope is the lever that flexes
- C) Fixed-date releases don't need a release plan at all
- D) They are two names for the same thing

---

**Q8.** In Northlight's R2, why does Data Export API get trimmed (CSV export + API key management ship; JSON export defers to R3) rather than SOC 2 being delayed?

- A) JSON export is technically harder than CSV export
- B) SOC 2's audit date is externally fixed and cannot move; Data Export API has no such constraint, so it absorbs the scope cut instead
- C) Elena prefers CSV over JSON
- D) R2's capacity was increased specifically to fit both in full

---

**Q9.** Per Lecture 2, §7, what does an *empty* result set from the dependency-integrity query prove?

- A) The backlog has no dependencies at all
- B) Nothing in the release plan is currently scheduled to ship before the dependency it relies on
- C) The release plan is fully staffed
- D) All items are correctly sized

---

**Q10.** Per Lecture 2, §8, why does Advanced Permissions get sequenced ahead of Data Export API despite both mattering to the business?

- A) Advanced Permissions has fewer story points
- B) Advanced Permissions unblocks revenue already stalled (renewals actively at risk); Data Export API unblocks revenue not yet stalled — sequencing follows cost of delay, not size alone
- C) Marcus prefers working on permissions systems
- D) Alphabetical order

---

**Q11.** Advanced Permissions slips two sprints, discovered at the Sprint 2 review. Per Lecture 3, §2's threshold test, why does this specific slip warrant a roadmap-level re-plan rather than quiet absorption?

- A) Any slip, of any size, always warrants a full re-plan
- B) It exceeds the reserved buffer, a downstream item (SOC 2) depends on it, SOC 2's date is externally fixed, and R1 had already been presented as high-confidence — it fails every column of the threshold test
- C) Marcus asked for a re-plan personally
- D) Two sprints is an arbitrary universal threshold for all releases

---

**Q12.** After the slip, why does SOC 2 get first claim on the 68 remaining points, per Lecture 3, §4, even though it isn't the highest-value item on the remaining list?

- A) It's alphabetically first
- B) It's the one item with zero flexibility on timing — everything else still has scheduling flexibility even if scope doesn't
- C) Priya personally ranked it highest
- D) It was the cheapest remaining item

---

**Q13.** Per Lecture 3, §5, what makes the example slip-communication message to Priya a *good* one?

- A) It's sent the moment SOC 2's date arrives and the slip is undeniable
- B) It's sent early (Sprint 2), states what's protected and what's at risk without burying the real decision, and gives Priya an actual choice
- C) It avoids mentioning any risk to keep the tone positive
- D) It asks Priya to make every detailed scheduling decision herself

---

**Q14.** What is the actual test, per Lecture 3, §7, for whether an exec-altitude and a delivery-altitude version of the same roadmap are honest versions of each other rather than two different stories?

- A) They use the same font and formatting
- B) They are exactly the same length
- C) Both could be shown, side by side, to the same skeptical outsider with zero contradiction between them
- D) The exec version is always approved first, then the delivery version is written to match

---

**Q15.** A teammate argues: "Just don't tell leadership about small slips, only the big ones." Using Lecture 3's vocabulary, what's the flaw in that advice?

- A) There's no flaw — this is exactly Lecture 3's recommendation
- B) "Small" is often only knowable in hindsight; by the time a slip is undeniably "big," it's too late to react cheaply — the whole point of early communication is surfacing it while it's still small and there's still a real choice to make
- C) Leadership should be told about every single backlog change, no matter how minor
- D) Slips should only ever be communicated in writing, never verbally

---

## Answer key

<details>
<summary>Reveal after attempting</summary>

1. **B** — a roadmap communicates direction at varying, honest confidence; a promise is a specific dated commitment. (Lecture 1, §1–2)
2. **B** — it states the actual business outcome (unblocking stalled partner revenue), not a restated feature or a scheduling note. (Lecture 1, §3)
3. **C** — the cumulative running total crosses the 114-point capacity line before reaching item 6; it's a capacity fact, not a priority judgment. (Lecture 1, §5)
4. **B** — Later means unscheduled due to capacity this quarter, not unimportant; it retains an owner, theme, and reason. (Lecture 1, §4)
5. **B** — raw average velocity ignores the support/on-call/PTO load the team has always actually carried; the buffer reflects real historical throughput. (Lecture 2, §3)
6. **B** — 22 average × 0.85 ≈ 18.7, rounded to 19. (Lecture 2, §3)
7. **B** — fixed-scope flexes on date if needed; fixed-date flexes on scope because the date can't move. (Lecture 2, §6)
8. **B** — SOC 2's date is an immovable external constraint; Data Export API has no such constraint, so it's the item with real flexibility to absorb the cut. (Lecture 2, §6)
9. **B** — an empty result proves nothing in the current assignment ships before what it depends on; it's a correctness check, not a staffing or sizing check. (Lecture 2, §7)
10. **B** — sequencing follows cost of delay relative to size, not size alone; Advanced Permissions protects revenue already at risk. (Lecture 2, §8)
11. **B** — it fails all four columns of the threshold test (size beyond buffer, downstream dependents, external commitment at risk, prior high-confidence framing), not just one. (Lecture 3, §2)
12. **B** — SOC 2 is the only remaining item with zero timing flexibility; everything else can still be resequenced even if its scope can't. (Lecture 3, §4)
13. **B** — sent early, states what's protected and at risk plainly, and gives a real choice rather than a fait accompli. (Lecture 3, §5)
14. **C** — the test is whether both versions, read side by side by a skeptical outsider, show zero contradiction — not formatting or length. (Lecture 3, §7)
15. **B** — "small" is frequently only knowable in hindsight; waiting until a slip is undeniably big removes the reaction time that made early communication valuable in the first place. (Lecture 3, §1 and §5)

</details>

**Scoring:** 13+ → start Week 6. 10–12 → re-read the lecture sections behind your misses. <10 → re-read all three lectures from the top; this week's sequencing and communication discipline is load-bearing for risk management (Week 6) and status reporting (Week 7).
