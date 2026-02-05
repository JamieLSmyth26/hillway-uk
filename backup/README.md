# Claude Code Configuration Backup

**Backup Date:** 5 February 2026

## Structure

```
backup/
├── CLAUDE.md           # Global instructions loaded every session
├── settings.json       # Model, permissions, and hooks config
├── commands/           # Custom slash commands (20 commands)
├── agents/             # Custom subagent definitions
└── memory/MEMORY.md    # Persistent memory across sessions
```

## Restore Instructions

```bash
cp backup/CLAUDE.md ~/.claude/
cp backup/settings.json ~/.claude/
cp -r backup/commands/* ~/.claude/commands/
cp -r backup/agents/* ~/.claude/agents/
```

## MCP Servers

```bash
claude mcp add github -- npx -y @modelcontextprotocol/server-github
claude mcp add context7 -- npx -y @upstash/context7-mcp
claude mcp add sequential-thinking -- npx -y @modelcontextprotocol/server-sequential-thinking
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem ~/
```
