# Week 12 — Quiz

Fifteen questions. Lectures closed. Aim for 13/15 — this is the last quiz of the course, and it draws on the whole 12 weeks, not just this one. A mix of multiple-choice and short scenario questions — the answer key at the bottom explains the *why*, not just the letter.

---

**Q1.** Per Lecture 1, §2, what is the single most common way a capstone fails, and what's the concrete rule of thumb for avoiding it?

- A) Picking a project that's too easy; the rule is to always add stretch scope
- B) Picking a project too large to actually ship in the window; the rule is "can you picture the literal final `git push`?"
- C) Picking a project with no risks; the rule is to always add artificial risk
- D) Skipping the quiz; the rule is to read the lectures twice

---

**Q2.** In the worked example, what specifically did Jordan cut from the original TaskPing idea, and where does Lecture 1 say that cut must be documented?

- A) Nothing was cut; TaskPing was small from the start
- B) The daemon, system tray icon, recurrence, and mobile app were cut, and the charter's objective/scope-out sections must state the cut explicitly
- C) The notification feature was cut entirely
- D) The cut only needs to live in Jordan's head, not the charter

---

**Q3.** Why does Lecture 1, §5 recommend Kanban flow over fixed sprints for a one-week solo capstone?

- A) Kanban is always superior to Scrum regardless of context
- B) A single week is too short for a meaningful sprint boundary, and flow naturally produces the daily throughput data the forecast needs
- C) Sprints require a certified Scrum Master, which solo learners don't have
- D) Kanban boards don't require a WIP limit, which is easier

---

**Q4.** Per Lecture 1, §6, what WIP limit should a solo capstone set on "In Progress," and why?

- A) 5, to allow flexibility
- B) 0, because nothing should ever be in progress
- C) 1, because with only one person doing the work, more than one "in progress" item just means context-switching, not more actual throughput
- D) There's no need for a WIP limit when working solo

---

**Q5.** Per Lecture 2, §1, what is the single most common way flow data gets corrupted mid-delivery?

- A) Using the wrong SQL data type for dates
- B) Leaving a card in "In Progress" when work is actually blocked, instead of moving it to "Blocked" and logging why
- C) Using GitHub Projects instead of Jira
- D) Forgetting to set a WIP limit

---

**Q6.** Per Lecture 2, §2, what makes a risk register "actually live" rather than a one-time Monday artifact?

- A) It has more than 5 rows
- B) It's reviewed and updated — statuses changed, new risks added — as things actually happen through the week, not written once and left static
- C) It's stored in a spreadsheet instead of SQL
- D) It's shown to a stakeholder at least once

---

**Q7.** Per Lecture 2, §5a, why is a simple average throughput number the wrong tool for forecasting a ship date?

- A) Averages are mathematically impossible to compute in SQL
- B) An average hides variance — the shape of good days and bad days — which is exactly what makes forecasting genuinely uncertain
- C) Averages only work for velocity, never for throughput
- D) Averages require a larger sample size than percentiles do

---

**Q8.** In one sentence, describe what a single run of the Monte Carlo forecast (Lecture 2, §5e) actually does.

- A) It computes the mean throughput and divides the backlog by it
- B) It randomly draws days (with replacement) from your real historical throughput, accumulating "items completed" until the remaining backlog is cleared, and records how many simulated days that took
- C) It asks the user to estimate how many days are left
- D) It runs your test suite 10,000 times to check for flaky tests

---

**Q9.** Why does Lecture 2, §5f recommend reporting p50 **and** p85 (not just p50) when communicating a forecast to a stakeholder?

- A) p50 is illegal to report alone under most PM certifications
- B) The median (p50) is a coin-flip case; p85 is confident enough to commit to while still being honest about variance — reporting only p50 hides exactly the uncertainty the technique exists to surface
- C) p85 is always a smaller number than p50, so it looks better
- D) Stakeholders only understand percentiles above 80%

---

**Q10.** Per Lecture 2, §5f, what is one honest limit of Monte Carlo forecasting that this week is explicit about?

- A) It only works with PostgreSQL, never SQLite
- B) It cannot be run more than once per project
- C) It only knows what already happened in your history — a risk that hasn't materialized yet in your data isn't reflected in the forecast, so you must combine it with your risk register
- D) It requires a minimum of 10,000 backlog items to be statistically valid

---

**Q11.** Per Lecture 3, §1, why does the release gate use a SQL query to check that every `must` item is `Done`, rather than an eyeball check of the board?

- A) SQL queries are required by this course's data-tooling rule for any check, no exceptions
- B) A card can look finished on the board without the underlying data actually confirming it — the query can't be fooled the way a glance at the board can
- C) GitHub Projects doesn't support checking item status any other way
- D) It's faster to type SQL than to look at a board

---

**Q12.** Per Lecture 3, §2, what two things does a real rollback plan need beyond just having the commands written down?

- A) A tagged last-known-good state, and an actual tested drill running those commands before you rely on them
- B) Approval from a manager, and a signed document
- C) A backup database, and a second developer to review it
- D) Nothing else — written commands are sufficient on their own

---

**Q13.** List, in order, the four sections of the closing retrospective from Lecture 3, §4.

- A) Budget, timeline, scope, quality
- B) What went well, what didn't, what you'd do differently, one data point from your own history that surprised you
- C) Charter, backlog, sprint plan, release
- D) Sponsor, product owner, tech lead, PM

---

**Q14.** Per Lecture 3, §5, why does the lecture say a delivery report that only lists **met** success criteria "isn't a report, it's marketing"?

- A) Because marketing materials are always dishonest
- B) Because an honest report has to state missed or partial criteria as plainly as met ones, with real cause named, or it isn't giving the reader the actual picture
- C) Because success criteria should never be measured after launch
- D) Because reports are only for external stakeholders, never for the builder themselves

---

**Q15.** In the worked example (Lecture 3, §6), what specifically explained the one-day gap between Jordan's Thursday forecast (p85 = 4 days) and the actual 5-day outcome?

- A) The Monte Carlo simulation had a bug
- B) Jordan simply worked less than planned
- C) The macOS notification-permissions risk, flagged as "watching" on Monday, materialized after the forecast was run — exactly the kind of not-yet-materialized risk Lecture 2, §5f warned a forecast can't see
- D) The forecast was never actually run; the date was a guess

---

## Answer key

<details>
<summary>Reveal after attempting</summary>

1. **B** — scoping too large is the most common failure; the rule of thumb is picturing the literal final `git push` by the deadline. (Lecture 1, §2)
2. **B** — the daemon, tray icon, recurrence, and mobile app were all cut; Lecture 1 insists the cut lives in the charter's objective and scope-out sections, not just in your head. (Lecture 1, §3–4)
3. **B** — a week is too short for sprint boundaries to mean much, and Kanban flow naturally generates the daily throughput data the forecast in Lecture 2 depends on. (Lecture 1, §5)
4. **C** — a WIP limit of 1, because for one person, more "in progress" items just means slower context-switched progress on all of them, not more real output — Little's Law (Week 8) explains the mechanism. (Lecture 1, §6)
5. **B** — leaving a card in "In Progress" while actually blocked is the single most common way flow data gets corrupted; move it to "Blocked" and log why, immediately. (Lecture 2, §1)
6. **B** — a live risk register is one that's actually revisited and updated as things happen, not a document written once on Monday and never touched again. (Lecture 2, §2)
7. **B** — an average collapses variance into one number, and variance — the shape of good and bad days — is precisely what makes forecasting uncertain and worth simulating properly. (Lecture 2, §5a)
8. **B** — one simulation run randomly resamples your own historical daily throughput, with replacement, accumulating completed items until the remaining backlog clears, and records the number of simulated days that took. (Lecture 2, §5e)
9. **B** — p50 is a coin flip; p85 is the honestly-confident number to actually commit to — reporting only p50 hides the variance the whole technique exists to surface. (Lecture 2, §5f)
10. **C** — Monte Carlo forecasting only knows what's already in your history; a flagged-but-not-yet-materialized risk isn't reflected in it, so the forecast and the risk register must be read together. (Lecture 2, §5f)
11. **B** — a board card can visually look "done" without the underlying data confirming it; a SQL query checks the actual fact, not the appearance. (Lecture 3, §1)
12. **A** — a tagged last-known-good state, plus an actual, executed rollback drill before you need it for real — commands alone, untested, are a guess dressed up as a plan. (Lecture 3, §2)
13. **B** — what went well, what didn't, what you'd do differently, and one data point from your own history that surprised you. (Lecture 3, §4)
14. **B** — an honest report states missed and partial criteria as plainly as met ones, with the real cause named; omitting the misses turns a report into a highlight reel. (Lecture 3, §5)
15. **C** — the notification-permissions risk (flagged "watching" Monday) materialized after Thursday's forecast was run, which is exactly the category of gap Lecture 2, §5f said a Monte Carlo forecast structurally can't see coming. (Lecture 3, §6)

</details>

**Scoring:** 13+ → you've completed the course; go ship your mini-project if you haven't already. 10–12 → re-read the lecture sections behind your misses before finalizing your delivery report. <10 → re-read all three lectures from the top; this week integrates the entire course, and a shaky score here usually points back to a specific earlier week worth revisiting.
