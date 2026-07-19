# Week 11 — Quiz

Fifteen questions. Lectures closed. Aim for 12/15 before starting Week 12. A mix of multiple-choice and short "what does this return?" — the answer key at the bottom explains the *why*, not just the letter.

---

**Q1.** In GitHub Projects (v2), where does a custom field's value (like `Priority`) actually live?

- A) On the issue itself, visible from any project it's added to
- B) On the project item — it can differ across two different projects the same issue belongs to
- C) In the repository's settings
- D) Custom field values are computed automatically and can't be set manually

---

**Q2.** Which GitHub Projects field type is purpose-built for sprints, with an `@current` value that saved views can reference without hardcoding a name?

- A) Single select
- B) Date
- C) Iteration
- D) Number

---

**Q3.** A built-in GitHub Projects workflow is configured to fire on "Pull request merged." What does it do?

- A) Deletes the item from the project
- B) Sets the item's Status field to a value you choose (typically Done)
- C) Opens a new issue automatically
- D) Sends an email to the assignee

---

**Q4.** Why must you use the GraphQL API (not the REST API) to script against a GitHub Projects v2 board?

- A) REST is deprecated for all of GitHub
- B) GraphQL is faster
- C) Projects v2 only exposes its data through the GraphQL API
- D) `gh` CLI doesn't support REST

---

**Q5.** In Jira, what's the key structural difference between a Scrum board and a Kanban board?

- A) Kanban boards can't have a Status field
- B) Scrum boards are organized around sprints; Kanban boards have no sprint concept, just continuous flow
- C) Scrum boards don't support custom fields
- D) There is no difference; they're the same feature with different names

---

**Q6.** A Jira workflow forbids moving an issue directly from `To Do` to `Done` — it must pass through `In Progress` first. What GitHub Projects concept enforces the same rule natively?

- A) The Status single-select field, configured with a required order
- B) None — GitHub Projects has no native transition-enforcement; you'd need a custom script/Action to replicate it
- C) The Iteration field
- D) Branch protection rules

---

**Q7.** `SELECT * FROM jira_issues WHERE priority IN ('High', 'Highest') AND status <> 'Done';` — what's the JQL equivalent?

- A) `priority = High, Highest AND status = Done`
- B) `(priority = High OR priority = Highest) AND status != Done`
- C) `priority > Medium AND status = Open`
- D) `priority IN (High, Highest) OR status != Done`

---

**Q8.** Why can't you reliably sort Jira's `priority` column correctly with a plain `ORDER BY priority` in SQL?

- A) SQL can't sort text columns
- B) `priority` is stored as a number in Jira, not text
- C) SQL sorts text alphabetically by default, which doesn't match Jira's configured priority rank (e.g., "Highest" would sort after "High" alphabetically, not before)
- D) You can — this is not actually a problem

---

**Q9.** Which of these is **not** one of the three official ways to export data from Jira covered in Lecture 2?

- A) CSV export from a search result
- B) The REST API `/search` endpoint
- C) A full project bulk export
- D) Direct SQL access to Jira's internal database

---

**Q10.** In Lecture 3's incident, why did the AI state that Sprint 6 shipped "14 story points" when that number appeared nowhere in the input?

- A) It correctly calculated the number from context clues
- B) The screenshot contained the number but it was cropped out
- C) It generated a plausible-sounding number because summarizing sprints usually involves a number like that, filling a gap with no data to fill it correctly
- D) 14 was the actual, correct number and the story is about something else

---

**Q11.** What are the three stages of the grounded-prompt pattern, in the correct order?

- A) Prompt, compute, verify
- B) Compute, serialize, constrain
- C) Draft, edit, ship
- D) Ask, answer, check

---

**Q12.** Why is "please don't make anything up" in a prompt not, by itself, a reliable fix for fabrication?

- A) AI assistants ignore explicit instructions
- B) It's a plea about behavior, not a structural constraint — the model can still fill a genuine data gap with something plausible-sounding unless the pipeline itself prevents ungrounded numbers from reaching the prompt
- C) It's actually a complete fix; nothing more is needed
- D) The phrase triggers a filter that blocks the response entirely

---

**Q13.** A grounding query returns zero rows because of a typo in the sprint name filter. What's the risky failure mode an ungrounded digest script might produce?

- A) The script crashes immediately, which is safe
- B) The AI narrates "no blockers this week, all clear" — technically consistent with the (broken) empty input, substantively wrong
- C) The AI automatically detects and fixes the typo
- D) Nothing — zero rows can't be misinterpreted

---

**Q14.** In the risk-flagging pattern from Lecture 3, who is responsible for defining what "risky" means — the query's filter condition?

- A) The AI, at the moment it narrates the result
- B) The PM, by writing the SQL that encodes the definition — the AI only narrates that query's output
- C) Neither — "risky" is an industry-standard term the AI already knows
- D) The stakeholder receiving the digest, after the fact

---

**Q15.** Which of these is a defensible reason to migrate a numeric Jira `story_points` field into GitHub Projects' `Size` single-select instead of a `Number` field?

- A) GitHub Projects doesn't support Number fields at all
- B) Some teams deliberately want T-shirt sizes instead of point values to avoid arguing over whether something is "a 3 or a 5" — but this requires an explicit, stated numeric-to-bucket rule, not a vague "roughly maps to" choice
- C) Single-select fields are always faster to query than Number fields
- D) Jira's story points are text, not numbers, so they must become a select field

---

## Answer key

<details>
<summary>Reveal after attempting</summary>

1. **B** — custom field values live on the project item, which is why the same issue can show a different Status on two different boards it belongs to.
2. **C** — the Iteration field type generates a rolling series of sprints and exposes `@current` so saved views never need manual date edits.
3. **B** — it sets Status to whatever value you configured (typically Done); it doesn't delete, email, or open anything on its own.
4. **C** — Projects v2 is GraphQL-only for programmatic access; the older REST Projects API doesn't cover v2 boards.
5. **B** — Scrum boards are built around planning/starting/completing sprints; Kanban boards are continuous flow with no sprint ceremony.
6. **B** — GitHub Projects' Status field is a plain single-select with no built-in transition enforcement; replicating Jira's workflow rules requires a custom script or GitHub Action.
7. **B** — parentheses around the OR'd priorities, `AND`, then the not-equal — same precedence rule as SQL's `WHERE`.
8. **C** — SQL's default text sort is alphabetical, which doesn't match Jira's configured ordinal priority rank; you need a `CASE`-based explicit rank to reproduce it.
9. **D** — direct database access isn't a supported (or documented) export path; the three real ones are CSV export, the REST API, and a full bulk export.
10. **C** — the model filled a data gap with a plausible-sounding specific because generating something that sounds like an answer is what happens absent real grounding, not because it "found" the number anywhere.
11. **B** — compute (real query), serialize (verbatim JSON), constrain (a prompt limited to narrating that JSON).
12. **B** — it's a behavioral plea, not a structural guarantee; the actual fix is the pipeline shape that never gives the model room to invent a number in the first place.
13. **B** — an ungrounded narrator treats "zero rows" as "nothing to report" rather than flagging that the query itself might be broken.
14. **B** — the PM encodes "risky" as a SQL filter; the AI's job is narration of that query's output, never inventing its own definition of risk.
15. **B** — T-shirt sizing is a legitimate team preference, but only if the numeric-to-bucket mapping is an explicit, written rule — not a hand-wave.

</details>

**Scoring:** 12+ → start Week 12. 9–11 → re-read the lecture sections behind your misses. <9 → re-read all three lectures from the top; Week 12's capstone assumes this week's tooling and AI discipline are automatic.
