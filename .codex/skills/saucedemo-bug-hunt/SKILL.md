---
name: saucedemo-bug-hunt
description: Drive the SauceDemo web app through real user flows, surface FUNCTIONAL anomalies only, and confirm each one with the bug-verifier subagent before recording it. Use for scheduled functional E2E bug-hunting on SauceDemo. Never performs security testing.
---

# SauceDemo Functional Bug Hunt

## Scope - read this first
- IN SCOPE: functional defects only - broken flows, wrong cart/checkout totals, bad form validation, broken navigation, state/UI bugs, data that doesn't match expected behavior.
- OUT OF SCOPE - HARD STOP: no security testing of any kind. Never attempt auth bypass, injection, IDOR / object-ID tampering, cookie or header manipulation, or rate-limit probing. If a flow looks like it MIGHT expose a security issue, stop, record it under "Needs human" (see routing), and move on. Do not explore it autonomously.
- You are a user, not an attacker. Target the app's intended behavior only.

## Tools
- Use the `playwright` MCP to open the app, log in, click, type, and read the rendered DOM.

## Workflow

### 1) Explore the standard flows
Walk these as a real user, noting actual vs. expected: login with each documented credential set; inventory sort orders, add/remove items, badge counts; cart quantities, totals, remove behavior; checkout form validation, item totals, tax/total math, confirmation; logout and session reset.

### 2) Collect candidates - do not trust them
For each anomaly write: page/URL, exact steps, expected, actual. Do NOT record it as a finding yet.

### 3) Hand every candidate to the verifier
Spawn the `bug-verifier` subagent with the steps. It returns exactly one verdict per candidate: CONFIRMED, NOT_REPRODUCED, WORKING_AS_INTENDED, COSMETIC, or OUT_OF_SCOPE.

### 4) Route every verdict - nothing is silently dropped
- CONFIRMED            -> append to FINDINGS.md under "## Confirmed findings".
- COSMETIC             -> append a one-line entry to FINDINGS.md under "## Observations".
- OUT_OF_SCOPE         -> (security only) append a one-line entry to FINDINGS.md under "## Needs human". Do not investigate it.
- NOT_REPRODUCED       -> do NOT write to FINDINGS.md, but name it in the run report (step 5).
- WORKING_AS_INTENDED  -> do NOT write to FINDINGS.md, but name it in the run report (step 5).
The rejected candidates (NOT_REPRODUCED, WORKING_AS_INTENDED) are the evidence the loop is checking itself. They must stay visible in the report, never vanish.

### 5) Report
End with a summary that names, by candidate: every CONFIRMED, COSMETIC, and OUT_OF_SCOPE, and - individually - every NOT_REPRODUCED and WORKING_AS_INTENDED. A run that drops a verdict without naming it has failed its own audit.

## FINDINGS.md sections and formats
- "## Confirmed findings": for each, a title line "[CONFIRMED] <short title>", then Flow, Steps (numbered, reproducible), Expected, Actual, Evidence, First seen (date).
- "## Observations": one line each - "[COSMETIC] <short title> - <where> - <what was seen> (<date>)".
- "## Needs human": one line each - "[OUT_OF_SCOPE / SECURITY] <short title> - <where> - not investigated (<date>)".
