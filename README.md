# codex

Personal Codex workflow and Claude Code interoperability notes.

Claude Code is installed locally as `claude`. This repo documents the Codex-side workflow
for asking Claude for a read-only second opinion. The sibling `../claude` repo documents
the reverse direction: Claude calls Codex through `codex exec` and `codex review`.

## Use the bridge

From a project directory, ask Codex to use the `claude-bridge` skill. It invokes Claude
through `claude -p` with read-only tools and returns the reply for Codex to assess. Verify
Claude's findings against the project before acting.

## MCP and plugins

No custom MCP server is required for the bridge. The Claude CLI is the communication
endpoint, and invoking it through the shell keeps the exchange visible. Exposing Claude's
general tools to Codex through MCP would grant broader capabilities than needed.

The skill works directly from `.agents/skills`. `plugins/claude-bridge` records the
packaging location for a future Codex plugin; no plugin is installed by this repo yet.
