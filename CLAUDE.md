# codex

Personal Codex instructions and Claude Code interoperability.

## Commands

No project commands are defined. Claude can be consulted with `claude -p`; keep the call
read-only, as described in `.agents/skills/claude-bridge/SKILL.md`.

## Layout

- `AGENTS.md`: Codex role and review instructions.
- `.agents/skills/claude-bridge/`: Codex skill for read-only Claude consultation.
- `plugins/claude-bridge/`: notes on future Codex plugin packaging.
- `logs/`: Claude CLI exchange log.

## Conventions

- Codex may use the full Git flow only in this repo; see `AGENTS.md` and
  `CODEX_GIT_GUIDE.md`.
