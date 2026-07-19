# Week 8 — Quiz

Fifteen questions. Lectures closed. Aim for 13/15 before starting Week 9. A mix of multiple-choice and short "what does this compute?" — the answer key explains the *why*, not just the letter.

---

**Q1.** Which pair of metrics is correctly matched to its definition?

- A) Velocity = count of issues finished per week; throughput = story points finished per sprint
- B) Velocity = story points finished per sprint; throughput = count of issues finished per week
- C) They're two names for exactly the same computation
- D) Velocity is a pandas concept; throughput is a SQL concept

---

**Q2.** An issue sat in the backlog for 40 days, then took 3 days once someone actually started it. Its **cycle time** is:

- A) 43 days
- B) 40 days
- C) 3 days
- D) Undefined without a story-point estimate

---

**Q3.** Which of these does **not** belong in a WIP count?

- A) An issue currently `In Progress`
- B) An issue currently `Blocked`
- C) An issue currently `In Review`
- D) An issue currently `To Do`, not yet started

---

**Q4.** Little's Law states:

- A) `Throughput = WIP × Cycle Time`
- B) `WIP = Throughput × Cycle Time`
- C) `Cycle Time = Throughput × WIP²`
- D) `WIP = Throughput ÷ Velocity`

---

**Q5.** A team's throughput stays flat for two months. Which of the following would still be consistent with "the team is being honest about its numbers," and which would be a red flag? *(Pick the one true statement.)*

- A) Throughput is immune to gaming by definition — nothing to watch for
- B) A team could hold throughput flat while quietly swapping big, valuable work for many small, easy tickets — the count looks the same, the delivered value doesn't
- C) Throughput can only go up over time in a healthy team
- D) Flat throughput always means the team is blocked

---

**Q6.** In the Atlas data, why does `ATLAS-126`'s blocked time **not** appear in the plain `SUM(left_at - entered_at)` blocked-time query from Lecture 2?

- A) `ATLAS-126` was never actually blocked
- B) Its `left_at` is `NULL` (still blocked, ongoing), so the subtraction is `NULL` and `SUM` silently ignores it
- C) `SUM` doesn't work on date columns
- D) The query has a syntax error

---

**Q7.** Which SQL clause would you use to keep only sprints whose `SUM(story_points)` exceeds 15, and why not the other one?

- A) `WHERE`, because it's evaluated first
- B) `HAVING`, because it filters after `GROUP BY` has produced the aggregate
- C) `ORDER BY`, because sorting implies filtering
- D) Either works identically

---

**Q8.** `SUM(points_done) OVER (ORDER BY day)` with no explicit frame computes, by default:

- A) The total across all rows, repeated on every row
- B) A running total from the start through the current row
- C) Only the current row's value
- D) An error — a frame is required

---

**Q9.** Atlas's 23 completed issues have a mean cycle time of 7.57 days and a median (p50) of 8 days, but a p85 of 10 days. What does the *gap* between p50 and p85 tell you that the mean cannot?

- A) Nothing — p50 and the mean are the same thing
- B) That the data is corrupted
- C) That there's a meaningful tail — a real fraction of issues take noticeably longer than the "typical" case, information a single average would flatten out
- D) That every issue takes between 7.57 and 8 days

---

**Q10.** Atlas's naive (unweighted) average flow efficiency across all completed issues is 89.5%. Segmented, "never blocked" issues average 100% and "was blocked" issues average ~51.5%. What's the correct lesson?

- A) The naive average was fine all along; segmentation just added noise
- B) Averaging a mostly-perfect population together with a small, badly-affected population produces a comfortable-looking number that hides the real, specific problem
- C) Flow efficiency should never be computed for a mixed population
- D) 89.5% and 51.5% are actually the same number, differently rounded

---

**Q11.** On a cumulative flow diagram, a WIP band (the region between "started" and "done") that keeps **widening** over time, without ever narrowing back down, is the visual signature of:

- A) A team accelerating
- B) A team accepting new work faster than it's finishing old work — cycle time is about to get worse for whatever's stuck in that band
- C) A perfectly healthy backlog
- D) A charting error — bands can't widen

---

**Q12.** Why is a spreadsheet a poor fit for an issue export with a full status-change history, compared to a relational database?

- A) Spreadsheets can't store dates
- B) A one-issue-to-many-status-changes relationship has no clean native representation in a spreadsheet without duplicating fields or maintaining lookups by hand — a database expresses and enforces it directly
- C) Spreadsheets are always slower for any dataset, regardless of size
- D) SQL is required by law for project data

---

**Q13.** In PostgreSQL, `MIN(entered_at) FILTER (WHERE status = 'In Progress')` computes:

- A) The minimum `entered_at` across *all* rows, ignoring the filter
- B) The minimum `entered_at` only among rows where `status = 'In Progress'`
- C) A syntax error — `FILTER` doesn't exist in SQL
- D) The same thing as `WHERE status = 'In Progress'` placed on the outer query, with no difference ever

---

**Q14.** A sponsor asks you to make "cycle time" an individual performance target for each engineer, scored monthly. What's the most likely unintended effect, based on this week's "signals, not targets" lesson?

- A) None — cycle time is ungameable
- B) Engineers may start declining or deprioritizing genuinely hard, valuable, or dependency-heavy work in favor of small, fast tickets that make their personal number look good
- C) Cycle time would become more accurate once it's a target
- D) The team's velocity would become irrelevant

---

**Q15.** What is the general SQL predicate pattern for "was this interval open at time X," used repeatedly this week for point-in-time WIP and status-duration queries?

- A) `status = 'In Progress'`
- B) `entered_at <= X AND (left_at IS NULL OR left_at > X)`
- C) `entered_at = X`
- D) `left_at IS NOT NULL`

---

## Answer key

<details>
<summary>Reveal after attempting</summary>

1. **B** — velocity is points-per-sprint (needs estimates and sprints); throughput is issue-count-per-week (needs neither).
2. **C** — cycle time starts when work actually begins (first `In Progress`), not when the ticket was created. The 40 backlog days are *lead time*, not cycle time.
3. **D** — `To Do` means not yet started; WIP is specifically "started but not finished." `Blocked` **does** count — it's still open, unfinished work, even though no one is actively typing.
4. **B** — `WIP = Throughput × Cycle Time`. Rearranged: `Cycle Time = WIP / Throughput` — more WIP at fixed throughput directly produces longer cycle time.
5. **B** — throughput is harder to game than velocity but not immune: swapping large valuable work for many small easy tickets keeps the *count* flat (or even rising) while real delivered value drops.
6. **B** — `left_at` is `NULL` for the still-open `Blocked` interval; `left_at - entered_at` evaluates to `NULL`, and `SUM` silently skips `NULL` values rather than erroring, so the row contributes zero instead of its true (ongoing) duration.
7. **B** — `HAVING` filters on the result of the aggregation, which hasn't been computed yet when `WHERE` runs (recall the C33 Week 1 evaluation-order table: `WHERE` before grouping, `HAVING` after).
8. **B** — the default frame for an `ORDER BY`-only window is "everything from the start through the current row," which is exactly a running total — the shape a burndown/burnup needs.
9. **C** — the mean and median being close can hide a real tail; p85 = 10 shows a meaningful chunk of issues run noticeably longer than the "typical" 8-day case, which a single averaged number flattens away.
10. **B** — this is the segmentation trap from Lecture 3: averaging 18 perfect (100%) issues with 5 badly-affected (~51.5%) issues produces a falsely reassuring 89.5% that hides a real, specific, fixable problem.
11. **B** — a widening, non-draining WIP band means new work keeps entering faster than old work leaves; by Little's Law, that guarantees rising cycle time for whatever's stuck inside it.
12. **B** — the one-to-many issue/status-history relationship has no clean, enforced spreadsheet representation; a database expresses it in two `CREATE TABLE` statements and a foreign key that can't silently drift.
13. **B** — `FILTER (WHERE ...)` restricts an aggregate function to only the matching rows, computed within the same `GROUP BY` — both PostgreSQL and SQLite (3.30+) support it.
14. **B** — turning a flow metric into an individual target is the textbook Goodhart's Law trap: people optimize the visible number (easy, low-risk tickets) rather than the underlying goal (genuinely fast, healthy delivery of real work).
15. **B** — `entered_at <= X AND (left_at IS NULL OR left_at > X)` is the general "was this interval open at time X" test, used for point-in-time WIP, blocked-duration, and any other interval-membership question.

</details>

**Scoring:** 13+ → start Week 9. 10–12 → re-read the lecture sections behind your misses. <10 → re-read all three lectures from the top; Week 9's budgeting math assumes this week's flow vocabulary is solid.
