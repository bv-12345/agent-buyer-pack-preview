# Hermes Audit — Pre-flight Check (Mode 1/15)

Run this before any agent deployment. Paste the following into Claude, Sonnet, GPT-4o, or any frontier model with access to your deployment plan.

## Prompt

```
You are a paranoid SRE auditing this agent deployment before it goes live.
Read the deployment plan and config files below.

Check each of the following and flag anything that would cause a failure,
data leak, or cost overrun within the first 24 hours of operation:

1. API keys — are all required keys set and none hardcoded in config files?
2. Rate limits — does the dispatch config respect provider TPM/RPM caps?
3. Cost caps — is there a daily spend limit and will routing logic respect it?
4. Error handling — what happens when every provider returns 429 or 500 simultaneously?
5. Stale lock detection — if a task crashes, will the lock clear within 5 minutes?
6. Logging — is every LLM call logged with cost, model, duration?
7. Secret exposure — does any config file contain a key visible to the model input?
8. Tail recursion — could the agent re-prompt itself into an infinite loop?
9. Provider diversity — if the primary provider goes down, does it fail open or closed?
10. Recovery path — after failure, what's the restart condition?
11. Environment parity — are dev/staging/prod configs the same thing with different keys?
12. Regression catch — is there an automated check that the previous 3 bugs haven't reappeared?

For each issue found, output:
- Risk level (HIGH/MED/LOW)
- One-sentence diagnosis
- One-sentence fix instruction
```

## Example output

```
HIGH — API key visible in routing-rules.json line 14.
Diagnosis: `OPENAI_KEY` stored as plaintext string in JSON config.
Fix: move to .env and reference via ${OPENAI_KEY} in routing rules.

MED — No daily cost cap on Claude Opus.
Diagnosis: Opus at £15/M input can hit £50/day on a single stuck loop.
Fix: add `maxDailySpend: 10` in provider dispatch config.

LOW — 429 handling falls through to crash.
Diagnosis: retry policy retries 3x then throws uncaught.
Fix: add fallback to secondary provider after 2 retries.
```

---

*This is 1 of 15 audit modes in the full Hermes Audit Suite v1 (£9, launches 2026-06-04). Full suite includes: SRE Audit, Security Scan, Cost Optimizer, Prompt Injection Check, Regression Hunter, Drift Detector, Persona Rotator, Compliance Check, Performance Benchmark, Dependency Scanner, State Integrity Check, Memory Leak Detector, and 3 bonus modes.*
