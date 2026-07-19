# Week 11 — Challenges

Two open-ended problems, harder and less prescriptive than the exercises. Do them after all three exercises — both assume you're fluent in this week's data (`board_items`, `jira_issues`) and the grounded-prompt pattern from Lecture 3.

1. **[Challenge 1 — Migrate a project from Jira to GitHub Projects](challenge-01-migrate-jira-to-github.md)** — take PLAT's real backlog and design (and script) its move into `board_items`' schema, honestly reconciling what doesn't map cleanly. *(~2h)*
2. **[Challenge 2 — Build an AI status digest that will not fabricate](challenge-02-ai-status-with-guardrails.md)** — implement the grounded-prompt pipeline end to end, then deliberately try to break it and fix what breaks. *(~1.5h)*

## How these are judged

Neither challenge has a single right answer with a row-count key. You're being judged on:

- **Honesty about loss** — a migration or a mapping that pretends nothing was lost is worse than one that names exactly what didn't survive and why.
- **Working code, not just a plan** — both challenges expect a runnable script (SQL + Python), not a design document.
- **Adversarial thinking** — Challenge 2 specifically expects you to try to break your own guardrail before you're done, not just build the happy path.

Keep your work in `challenge-01/` and `challenge-02/` folders with your scripts **and** a short written explanation. The explanation is graded as heavily as the code.
