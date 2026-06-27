# saucedemo-loop

A scoped Loop Engineering experiment: a scheduled Codex automation that hunts FUNCTIONAL bugs in the SauceDemo web app and confirms each with an independent verifier before recording it.

## Hard scope wall - applies to every agent and every run
- Functional bugs ONLY. No security testing, ever: no auth bypass, injection, IDOR / object-ID tampering, cookie or header manipulation, rate-limit probing.
- Anything security-adjacent: STOP, write one line under "Needs human" in FINDINGS.md, do not investigate.
- This rule is restated here deliberately. Do not assume it is inherited from anywhere.

## How the loop works
- `$saucedemo-bug-hunt` explores the app via the playwright MCP and surfaces candidates.
- The `bug-verifier` subagent reproduces each from a clean session. Default-FAIL: a bug is fake until reproduced.
- Every verdict is routed, never dropped: CONFIRMED -> "Confirmed findings", COSMETIC -> "Observations", OUT_OF_SCOPE (security) -> "Needs human"; NOT_REPRODUCED and WORKING_AS_INTENDED are named in the run report. The hunter forgets between runs; the file does not.

## Boundaries
- Agents do not edit application code - this loop discovers and reports, it does not fix.
- No finding becomes a GitHub issue automatically. That is a human decision.
