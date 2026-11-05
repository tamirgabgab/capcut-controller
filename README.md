# capcut-controller

Tooling for generating UGC-style product videos for social media with Claude and CapCut.

## Stage 1 — Connect Claude to CapCut via MCP

Full guide: [docs/capcut-mcp-setup.md](docs/capcut-mcp-setup.md)

Config templates:

- [mcp/.mcp.example.json](mcp/.mcp.example.json) — Claude Code, project scope (copy to `.mcp.json`)
- [mcp/claude_desktop_config.example.json](mcp/claude_desktop_config.example.json) — Claude Desktop (Windows paths)

## Prompts

- [docs/prompts/always-infinity-ugc-v2.md](docs/prompts/always-infinity-ugc-v2.md) — enhancement prompt for an Always Infinity UGC clip
