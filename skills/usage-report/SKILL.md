---
name: "usage-report"
description: "Report local agent CLI usage with ccusage on Linux/ARM64."
metadata:
  {
    "openclaw":
      {
        "emoji": "📊",
        "requires": { "bins": ["npx"] }
      }
  }
---

# Usage Report

Use `ccusage@20.0.6` through `npx` to report local coding-agent CLI token usage and costs. This is the Linux/ARM64 replacement for the macOS/CodexBar-only `model-usage` skill.

## Safety

- This reads local usage/session logs; do not upload logs or credentials.
- Never print tokens, API keys, auth files, or credential values.
- Use the pinned package version: `ccusage@20.0.6`.
- Use Lucas's timezone by default: `America/Sao_Paulo`.
- Prefer JSON output when parsing or summarizing; use table output only when the user wants raw CLI output.
- Future package/version changes are ask-first.

## Supported Sources

Use these commands when the user names a specific source:

- General/all detected: `npx -y ccusage@20.0.6 <period> --timezone America/Sao_Paulo`
- OpenClaw: `npx -y ccusage@20.0.6 openclaw <period> --timezone America/Sao_Paulo`
- Codex: `npx -y ccusage@20.0.6 codex <period> --timezone America/Sao_Paulo`
- Claude Code: `npx -y ccusage@20.0.6 claude <period> --timezone America/Sao_Paulo`

## Period Mapping

Always compute dates in `America/Sao_Paulo` unless the user asks otherwise.

General/all detected supports:

- Today/default: `daily --since YYYY-MM-DD --until YYYY-MM-DD`
- Last 7 days: `daily --since YYYY-MM-DD --until YYYY-MM-DD`
- This week/weekly: `weekly`
- This month/monthly: `monthly`
- Sessions: `session`

OpenClaw and Codex support:

- Today/default: `daily --since YYYY-MM-DD --until YYYY-MM-DD`
- Last 7 days: `daily --since YYYY-MM-DD --until YYYY-MM-DD`
- This month/monthly: `monthly`
- Sessions: `session`

Claude Code supports:

- Today/default: `daily --since YYYY-MM-DD --until YYYY-MM-DD`
- Last 7 days: `daily --since YYYY-MM-DD --until YYYY-MM-DD`
- This week/weekly: `weekly`
- This month/monthly: `monthly`
- Sessions: `session`
- Billing windows: `blocks`

If the user asks for a weekly OpenClaw or Codex report, use `daily` with a 7-day date range and summarize it as a week.

## Useful Flags

- `--json`: use for machine parsing and concise summaries.
- `--breakdown`: use when the user asks for per-model detail and the selected command supports it.
- `--offline`: use when avoiding live price refresh is preferable or if network calls are not needed.
- `--no-color`: use for plain text output.

## Workflow

1. Determine source: all, OpenClaw, Codex, or Claude.
2. Determine period and date bounds in `America/Sao_Paulo`.
3. Run the pinned `npx -y ccusage@20.0.6 ...` command.
4. If the command fails because no local usage data exists, say that directly and suggest a broader source/period.
5. Summarize compactly: period, source, models when available, total tokens, cost, and any caveat.
6. For financial decisions, mention that costs are estimates based on local logs and ccusage pricing logic.

## Examples

Today for OpenClaw:

```bash
npx -y ccusage@20.0.6 openclaw daily --timezone America/Sao_Paulo --since 2026-06-08 --until 2026-06-08 --json
```

Weekly all detected sources:

```bash
npx -y ccusage@20.0.6 weekly --timezone America/Sao_Paulo --json
```

Seven-day Codex summary:

```bash
npx -y ccusage@20.0.6 codex daily --timezone America/Sao_Paulo --since 2026-06-02 --until 2026-06-08 --json
```

Codex sessions:

```bash
npx -y ccusage@20.0.6 codex session --timezone America/Sao_Paulo --json
```

Claude monthly with model detail:

```bash
npx -y ccusage@20.0.6 claude monthly --timezone America/Sao_Paulo --breakdown --json
```
