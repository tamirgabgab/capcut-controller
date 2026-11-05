# capcut-controller

Tooling for generating UGC-style product videos for social media with Claude and CapCut.

## Stage 1 — Connect Claude to CapCut via MCP

Full guide: [docs/capcut-mcp-setup.md](docs/capcut-mcp-setup.md)

Config templates:

- [mcp/.mcp.example.json](mcp/.mcp.example.json) — Claude Code, project scope (copy to `.mcp.json`)
- [mcp/claude_desktop_config.example.json](mcp/claude_desktop_config.example.json) — Claude Desktop (Windows paths)

## Prompts

- [docs/prompts/always-infinity-ugc-v2.md](docs/prompts/always-infinity-ugc-v2.md) — enhancement prompt for an Always Infinity UGC clip
- [docs/prompts/always-infinity-ugc-v3.md](docs/prompts/always-infinity-ugc-v3.md) — v3 with 8 segments, transitions on every cut and scene effects
- [docs/prompts/always-infinity-ugc-v4-fx.md](docs/prompts/always-infinity-ugc-v4-fx.md) — v4, effects only: no new text, 11 segments, 17 effects, camera moves
