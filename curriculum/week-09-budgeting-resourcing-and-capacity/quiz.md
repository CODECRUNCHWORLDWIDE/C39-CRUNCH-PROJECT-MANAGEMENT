# Week 9 — Quiz

Fourteen questions. Lectures closed. Aim for 12/14 before starting Week 10. A mix of multiple-choice and short "what does this compute" — the answer key at the bottom explains the *why*, not just the letter.

---

**Q1.** On a typical technology project, which cost category is almost always dominant, usually 70–90% of the total?

- A) Tooling and SaaS subscriptions
- B) Vendor/third-party contracts
- C) Labor
- D) Contingency

---

**Q2.** A person's "fully loaded hourly rate" refers to:

- A) Their take-home pay divided by hours worked
- B) What it actually costs the company per hour of their time — salary, benefits, and overhead included
- C) The rate they'd charge as an independent contractor
- D) Their salary divided by 2,080 (standard work-year hours), with nothing else added

---

**Q3.** Which contingency-sizing method ties the reserve's size directly to a project's own scored risk register, rather than an industry-norm percentage?

- A) The percentage-of-base method
- B) The risk-based method
- C) The EAC method
- D) The burn-rate method

---

**Q4.** Atlas records a $4,300 emergency vendor invoice **and** a $4,300 contingency draw for the same event. When computing total **actual cost (AC)** for a forecast, you should:

- A) Sum both — $8,600 total, since two rows were recorded
- B) Sum only the vendor invoice; the contingency row tracks reserve remaining, not a second expense
- C) Sum only the contingency draw; the vendor invoice was already covered
- D) Average the two rows

---

**Q5.** "Variance" in this week's sense is best defined as:

- A) Actual-to-date minus the full annual budget (BAC)
- B) Plan-to-date minus actual-to-date, at a specific point in the schedule
- C) The standard deviation of monthly spend
- D) Actual-to-date divided by BAC

---

**Q6.** Of PV, EV, and AC, which one requires knowing **what percentage of the planned scope is actually done**, not just what was spent or planned?

- A) PV (planned value)
- B) AC (actual cost)
- C) EV (earned value)
- D) None of them — all three are pure dollar totals

---

**Q7.** A project's CPI (cost performance index) is 0.85. This means:

- A) The project is 85% complete
- B) For every dollar spent, the team earned about 85 cents of planned value — costing more than expected per unit of real progress
- C) The project is 15% ahead of schedule
- D) 85% of the contingency reserve remains

---

**Q8.** SPI (schedule performance index) below 1.0 means:

- A) The project is over budget
- B) Less value has been earned than the schedule called for at this point — the project is running behind
- C) The team's rate has increased
- D) The contingency reserve has been fully drawn

---

**Q9.** Given `BAC = $200,000` and `CPI = 0.80`, the CPI-method EAC (estimate at completion) is:

- A) $160,000
- B) $200,000
- C) $250,000
- D) $280,000

---

**Q10.** TCPI (to-complete performance index) answers which specific question?

- A) How much of the budget has been spent so far
- B) What cost-efficiency the *remaining* work must achieve, from this point forward, to still land exactly on the original BAC
- C) How many story points remain
- D) What percentage of the team is over-allocated

---

**Q11.** In SQL, `SUM(SUM(a.amount)) OVER (PARTITION BY category ORDER BY month)` computes:

- A) A single grand total across all categories and months
- B) The average monthly spend per category
- C) A running (cumulative) total per category, ordered by month
- D) The variance between planned and actual spend

---

**Q12.** Why does checking allocation **one project at a time** miss a real over-allocation, even if each individual project's plan looks completely reasonable on its own?

- A) SQL can't `JOIN` across more than two tables
- B) A person's true availability is capped once, across their *whole* week — summing their commitment only within one project can't see a conflicting commitment recorded against a different project
- C) Percentages can't be summed in SQL
- D) It can't — checking one project at a time is always sufficient

---

**Q13.** Yuki Tanaka, a contractor, is allocated "100%" in October. Marcus Webb, an employee, is also allocated "100%" that month. Why might these represent a **different number of actual hours**?

- A) Contractors don't have a `weekly_capacity_hours` value
- B) Their `weekly_capacity_hours` values differ (30h vs. 40h), so the same percentage converts to different hours
- C) `allocated_pct` is meaningless for contractors
- D) It never does — 100% always means 40 hours in this schema

---

**Q14.** Which of the following is **one of the three specific spreadsheet failure modes** named in Lecture 2 §8?

- A) Spreadsheets cannot store dollar amounts with decimals
- B) A typo'd category value in a `SUMIF`/`VLOOKUP` formula silently returns a wrong total instead of erroring, because there's no real referential integrity
- C) Spreadsheets cannot be shared between more than one person
- D) Spreadsheets cannot compute percentages

---

## Answer key

<details>
<summary>Reveal after attempting</summary>

1. **C** — labor dominates almost every technology project's budget, usually 70–90% of the total, which is why Lecture 1 spends most of its time on rates and planned hours rather than tooling or vendor cost.
2. **B** — the fully loaded rate already includes benefits and overhead folded in by Finance; it's what the *company* pays per hour, not what the person takes home.
3. **B** — the risk-based method sizes contingency against specific, scored risk exposure (e.g., from a Week 6 risk register), which is more defensible in front of a skeptical CFO than a flat industry-norm percentage, though the percentage method (A) is a reasonable simpler starting point.
4. **B** — the vendor invoice is the real, single actual cost. The contingency row is a management record of *where the money came from*, not a second expense; summing both double-counts the same $4,300.
5. **B** — variance compares plan-to-date to actual-to-date at a specific point in the schedule, not against the full annual budget, which tells you almost nothing about whether this specific period ran hot.
6. **C** — EV (earned value) is `% scope complete × BAC`; PV and AC are both pure dollar totals from the time-phased plan and the ledger, respectively, needing no scope-completion input.
7. **B** — CPI = EV / AC. A CPI of 0.85 means every dollar spent bought about 85 cents of planned value — the project costs more than expected per unit of real progress. It says nothing directly about % complete (A) or schedule (C).
8. **B** — SPI = EV / PV. Below 1.0 means less value has been earned than the time-phased plan called for by this point — the project is behind schedule, independent of cost.
9. **C** — `EAC = BAC / CPI = 200,000 / 0.80 = 250,000`.
10. **B** — TCPI = `(BAC − EV) / (BAC − AC)`, and it answers exactly one question: what efficiency is required on the *remaining* work to still hit the original BAC. Comparing it to actual historical CPI tells you whether the original budget is still realistic.
11. **C** — the inner `SUM` groups rows into one per category/month; the outer `SUM() OVER (PARTITION BY ... ORDER BY ...)` computes a running cumulative total across those grouped rows, restarting for each category.
12. **B** — availability is a whole-person, whole-week constraint. Summing a person's commitment only within one project's rows can never see a second, conflicting commitment recorded against a different project — that's exactly how Sofia Reyes's November over-allocation stayed invisible to an Atlas-only view.
13. **B** — `allocated_pct` is always relative to that specific person's own `weekly_capacity_hours`. Yuki's 100% is 30 hours (her contracted cap); Marcus's 100% is 40 hours. Same percentage, different real commitment.
14. **B** — this is Failure 2 from Lecture 2 §8: a mistyped category string breaks a `SUMIF`/`VLOOKUP` silently, with no error, unlike a SQL foreign key which refuses a bad reference at write time.

</details>

**Scoring:** 12+ → start Week 10. 9–11 → re-read the lecture sections behind your misses. <9 → re-read all three lectures from the top; the EVM vocabulary (PV/EV/AC/CPI/SPI/TCPI/EAC) compounds directly into Challenge 1 and the mini-project.
