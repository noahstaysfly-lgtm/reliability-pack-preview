# Claude Code Skill Hygiene Self-Audit — v1

**One page. Five sections. ~10 minutes.** Score each item 1 (yes) or 0 (no).
Anything under 9/14 = your skills are leaving activation on the table.
Tested on Claude Code 2.1.111 → 2.1.138, Opus 4.7. Source frame: PLAYBOOK §1.

---

## A. Skill descriptions (the routing table)

For every file in `~/.claude/skills/`, open it and check the `description:` field:

- [ ] Starts with an imperative verb (`REVIEW`, `DRAFT`, `DEBUG`, `REFACTOR`).
- [ ] Contains at least 3 trigger words a user would actually type.
- [ ] States the output contract or a constraint (not just a topic).
- [ ] Is under 200 characters. Long descriptions dilute the routing signal.
- [ ] Has been touched in the last 60 days (rotted skills compete for budget).

## B. Activation forcing function

- [ ] `~/.claude/hooks/force-skill-eval.sh` (or equivalent UserPromptSubmit hook) is installed AND registered in `settings.json` under `hooks.UserPromptSubmit`.
- [ ] You can run `CLAUDE_USER_PROMPT="test prompt" ~/.claude/hooks/force-skill-eval.sh` and see the `[skill-eval]` prefix on stdout.

## C. Token economics

- [ ] `SLASH_COMMAND_TOOL_CHAR_BUDGET` is set in `~/.claude/settings.json` (start with `"30000"` for an 8-12 skill setup; tune from there).
- [ ] No skill description is over 200 chars (long descriptions ship on every prompt whether the skill fires or not).
- [ ] Your `CLAUDE.md` is under 200 lines (every line ships on every turn).

## D. Subagent delegation

- [ ] Every subagent in `~/.claude/agents/` has a `tools` list scoped to its job — NOT `["*"]`. Broad tool lists tell the main agent the subagent is generic, and it stops delegating.
- [ ] Each subagent description names the *delegation trigger* explicitly ("USE WHEN the user asks to plan...", not "for planning").
- [ ] No two subagents have overlapping trigger phrases.

## E. Measurement (the only one that compounds)

- [ ] OTEL is wired. `claude_code.skill_activated` events with the `invocation_trigger` attribute land somewhere you can query (added in Claude Code v2.1.126). Without this you are guessing.
- [ ] You have a 7-day-rolling activation rate per skill. A skill below 60% activation on its own trigger phrases is a rewrite candidate; below 30% is a delete candidate.

---

**Score: ___ / 14.** 12-14 = your setup is healthy. 9-11 = rewrite the worst-scoring skill description first. Under 9 = install the hook and the char-budget knob today; everything else flows from those two.

The full why-it-works walkthrough (650-trial study, citations, regression history through 2.1.138) is in *The Claude Code Reliability Pack* — see footer.
