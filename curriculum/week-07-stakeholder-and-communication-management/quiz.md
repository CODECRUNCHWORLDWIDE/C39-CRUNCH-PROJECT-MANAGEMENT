# Week 7 — Quiz

Fifteen questions. Lectures closed. Aim for 13/15 before starting Week 8. A mix of multiple-choice and short scenario questions — the answer key at the bottom explains the *why*, not just the letter.

---

**Q1.** Why are power and interest scored on two **independent** scales rather than combined into a single "importance" number?

- A) Because combining them is mathematically impossible
- B) Because a stakeholder can have high power and low interest (or the reverse), and that gap is exactly where risk hides
- C) Because PMI requires two separate numbers
- D) Because interest is always higher than power in practice

---

**Q2.** In the power/interest grid, which quadrant gets the **most** 1:1 time and involvement in decisions?

- A) Monitor
- B) Keep Informed
- C) Keep Satisfied
- D) Manage Closely

---

**Q3.** Why is a "Keep Satisfied" stakeholder more dangerous to neglect than a "Monitor" stakeholder, per Lecture 1?

- A) Keep Satisfied stakeholders always complain more
- B) They have real power to affect the project but aren't watching closely, so bad news reaches them late, from someone else, with no context
- C) Monitor stakeholders have no power at all, so nothing can go wrong with them
- D) There's no real difference; the labels are arbitrary

---

**Q4.** Which of the following best defines a "silent blocker risk," per Lecture 1, §5?

- A) Any stakeholder who hasn't responded to an email in a week
- B) A stakeholder with low power and low interest
- C) A stakeholder with high power and low **current** visible interest, who could react badly if surprised later
- D) A stakeholder who has formally vetoed the project

---

**Q5.** Every project needs exactly one identifiable sponsor. Per Lecture 1, §4, what specific question should the sponsor always be able to answer?

- A) What is today's sprint velocity?
- B) Is this still worth doing, and what wins if scope/time/cost can't all hold?
- C) Which engineer should fix the next bug?
- D) What color should the status report be?

---

**Q6.** A communication plan row specifies audience, message focus, channel, and cadence. Why must the **message focus** differ between Priya and Jamie even when reporting the same underlying Atlas status, per Lecture 2, §2?

- A) It shouldn't differ — sending identical messages to everyone is best practice
- B) Priya needs trade-off-level detail to make decisions; Jamie needs plain-language facts she can repeat to customers — same truth, different depth and framing
- C) Jamie has higher power than Priya, so she gets more detail
- D) Message focus is determined randomly to keep things fair

---

**Q7.** Per Lecture 2, §3, what makes a RAG (Red/Amber/Green) status system actually trustworthy over time?

- A) Using green as often as possible to keep morale up
- B) Defining each color against observable, consistent criteria decided in advance, not a feeling in the moment
- C) Letting each stakeholder choose their own definition of green
- D) Avoiding red entirely, using amber for all bad news instead

---

**Q8.** A schedule has 1 day of critical-path buffer left, and the next blocked task needs 3 days of work once unblocked, with no credible way to compress it further. Per Lecture 2, §3's criteria, what should the schedule RAG be?

- A) Green, because the team is working hard
- B) Amber, because there's still some buffer left
- C) Red, because the mitigation has effectively failed and the committed date is no longer credible
- D) There's not enough information to know

---

**Q9.** Why does Lecture 2, §5 say that burying a slip behind good news is worse for trust than reporting an honest overall Amber or Red?

- A) It isn't worse — leading with good news is always the better strategy
- B) Readers eventually notice the pattern and start distrusting every future "Green," even when it's true
- C) Burying bad news is illegal under most corporate reporting standards
- D) Good news should never appear in the same report as bad news

---

**Q10.** What is the key structural difference between a complaint and a real escalation, per Lecture 3, §1?

- A) A complaint is spoken; an escalation is always written
- B) A complaint describes a problem and stops; an escalation adds impact, a specific ask, and a recommendation
- C) Escalations must always go to the CEO
- D) There is no real difference; they're the same thing with different names

---

**Q11.** What is the "gut-check" Lecture 3, §2 suggests to test whether an escalation is finished before sending it?

- A) Whether it's under 100 words
- B) Whether the recipient could respond in one sentence
- C) Whether it includes at least three options
- D) Whether it was reviewed by legal

---

**Q12.** Per Lecture 3, §3, which of these is the clearest example of a genuine escalation rather than something to just handle yourself?

- A) A missing label on a Jira ticket
- B) A blocker that requires a sponsor to approve a scope cut or a date change outside your own authority
- C) A typo in a meeting invite
- D) A teammate being five minutes late to standup

---

**Q13.** Why does Lecture 3, §4 recommend logging even an "obvious yes" change request before actioning it?

- A) Because obvious requests are usually rejected anyway
- B) Because scope creep rarely happens as one dramatic decision — it accumulates through many small, individually-reasonable, unlogged "sure, why not" moments
- C) Because logging is required by law for all software changes
- D) Because obvious requests take longer to log than to build

---

**Q14.** In the Priya/Amara conflict scenario (Lecture 3, §5), what is the recommended **first** step before proposing any resolution?

- A) Immediately side with whichever stakeholder has more formal authority
- B) Separate each person's stated position from their actual underlying need
- C) Avoid the conversation until one of them brings it up directly with the other
- D) Flip a coin to stay neutral

---

**Q15.** Per Lecture 3, §5, what should a PM do if two stakeholders still can't agree after seeing the same neutral facts and an explicitly named trade-off?

- A) Let the decision default by whichever deadline passes first
- B) Defer to whoever has final decision authority per the charter, and communicate that respectfully to the other party so they still feel heard
- C) Escalate to a third, uninvolved stakeholder to break the tie
- D) Quietly implement whichever option is technically easiest

---

## Answer key

<details>
<summary>Reveal after attempting</summary>

1. **B** — power and interest are independent; a high-power, low-interest gap is precisely the silent-blocker pattern the whole lecture is built around. (Lecture 1, §2)
2. **D** — Manage Closely stakeholders get full partnership, real detail, and involvement in trade-off decisions. (Lecture 1, §3)
3. **B** — Keep Satisfied stakeholders have real power but aren't watching, so bad news reaches them late and from someone else, without context — the exact failure mode that damages trust. (Lecture 1, §3)
4. **C** — silent blocker risk is specifically high power + low **current visible** interest, not low power, not a formal veto (that's already a known blocker, not a silent one). (Lecture 1, §5)
5. **B** — the sponsor must be able to answer whether the project is worth continuing and what gives when scope/time/cost can't all hold — the triple-constraint question from Week 1. (Lecture 1, §4)
6. **B** — same underlying truth, but Priya needs decision-grade detail while Jamie needs customer-repeatable plain language; that's the "one message, many audiences" principle. (Lecture 2, §2)
7. **B** — RAG only works, and stays trusted, when each color is tied to observable criteria fixed in advance, not a mood call made report by report. (Lecture 2, §3)
8. **C** — Red: the criteria explicitly describe "mitigation has failed and the committed date is no longer credible," which is exactly 1 day of buffer against a 3-day need with no further compression possible. (Lecture 2, §3)
9. **B** — once a reader notices bad news getting buried, they stop trusting every future "Green," which is far more corrosive than one honest bad report. (Lecture 2, §5)
10. **B** — a complaint stops at describing the problem; a real escalation adds impact, a specific ask, and a recommendation, so the reader can actually decide something. (Lecture 3, §1)
11. **B** — could the recipient respond in one sentence; if not, the escalation still requires them to do thinking that should have been done before sending. (Lecture 3, §2)
12. **B** — anything requiring authority you don't have (a sponsor-level scope/date call) is a genuine escalation; the others are things to just fix yourself. (Lecture 3, §3)
13. **B** — scope creep is death by a thousand small, unlogged "sure" moments, not one dramatic unauthorized decision; logging even the easy yeses keeps every trade-off visible and chosen. (Lecture 3, §4)
14. **B** — separating stated position from underlying need is the first step, and it's what sometimes reveals a resolution neither original position considered. (Lecture 3, §5)
15. **B** — defer to whoever the charter actually names as the decision authority, and close the loop respectfully with the other party so they feel heard even though the outcome didn't go their way. (Lecture 3, §5)

</details>

**Scoring:** 13+ → start Week 8. 10–12 → re-read the lecture sections behind your misses. <10 → re-read all three lectures from the top; this week's judgment calls compound directly into how every later status report and escalation lands.
