# Week 8 — Challenges

Two open-ended problems. Unlike the exercises, these don't have one checkable numeric answer for every step — the skill being tested is judgment: how you package data for reuse, and how you build and defend a diagnosis. Do both after the three exercises.

1. **[Challenge 1 — Build a flow dashboard](challenge-01-build-a-flow-dashboard.md)** — package this week's queries into a reusable, date-independent set of SQL views instead of one-off scripts. *(~90 min.)*
2. **[Challenge 2 — Diagnose a stalling team](challenge-02-diagnose-a-stalling-team.md)** — use Atlas's real data to answer, with evidence, whether the team is actually slowing down and why. *(~90 min.)*

## How these are judged

There's no answer key with a single correct number for every line. Instead, each challenge states what a *strong* submission looks like. You're being graded on:

- **Reusability (Challenge 1)** — do your views still work next sprint, or did you quietly hardcode today's date?
- **Evidence discipline (Challenge 2)** — is every claim backed by a specific, reproducible query, or is it a hunch wearing a SQL costume?
- **Honesty over tidiness** — a strong diagnosis reports a messy or inconclusive result exactly as messy or inconclusive as it actually is, rather than forcing a clean story the data doesn't fully support.

Keep your work in `dashboard.sql` / `dashboard-notes.md` and `diagnosis.md` respectively. The reasoning is the point — a working query with no stated interpretation is an incomplete answer.
