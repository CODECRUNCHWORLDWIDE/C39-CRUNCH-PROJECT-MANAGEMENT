# Challenge 1 — Diagnose a Team Where Scrum Has Become Theater

**Goal:** Practice the diagnostic skill Lecture 2 builds toward — telling real Scrum from a team that's copied its artifacts and lost every actual purpose behind them.

**Estimated time:** 90 minutes.

## Setup

Re-read Lecture 2 in full (all four ceremonies and their failure-mode "tells") and Lecture 1, §7 (the "we're Agile so we don't need a plan" trap) before starting. Create a file `diagnosis.md` in your portfolio at `c39-week-02/challenge-01/`.

## The scenario

**Solstice Analytics**, an unrelated company to Northlight, runs a six-person team called **Comet** that builds internal reporting tools. Comet has "done Scrum" for eight months. Here's what an embedded observer — you, brought in to advise — sees over one full sprint:

**Sprint planning (Monday, week 1):** The engineering manager, Raj, opens a spreadsheet (not the backlog tool) with a list he made over the weekend and reads it out: "okay, this sprint we're doing tickets 401 through 414." Nobody asks why those fourteen and not others. The product owner, who's on the call, doesn't speak except to say "sounds good" forty seconds in. No sprint goal is ever stated — when you ask Raj afterward what the sprint is *for*, he says "finishing the Q3 ticket list," which is a deadline, not a goal. Estimation doesn't happen; tickets don't have point values.

**Daily standup (every morning, 9:00am, 20–30 minutes):** Each of the six engineers, in turn, tells Raj what they did yesterday and what they'll do today. Raj asks follow-up questions ("why did that take so long?") to several people. Nobody else on the team asks anything. Twice this sprint, an engineer mentions being "a little stuck," Raj says "let's talk after standup," and — per two engineers you interview separately — that follow-up conversation never actually happens either time.

**Sprint review (Friday, week 2, 30 minutes):** Raj presents a slide deck summarizing what shipped. No live software is shown — "the demo environment's been flaky, so we just walk through it." The two stakeholders on the call (Raj's manager and someone from support) ask no questions and the call ends four minutes early. Nothing discussed changes what's planned for next sprint — that was already decided in a separate meeting Raj held with his manager on Thursday.

**Sprint retrospective (Friday, week 2, immediately after review, 15 minutes):** Raj asks "anything we should talk about?" The team is quiet. Someone says "I think we're doing okay." Raj says "cool, sounds like a good sprint" and ends the meeting. This is, per two engineers, close to verbatim what happens most sprints. You find last sprint's retro notes (a single line in a wiki page): "Team agreed to write better PR descriptions." No PR description has changed length or quality since.

**One more data point:** when you ask the team, privately, whether they'd describe Comet as "Agile," four of six say yes — because they run sprints, standups, and have a board with columns. Two say they're not sure what the alternative would even be.

## Tasks

1. **Diagnose each of the four ceremonies separately.** For each one, state clearly: is it functioning, broken, or theater? Cite the **specific evidence** from the scenario (a quote or detail) that supports your diagnosis, and name the matching "tell" from Lecture 2's failure-mode description for that ceremony.
2. **Identify the root cause, not just the four symptoms.** All four ceremonies being broken the same way (Raj deciding alone, the team staying passive) is not a coincidence — it's one underlying dynamic showing up four times. Name it in 2–3 sentences. (Hint: reread Lecture 2 §1 on what the Product Owner and Scrum Master roles are actually supposed to own, and ask who's holding those decisions at Comet instead.)
3. **Propose one concrete, specific fix per ceremony** — not "communicate better," an actual changed behavior someone would do differently starting next sprint. At least one fix must directly change something **Raj** does, since he's the common thread in every broken ceremony.
4. **Answer the trap question directly.** Using Lecture 1 §7's vocabulary, is Comet's problem "too much process" or "not enough real Agile practice underneath borrowed Agile terminology"? Defend your answer in 3–4 sentences using specific scenario evidence — resist the easy, wrong answer of "they should switch to Kanban" without first establishing whether the actual problem is Scrum's fault or Raj's implementation of it.

## What a strong answer contains

- Four separate, evidence-cited diagnoses (not one paragraph vaguely describing "bad culture").
- A root cause that's a specific *mechanism* ("Raj is making PO- and Scrum-Master-owned decisions unilaterally, so the ceremonies exist as reporting structures to him rather than team-owned processes"), not a vague description ("the team lacks psychological safety" is a real and related idea, but on its own it doesn't explain *why* — connect it to who's holding which decision).
- Fixes specific enough that you could tell, next sprint, whether they happened (same bar as Exercise 3's action items).
- A defended answer to the trap question that doesn't just recommend switching frameworks as an escape hatch — Lecture 2 §6 (Scrum vs. Kanban) exists for real fit reasons, and "swap frameworks" is rarely the actual fix for a team where the frameworks were never really run in the first place.

## Submission

Commit `diagnosis.md` to your portfolio under `c39-week-02/challenge-01/`.
