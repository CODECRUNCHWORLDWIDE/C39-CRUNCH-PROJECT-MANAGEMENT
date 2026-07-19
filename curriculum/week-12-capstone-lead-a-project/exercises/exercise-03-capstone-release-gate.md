# Exercise 3 — Define Your Capstone Release Gate and Rollback

**Skill:** Applying Lecture 3, §1–2 — writing and running an explicit release gate, and writing and testing a rollback plan, before shipping.

**Estimated time:** 1 hour. Do this Friday, before you tag and push your release.

---

## Setup

You need a mostly-complete capstone (most `must` items `Done` or close to it) and a git repo you can tag and push to.

---

## Part A — Write the gate (15 min)

In `release-gate.md`, write your gate as a literal, ordered checklist — copy Lecture 3, §1's structure and adapt it to your project specifically:

1. A SQL query proving every `must` item is `Done`.
2. The exact command(s) for a clean-checkout test.
3. The specific acceptance criteria you'll re-check (list them by backlog item key, not vaguely).
4. A line confirming your charter's scope-out list still matches reality.

## Part B — Run the gate for real (15 min)

Actually run it. Paste the SQL query's real output (not a hypothetical) into `release-gate.md`:

```sql
SELECT item_key, title, current_status
FROM capstone_items
WHERE priority = 'must' AND current_status != 'Done';
```

If this returns any rows, **the gate fails — do not proceed to Part C.** Go finish those items, or make the deliberate, documented call to descope them (updating the charter, not just skipping the check).

Then do the clean-checkout test for real:

```bash
cd /tmp
git clone <your-repo-url> capstone-gate-test
cd capstone-gate-test
# run your actual install + usage instructions exactly as your README states them
```

Note in `release-gate.md` whether this worked with zero manual fixes. If it didn't, fix the README or the setup — a gate that "passes" only because you know the undocumented extra step isn't a passing gate.

## Part C — Write and test the rollback plan (20 min)

In `ROLLBACK.md`:

1. Tag your current last-known-good state (Lecture 3, §2, step 1).
2. Write the exact rollback commands.
3. **Actually run the rollback drill** on a throwaway branch (Lecture 3, §2, step 3) and note in `ROLLBACK.md` that you did, with the date.

```bash
git tag -a v0.9-pre-release -m "Last verified state before release"
git push origin v0.9-pre-release

git checkout -b rollback-drill
git checkout v0.9-pre-release -- .
# confirm it still runs
git checkout main
git branch -D rollback-drill
```

## Part D — Ship (10 min)

With the gate passed and the rollback tested:

```bash
git tag -a v1.0 -m "Capstone release"
git push origin v1.0
git push origin main
```

Update `capstone_items` to reflect final reality if anything changed during the gate.

---

## Expected outcome

`release-gate.md` with a real (not hypothetical) passing SQL query result and clean-checkout confirmation, `ROLLBACK.md` with a rollback plan you've actually run once, and a pushed `v1.0` tag in your repo.

## Common mistakes

- **Writing the gate and skipping straight to shipping** without actually running the SQL query and clean-checkout test. The exercise is running the gate, not describing one you plan to run.
- **Testing the rollback by reading the commands instead of executing them.** A rollback plan you've only read is a plan you're guessing works — Lecture 3, §2 is explicit that this is the whole point of the drill.
- **Tagging `v1.0` before the gate passes**, "because it's basically done." A gate you can bypass under time pressure isn't a gate — that's exactly the failure mode Week 10 and this exercise both exist to prevent.
