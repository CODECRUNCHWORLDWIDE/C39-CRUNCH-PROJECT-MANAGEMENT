# Exercise 1 — Point a Backlog with Planning Poker

**Goal:** Size all 10 Sprint 3 candidates using relative estimation against Atlas's real history, and run a simulated planning-poker round (you play all four voters) on the two most disagreement-prone stories.

**Estimated time:** 60 minutes.

## Setup

Confirm your data's in place:

```sql
SELECT COUNT(*) FROM atlas_story_history;   -- must print 16
SELECT COUNT(*) FROM sprint3_candidates;    -- must print 10
```

Pull the reference set and keep it visible the whole exercise — you cannot size relatively with it closed:

```sql
SELECT title, story_points FROM atlas_story_history ORDER BY story_points, title;
```

Create `exercise-01.sql` for your queries and `exercise-01-notes.md` for your written reasoning (Part B needs prose, not just SQL).

## Part A — Size all 10 candidates by comparison (30 min)

For **each** of the 10 rows in `sprint3_candidates`, write one sentence naming the *closest reference story* from `atlas_story_history` and whether the candidate is smaller, about the same, or bigger — then assign a Fibonacci point value (`1, 2, 3, 5, 8, 13`). Put your reasoning in `exercise-01-notes.md`, one entry per item, e.g.:

> **#24 — Bulk-invite teammates via CSV.** Closest reference: "Invite a teammate by email" (3 pts) — reuses the same invite pipeline, adds one well-understood piece (a CSV parser). Slightly bigger than the reference because of file-format edge cases. **5 points.**

Some starting hints (yours don't have to match — defend your own reasoning):

- **#27 — Spike: evaluate real-time delivery for comments.** A spike is time-boxed *research*, not a shippable feature — there's no reference story of the same shape in the history table. How do you size something you can't compare to prior *delivered* work? (This is a real, common judgment call — state your approach.)
- **#26 — Transfer workspace ownership.** Small code change, but "high blast-radius if wrong" per the notes column. Does risk belong in the point size, or somewhere else? Defend your choice either way.
- **#28 — Fix: notification email sent twice on rapid replies.** Compare to `#19` in the history table (`invite email fails for + addresses`, 1 point estimated, 2 actual days — a 2x overrun). What does that history tell you about sizing "small-looking" bugs?

Once you've reasoned through all 10, record your final sizes:

```sql
UPDATE sprint3_candidates SET story_points = 5  WHERE item_id = 24;  -- example; use YOUR numbers
-- ... one UPDATE per item_id, all 10
```

## Part B — Run a real poker round on the two hardest items (20 min)

Pick the two items from Part A where you felt **least confident**, or where you can imagine a teammate reasonably disagreeing with you by more than one Fibonacci step. For each, play all **four** voters yourself (Priya N., Sam O., Dana K., Wes T.) — genuinely try to inhabit a different perspective for each vote, not just repeat your Part A number four times. Record every vote:

```sql
CREATE TABLE poker_rounds (
    round_id    INTEGER PRIMARY KEY,
    item_id     INTEGER NOT NULL,
    round_num   INTEGER NOT NULL,
    voter       TEXT    NOT NULL,
    points_cast INTEGER NOT NULL
);

INSERT INTO poker_rounds (item_id, round_num, voter, points_cast) VALUES
(/* item_id */ 0, 1, 'Priya N.', 0),
(0, 1, 'Sam O.', 0),
(0, 1, 'Dana K.', 0),
(0, 1, 'Wes T.', 0);
-- repeat for your second chosen item
```

Query the spread:

```sql
SELECT item_id, MAX(points_cast) - MIN(points_cast) AS spread
FROM poker_rounds
WHERE round_num = 1
GROUP BY item_id;
```

If the spread is 0 or 1 step, that's a legitimate (if slightly suspicious) outcome — note in `exercise-01-notes.md` whether you think you genuinely reasoned four different ways or defaulted to agreeing with yourself. If the spread is wider, write the "discussion" each voter would offer (in character, 1–2 sentences each) and record a round 2 vote that converges.

## Done when…

- [ ] All 10 rows in `sprint3_candidates` have a non-`NULL` `story_points`, each backed by a one-sentence comparison in `exercise-01-notes.md`.
- [ ] Two items have a full 4-voter `poker_rounds` entry for round 1, with a spread computed.
- [ ] You've written, in your own words, how you sized the spike (#27) with no directly comparable shipped story.
- [ ] Your final points are internally consistent — nothing you called "clearly smaller than a 5-point reference" ended up pointed at 8 or higher.

## Stretch

- Query which of your 10 new points, if any, exceed `13` — a size that, per Lecture 1, is a signal the story may need splitting rather than sizing. If any do, write one sentence on how you'd slice it (Week 3's INVEST/vertical-slicing skills apply directly here).
- Compare your point *total* across all 10 items to Sprint 1's real velocity (28 points) and Sprint 2's (22 points) — does the shape of this new set feel lighter, heavier, or about the same? You'll use this instinct as a sanity check in the mini-project.

## Submission

Commit `exercise-01.sql` and `exercise-01-notes.md` to your portfolio under `c39-week-04/exercise-01/`.
