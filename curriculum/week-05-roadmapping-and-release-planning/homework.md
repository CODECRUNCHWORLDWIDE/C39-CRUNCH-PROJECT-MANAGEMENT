# Week 5 — Homework

Five problems, ~4.5 hours total, spread across the week. A mix of applied SQL/Python, structured writing, and one interview-style reflection. Commit each.

All problems reference Northlight Software and the Q3 roadmap dataset from Lectures 1–3 unless a problem says otherwise.

---

## Problem 1 — Find a real public roadmap and grade it (60 min)

Find a **public product roadmap** from a real company (many SaaS companies publish one — search "[company] public roadmap" or check GitHub's, Linear's, or a tool you personally use). In `roadmap-review.md` (300–400 words):

- Is it feature-framed or outcome-framed (Lecture 1, §3)? Quote one item as evidence.
- Does it use a now/next/later structure or something else (fixed quarters, a simple list, a Kanban board)? Evaluate whether its structure honestly communicates confidence, per Lecture 1, §4.
- Find (or infer) one item that looks like it's been sitting unshipped for a long time. Per Lecture 1, §7, what failure mode does that risk, and what would you change about how it's presented?

---

## Problem 2 — Recompute Northlight's capacity under a different buffer (45 min)

Lecture 2, §3 used a 15% capacity buffer. In `buffer-sensitivity.md`, using SQL or Python, recompute `planning_velocity` and total Q3 capacity under **three** buffer assumptions: 10%, 15% (the lecture's baseline), and 25%. Show all three calculations. Then answer in 3–4 sentences: at a 25% buffer, does the original Now-bucket cutoff from Lecture 1, §5 still hold (items 1–5, cumulative 108)? What's the smallest buffer that still keeps items 1–5 inside capacity?

---

## Problem 3 — Write outcome statements for three vague roadmap items (45 min)

Outside this week's exercises, find or invent **three vague roadmap items** you've actually seen (things like "Improve performance," "Redesign settings page," "AI features") — from a real product, your own work, or a plausible invention. In `vague-to-outcome.md`, for each: state the vague item, then rewrite it as a proper outcome statement (Lecture 1, §3) — the thing a non-technical stakeholder could evaluate as true or false — and name one specific initiative that *might* deliver that outcome, making clear the outcome is the commitment and the initiative is just today's best guess.

---

## Problem 4 — The fixed-scope/fixed-date decision in your own words (30 min)

In `release-type-reflection.md` (200–300 words): think of a real deadline you've worked against (school, work, a personal project — anything with a genuine external date). Was it actually fixed-date (the date truly couldn't move) or was it treated as fixed-date without really being one (Lecture 2, §6)? What got cut, if anything, and was that cut a visible, deliberate decision or something that happened quietly? What would you do differently now, using this week's vocabulary?

---

## Problem 5 — Interview someone who's shipped a slipped release, or analyze one publicly (60 min)

Real judgment about slips is learned partly by watching it happen. Pick **one**:

- **(A)** Find someone who has managed a release that slipped (a PM, an EM, a lead — even informally) and ask: "Tell me about a time a fixed date and a slip collided — what actually got cut, and who decided?" and "How early did you tell people it was at risk?" Write up their answers and one thing that surprised you in `interview.md`.
- **(B)** If you can't find someone, find a **public account** of a shipped-late product or missed deadline (a postmortem, a company blog post, a well-documented news story) and analyze it through this week's lens in `slip-analysis.md` (250–400 words): using Lecture 3, §2's threshold test, should this have triggered a re-plan earlier than it apparently did? What does the public account suggest about *how* the slip was communicated — early and small, or late and large?

---

## Time budget

| Problem | Time |
|--------:|-----:|
| 1 | 60 min |
| 2 | 45 min |
| 3 | 45 min |
| 4 | 30 min |
| 5 | 60 min |
| **Total** | **~4 h** |

After homework, take the [quiz](./quiz.md) and ship the [mini-project](./mini-project/README.md) if you haven't already.
