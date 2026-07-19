# Week 6 — Homework

Five problems, ~4 hours total, spread across the week. These reinforce the lectures with a mix of applied SQL/Python work, structured analysis, and one interview-style reflection. Commit each.

Problems reference Project Atlas and Northlight Software (see [Lecture 1](./lecture-notes/01-risk-registers-and-raid.md) for the cast) unless a problem says otherwise.

---

## Problem 1 — Interview someone about a real risk or dependency (60 min)

Risk and dependency management is learned partly by watching real ones unfold. Pick **one**:

- **(A)** Find someone in your network who has run or worked on a real project (a PM, engineer, designer, or team lead — formal title not required) and ask: "Tell me about a risk that materialized and caught the team off guard — what would a live risk register have changed?" and "Describe a cross-team dependency that blocked you, and how you finally got unblocked." Write up their answers plus one thing that surprised you, in `interview.md`.
- **(B)** If you can't find someone to interview, find a **public postmortem** of a project or incident (search "[company] postmortem" or "incident retrospective") and analyze it through this week's lens instead: was there an unmanaged risk that, scored honestly using Lecture 1 §3's scale, should have been flagged earlier? Was there a cross-team dependency chain that stayed invisible? Write this analysis in `postmortem-analysis.md` (250–400 words), citing the source.

---

## Problem 2 — Score and log your own real risks (45 min)

In `my-risks.sql`, using this week's `risks` schema, log **5 real risks** from a project you're actually part of (work, school, open source, personal). Score each, assign a real or role-based owner, and assign a response type and plan. This is the same discipline as Exercise 1–2, but with no answer key — the check is whether you could defend every score and plan to someone who knows the project as well as you do.

---

## Problem 3 — Trace one real dependency chain (45 min)

Outside of this week's challenges, find (or reconstruct from memory) **one real dependency chain** you've experienced — something you needed that depended on something someone else needed, at least two hops deep. In `real-dependency-chain.md`, map it (a simple ordered list is fine if you don't want to build a full SQL table for this one), and write 3–4 sentences on whether it was visible to you early or discovered late, and what Lecture 2 §4's three coupling-reduction moves (resequence, fallback, own the interface) would have suggested, in hindsight.

---

## Problem 4 — The critical path in your own words (30 min)

In `critical-path-reflection.md` (200–300 words), answer: think of a time (work, school, or even a non-technical example like planning an event) where you treated every remaining task as equally urgent because you hadn't actually mapped which ones had slack. What would knowing the critical path have changed about where you spent your effort in the final stretch?

---

## Problem 5 — Compute a tiny critical path by hand (60 min)

Not every schedule needs a Python script. In `tiny-critical-path.md`, invent (or use a real) 5-task schedule for something small and concrete — planning a weekend trip, running a small event, a short coding side-project — with realistic durations and at least one task that depends on two others (so the "maximum, not average" rule from Lecture 3 §3 actually gets exercised). Compute the forward pass, backward pass, slack, and critical path fully by hand, and state which task in your plan you'd protect first if you only had time to protect one.

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
