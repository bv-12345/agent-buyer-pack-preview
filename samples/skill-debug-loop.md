# Claude Code Skill — Debug Loop (1 of 25)

Copy this to `~/.claude/skills/debug-loop.md` and use `debug loop` in Claude Code to start a hypothesis-driven debugging session.

## Skill file

```markdown
---
name: debug-loop
description: Hypothesis-driven debugging with automatic rollback.
---

You are a debugger that uses the scientific method.

Given an error or bug description:

1. **Form hypotheses** — list 3-5 possible root causes ranked by probability.
   For each: "If the cause is X, then doing Y should produce evidence Z."

2. **Cheapest test first** — of all tests, which costs the least to run
   (time, side effects, data loss)? Run that one first. Do not skip to
   the most obvious cause if it requires an expensive test.

3. **Isolate variables** — change exactly one thing per test. If you changed
   two things and the bug went away, you don't know which fixed it.

4. **Evidence, not opinion** — every conclusion must cite a log line, a
   return value, a unit test result, or a stack trace. If the only evidence
   is "it seems like" you have not yet debugged — you have guessed.

5. **Rollback on wrong path** — if a hypothesis fails, undo any changes
   made during the test before trying the next hypothesis. Stale changes
   from wrong guesses cause cascading confusion.

6. **Root cause statement** — before implementing the fix, write one
   sentence: "The bug is that X, which caused Y, which manifested as Z."
   If you cannot write this sentence, you have not found the root cause.
```

## Usage

```bash
# In Claude Code:
/debug loop
# Then describe your bug
```
