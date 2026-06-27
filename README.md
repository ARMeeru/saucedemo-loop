# saucedemo-loop

First loop: scheduled functional E2E bug-hunting on SauceDemo, with maker-checker verification.

## Run order
1. (optional) `git init` in this folder - enables worktree mode and versions FINDINGS.md over time.
2. Open this folder as your Codex project (launch the CLI from here, or add it in the app).
3. Confirm the playwright MCP block in `.codex/config.toml` against https://developers.openai.com/codex/mcp
4. In `.codex/agents/bug-verifier.toml`, set `model` to something DIFFERENT from the model the automation/hunter uses (see `/models`).
5. SMOKE TEST FIRST: in a normal thread, run `$saucedemo-bug-hunt`. Watch the bug-verifier open a clean session.
6. CALIBRATE: plant a known non-bug (verifier must reject) and a known real bug (verifier must confirm). Tune `.codex/agents/bug-verifier.toml` until both land.
7. Only then create the automation: Automations pane -> standalone, custom cron `0 3 * * *`, dedicated worktree, fast model, workspace-write, prompt below.

## Automation prompt
Run $saucedemo-bug-hunt against https://www.saucedemo.com. Hand every candidate anomaly to the bug-verifier subagent and append only CONFIRMED functional findings to FINDINGS.md. Functional only - no security testing; route anything ambiguous to a "Needs human" line. Report nothing if there are no new confirmed findings.

## Scope
Functional bugs only. See AGENTS.md for the hard wall.
