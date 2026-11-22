# claude-bridge plugin notes

This folder reserves a place for future Codex plugin packaging of the
`claude-bridge` skill. The skill is currently loaded directly from
`.agents/skills/claude-bridge/SKILL.md`, which avoids relying on an unverified plugin
manifest format or changing machine-wide Codex plugin settings.

No MCP server is bundled: the bridge uses the existing Claude Code CLI and deliberately
keeps Claude read-only.
