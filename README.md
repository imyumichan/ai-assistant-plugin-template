# Template for an Agentic AI Assistant Plugin

An agentic AI assistant plugin for GitHub Copilot CLI, and more, that bundles
curated agents, instructions, skills, hooks, and MCP server integrations into
a single installable package.

## Quick Start

This plugin supports two modes:

1. **VS Code workspace mode** for fast iteration during development
2. **Copilot CLI plugin mode** for installing the packaged plugin

### Mode 1: VS Code Workspace Mode

1. Clone this repository.
2. Open the repository root as the VS Code workspace.
3. Edit agents, prompts, skills, instructions, and MCP definitions directly.

VS Code discovers agents, skills, instructions, prompts, and hooks through the
symlinks in `.github/`, and MCP servers through `.vscode/mcp.json`. Edits take
effect without reinstalling the plugin.

### Mode 2: Copilot CLI Plugin Mode

Install the plugin from the repository root. `plugin.json` declares the agents, skills, prompts (as commands), hooks, and MCP servers (`.mcp.json`) that get packaged:

```bash
# Install from a local path or a public repository
copilot plugin install ./

# Verify installation
copilot plugin list
```

An installed plugin is a snapshot: reinstall after making changes to pick them up. Use this mode to test the packaged plugin as users will receive it.

## Plugin Components

| Component | Path | Purpose |
|-----------|------|---------|
| Agents | `agents/` | Specialized AI assistants for specific workflows |
| Instructions | `instructions/` | Shared guidance and project-specific constraints |
| Prompts | `prompts/` | Reusable prompt templates for consistent AI interactions |
| Skills | `skills/` | Discrete callable capabilities with scoped instructions |
| Hooks | `hooks/hooks.json` | Event handlers triggered during agent sessions |
| MCP Servers | `.mcp.json` | Model Context Protocol integrations |

## MCP configuration note

The Copilot CLI plugin reads MCP servers from `.mcp.json` under a top-level `mcpServers` key. VS Code uses a different format, with a top-level `servers` key in `.vscode/mcp.json`.

> [!NOTE]
> When you commit a change to `.mcp.json` in this repository, the Git hook installed at `.githooks/pre-commit` runs automatically and stages the generated `.vscode/mcp.json` in the same commit, so the editor config stays synchronized without any extra manual step.

### Manual update

After editing `.mcp.json`, regenerate the VS Code file with:

```bash
uv run scripts/sync-mcp-server-configs.py
```

This keeps the workspace editor config in sync with the plugin config without manually duplicating the values.

## Development

1. Clone this repository.
2. Update agents, skills, hooks, prompts, or MCP definitions.
3. Reinstall locally to pick up changes:
   ```bash
   copilot plugin install ./
   ```
4. Validate behavior in an interactive session:
   ```bash
   copilot
   /agent         # list available agents
   /skills list   # list available skills
   ```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## Structure

```text
.
├── plugin.json            # Plugin manifest (required)
├── .github/               # GitHub configs and workspace discovery entrypoints
├── .vscode/               # VS Code-specific MCP config generated from .mcp.json
├── agents/                # Custom agent definitions (*.agent.md)
├── docs/                  # Documentation and guides
├── instructions/          # Shared instruction files (*.instructions.md)
├── prompts/               # Reusable prompt templates (*.prompt.md)
├── scripts/               # Helper scripts for hooks and tooling
├── skills/                # Skill definitions (*/SKILL.md)
├── .mcp.json              # MCP server configuration for Copilot CLI
├── hooks/hooks.json       # Hook configuration
└── README.md              # Plugin overview and setup guide
```
