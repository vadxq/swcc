# SWCC Codex Install

This repository keeps the editable Claude-side source in `CLAUDE.md` and `.claude-plugin/`, then regenerates the managed Codex files under `.codex/`.

## Managed Codex Files

- `.codex/skills/*` — the 5 user-invocable Codex skills
- `.codex/skills/swcc/agents/*` — the 10 shared SWCC role prompts
- `.codex/skills/swcc/scripts/*` — install and uninstall helpers
- `.codex/scripts/sync-from-claude.py` — the generator
- `.codex/generated-manifest.json` — the managed file manifest

## Sync From Claude

Run from the repository root:

```bash
bash .codex/sync.sh
```

Helpful modes:

```bash
bash .codex/sync.sh --dry-run
bash .codex/sync.sh --check
```

What it does:

- reads `CLAUDE.md` and `.claude-plugin/`
- regenerates all managed files under `.codex/`
- keeps skill and agent content close to the Claude source, with only Codex-specific path and invocation rewrites
- does **not** modify any file outside `.codex/`

## Install Into Codex

After syncing, install the repo-local skill root:

```bash
bash .codex/skills/swcc/scripts/install.sh
```

What it does:

- creates `./.agents/` if needed
- links `./.agents/skills` to `.codex/skills/`
- keeps the editable Claude source in `CLAUDE.md` and `.claude-plugin/`

After installing, restart Codex or reopen the repository.

## Verify

1. Restart Codex or reopen the project.
2. Open this repository in Codex.
3. Invoke one of the skills explicitly, for example:

```text
$zhili 给这个项目加 JWT 认证
$xieshang 只给我这个功能的实施方案
$jicha 重点关注安全问题和测试覆盖
```

Expected skill names:

- `jicha`
- `juguo`
- `xieshang`
- `zhili`
- `zhixing`

## Uninstall

Remove repo-local symlinks:

```bash
bash .codex/skills/swcc/scripts/uninstall.sh
```

## Notes

- Source of truth: `CLAUDE.md` and `.claude-plugin/`
- Managed Codex outputs live only under `.codex/`
- The runtime artifacts still live in `.tmp/swcc/`, just like the Claude version.
- The Codex adaptation preserves the same five workflow names and all ten shared political-role prompts.
- The orchestration backend is adapted to Codex sub-agents, so the coordinator skill dispatches `agent_prompt`-based role work instead of Claude's `swcc:agent-name` namespace.
- Project title source: `SWCC — Socialism With Chinese Characteristics (SW Claude Code)`
