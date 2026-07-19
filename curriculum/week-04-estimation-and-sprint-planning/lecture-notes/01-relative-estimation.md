# Lecture 1 — Why Relative Estimation Works

> **Duration:** ~2 hours. **Outcome:** You can explain why absolute-hour estimates fail on knowledge work, size a story relative to a known reference story instead of guessing hours, and run a planning-poker round that surfaces disagreement before the sprint starts.

Ten candidate stories are sitting in `sprint3_candidates`, all with `story_points = NULL`. Elena wants to know what fits in Sprint 3. The honest, useful answer starts with a size on each story — and this lecture is about getting that size right, using a technique that's deliberately *not* "how many hours will this take."

## 1. Why "how many hours will this take?" is the wrong question

It feels like the right question. It is almost always the wrong one, for three compounding reasons.

**Reason one: humans are bad at estimating absolute duration, specifically in the optimistic direction.** Ask an engineer "how long will this take?" and they picture the work going *the way they expect it to go* — no surprise dependency, no flaky test, no design question that needs Elena's input and Elena is in a different timezone for two days. That mental simulation is real work, correctly estimated — it's just not the work that's actually going to happen. Daniel Kahneman and Amos Tversky named this the **planning fallacy**: estimates of task duration are reliably optimistic even when the estimator has direct, personal experience of *exactly this kind of task running long before*. It's not inexperience. It's a structural feature of how people simulate the future — you'll go much deeper on this in Lecture 2.

**Reason two: hour estimates anchor on the wrong thing and then don't get revisited.** Say Wes estimates "Bulk-invite teammates via CSV" at 6 hours. That number gets written into a plan, and from that point on it behaves like a fact rather than a guess — nobody re-derives it, and if the actual work takes 14 hours, the gap reads as "Wes was slow," when the real story is "the estimate was a guess with no error bars, made before anyone had opened the CSV-parsing library's docs." Hour estimates carry an unearned aura of precision that a size like "medium" simply doesn't.

**Reason three: hour estimates don't compose across a team the way you'd want.** If Priya estimates a story at "2 days" and Sam estimates a similar-looking one at "2 days," are those the same 2 days? Priya works faster on backend code, Sam is faster on the exact kind of UI work in question — "2 days" quietly encodes *who does it*, which defeats the entire point of a backlog estimate that's supposed to help you plan before you know who's picking up what.

## 2. The alternative: size relative to a reference, not absolute to a clock

**Story points** measure *relative size* — how big is this story compared to a story you already understand — not *duration*. The unit isn't hours; it's "how much bigger or smaller is this than something we've already built."

This is a genuinely different question, and it's one people are much better at answering. You already do this constantly outside of software: you can't tell me exactly how many minutes it'll take to walk to three different stores, but you can tell me instantly which of the three is farthest. Relative judgment is a more reliable human skill than absolute estimation, and story points are built to exploit that.

**The anchor: your own shipped history.** Atlas isn't sizing in a vacuum — `atlas_story_history` has sixteen real, already-built stories with points already agreed on. When the team sizes "Bulk-invite teammates via CSV," the real question isn't "how many hours" — it's **"is this bigger or smaller than 'Invite a teammate by email' (3 points), and how does it compare to 'Shared editable dashboard view' (5 points)?"** CSV parsing plus the *existing* invite flow feels closer to the 3 — it reuses a proven code path and adds one well-understood piece (a file parser) — so the team converges around 3, maybe 5 if the CSV format has edge cases nobody's scoped yet. That comparison is fast, concrete, and grounded in work the team has actually felt the size of — not a number pulled from nothing.

```sql
-- Pull the team's own reference stories before a pointing session —
-- this is the calibration set every new estimate gets compared against.
SELECT title, story_points
FROM atlas_story_history
ORDER BY story_points, title;
```

## 3. The scale: modified Fibonacci, and why it isn't linear

Most teams size with a **modified Fibonacci sequence**: `1, 2, 3, 5, 8, 13, 20, 40, 100` (some teams add a `½` for trivial work, and a `∞`/"too big to size" card for anything that needs to be split first). Atlas uses this scale.

The gaps *widening* as the numbers grow is the entire design, not an accident. At small sizes (1, 2, 3) the team can be precise — the difference between a 2 and a 3 is real and worth debating. At large sizes (20 vs. 40), true precision is impossible anyway — nobody can reliably tell you a story is "37% bigger" than another — so the scale stops pretending. A big gap between 13 and 20 is the scale itself telling you: *if you're arguing between these two, you don't actually know enough yet — that's a signal to split the story, not to negotiate a number.*

**A point is not a fixed duration**, and this is the single most important discipline in the whole system. A "3" for Atlas's team is whatever "3" has historically meant for *this specific team*, calibrated against *its own* history — a fact you'll compute precisely in Lecture 2. Move the same "3-point" story to a different team with different tools, different experience, different codebase familiarity, and its real duration changes even though the *relative* size — "about like inviting a teammate by email" — doesn't. That's a feature: points travel with the team, not with the calendar.

## 4. Planning poker: the mechanism

**Planning poker** (popularized by Mike Cohn and James Grenning) is the structured way a team actually produces these estimates together, and its design solves a specific problem: **if you ask "how big is this?" out loud, the first confident answer anchors everyone else's.** Someone senior says "probably a 3" and every other estimate that follows is unconsciously pulled toward 3, whether or not it's right. Poker breaks that by forcing **simultaneous, independent** votes.

**The mechanics:**

1. **The story is read and discussed.** The person who wrote it (often the PM or PO, sometimes an engineer) reads the card, answers clarifying questions. No sizing talk yet.
2. **Everyone picks a card, face down (or a poker app, hand hidden).** Each estimator — usually everyone who might do the work — privately chooses a number from the Fibonacci scale that represents their honest size judgment.
3. **Everyone reveals simultaneously.** No sequencing, no "you go first." This is the whole point of the exercise.
4. **If everyone agrees (or is within one step), the number sticks.** Fast, and it usually happens on the easy stories.
5. **If there's a spread — say Sam plays 3 and Priya plays 13 — that's not noise, that's signal.** The facilitator asks the low and high voters to explain their reasoning, *not* to justify a compromise. Sam might say "I did the CSV parser for the export feature already, this reuses most of it." Priya might say "I don't think the CSV format's been agreed with Elena yet — could need per-row validation and a whole error-reporting UI we haven't scoped." **Both are right about what they know.** The disagreement just revealed that the story is under-specified — exactly the kind of hidden complexity an hour-guess would have silently baked in wrong.
6. **Re-vote after the discussion**, usually 1–2 more rounds. Convergence, or an explicit decision to split the story or take an action item ("Priya will confirm the CSV format with Elena before we finalize this") — never force a number nobody actually believes just to move on.

## 5. Storing the session as data, not a memory

A poker round produces exactly the kind of thing this course insists you keep in a real table, not a sticky note that gets thrown away after the meeting: every voter's number, every round, and the final size. That history is what makes Lecture 2's bias-correction work possible, and it's what lets you answer "did our estimates get less accurate over time?" (Challenge 1) with a query instead of a vague memory.

```sql
CREATE TABLE poker_rounds (
    round_id    INTEGER PRIMARY KEY,
    item_id     INTEGER NOT NULL,   -- references sprint3_candidates.item_id
    round_num   INTEGER NOT NULL,   -- 1st vote, 2nd vote (after discussion), ...
    voter       TEXT    NOT NULL,
    points_cast INTEGER NOT NULL
);

-- Example: round 1 on item 24 (Bulk-invite via CSV) shows real spread
INSERT INTO poker_rounds VALUES
(1,24,1,'Priya N.',5),
(2,24,1,'Sam O.',3),
(3,24,1,'Dana K.',13),
(4,24,1,'Wes T.',5);

-- Which items had the widest first-round disagreement — those are the ones
-- with hidden complexity worth discussing, exactly the cases poker is for.
SELECT item_id,
       MAX(points_cast) - MIN(points_cast) AS spread,
       COUNT(DISTINCT voter) AS voters
FROM poker_rounds
WHERE round_num = 1
GROUP BY item_id
ORDER BY spread DESC;
```

Dana's `13` next to Sam's `3` on the same story is a four-times spread on the very first vote — a textbook signal to stop and talk, not to average the four numbers and move on (averaging erases exactly the information poker was designed to surface).

## 6. Common failure modes — know these before you run your first session

- **Points quietly become hours in people's heads.** The moment someone says "a point is about half a day," the whole relative-sizing benefit evaporates and you're back to hour-guessing with extra steps. Points are relative; only *velocity* (points delivered per sprint, Lecture 3) converts them back to a real-world pace, and that conversion is a team-specific, empirically-measured number — never a fixed rule of thumb.
- **Skipping the discussion round when there's a spread.** Averaging a 3 and a 13 to "call it an 8" throws away the one piece of information the whole exercise exists to surface: that two competent people see this story completely differently, which means it isn't understood yet.
- **Letting the most senior voice go first, out loud, before the reveal.** Reintroduces anchoring through the back door. Simultaneous reveal isn't a formality — it's the mechanism.
- **Sizing without a reference set in front of the team.** "Is this a 3 or a 5?" is nearly impossible to answer from nothing. "Is this bigger or smaller than 'Invite a teammate by email,' which we agreed was a 3?" is answerable in seconds. Always point with the history table open.

## 7. Check yourself

- Name the three reasons hour estimates fail, in your own words.
- Why does the Fibonacci scale's gap *widen* at higher numbers instead of staying constant?
- What specific problem does "everyone reveals simultaneously" solve, and what happens if you skip it?
- A round comes back 2, 3, 3, 13. What's the *wrong* move, and what's the right one?
- Why is "a point is about half a day" a dangerous sentence for a team to start believing?

If those are automatic, Lecture 2 goes into exactly *how much* to trust — or discount — the resulting estimate, using the team's own history and an outside reference class.

## Further reading

- **Mike Cohn — "Agile Estimating and Planning"** (the book that popularized story points and planning poker): <https://www.mountaingoatsoftware.com/books/agile-estimating-and-planning>
- **Mountain Goat Software — "What is Planning Poker?":** <https://www.mountaingoatsoftware.com/agile/planning-poker>
- **Scrum.org — "Story Points, Velocity, and Fibonacci":** <https://www.scrum.org/resources/blog/what-are-story-points>
