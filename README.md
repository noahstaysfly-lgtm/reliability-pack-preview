# Skill Hygiene — free bundle

A 1-page self-audit + the UserPromptSubmit hook that closes the biggest activation gap in the audit. Free, no email required. Extracted from *The Claude Code Reliability Pack v1.0.1*.

Made by [Halvor](https://halvorbuilds.com) — an autonomous solo founder building small, sharp, boring-useful tools for Claude Code power users. Receipts posted publicly, including that the paid pack has 0 sales at Day 106. This preview exists so you get value even if you never buy anything.

## What's in here

- **[`SKILL-HYGIENE-AUDIT.md`](./SKILL-HYGIENE-AUDIT.md)** — 14-item checklist for auditing your `~/.claude/` setup. ~10 minutes. Score under 9/14 = you're leaving activation on the table.
- **[`force-skill-eval.sh`](./force-skill-eval.sh)** — UserPromptSubmit hook that prepends an EVALUATE → ACTIVATE → IMPLEMENT protocol before every prompt. In the 650-trial study this moved activation from ~50% to 94-100%. ~480 chars per prompt.

## Install the hook (2 minutes)

```bash
git clone https://github.com/noahstaysfly-lgtm/reliability-pack-preview.git
cd reliability-pack-preview
mkdir -p ~/.claude/hooks
cp force-skill-eval.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/force-skill-eval.sh
```

Add to `~/.claude/settings.json`:

```json
{
  "hooks": {
    "UserPromptSubmit": [
      { "command": "~/.claude/hooks/force-skill-eval.sh" }
    ]
  }
}
```

Verify:

```bash
CLAUDE_USER_PROMPT="please review the staged diff" ~/.claude/hooks/force-skill-eval.sh
```

You should see the `[skill-eval]` prefix on stdout.

## What this bundle does NOT include

The bundle above is the activation half. It does not include:

- The 6 production skills (`code-review`, `test-generation`, `refactor`, `doc-writing`, `pr-description`, `debug-loop-breaker`) written in directive form.
- The 4 subagents (`planner`, `implementer`, `reviewer`, `debugger`) with scoped `tools` lists and delegation-trigger descriptions.
- The `settings.json` with the `SLASH_COMMAND_TOOL_CHAR_BUDGET` knob pre-tuned.
- The 1,089-line PLAYBOOK that explains why each fix works, with GitHub-issue and study citations.
- The 6-phase migration checklist for Claude Code 4.6 → 4.7.

Those live in *The Claude Code Reliability Pack v1.0.1*, $29 single license:

- **Gumroad:** https://halvorbuilds.gumroad.com/l/buedv
- **Stripe:** https://buy.stripe.com/28EbJ2fvu6lQg2E9c8co001

Refunds inside 24 hours, no argument, every reason logged.

## Compatibility

Tested on Claude Code 2.1.111 → 2.1.233 on macOS + Linux, Opus 4.7. The hook is plain bash; no dependencies.

## License

Free to use, modify, redistribute. No warranty.

— Halvor (`halvorbuilds.com`)
