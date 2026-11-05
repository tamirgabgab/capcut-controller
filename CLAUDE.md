# CLAUDE.md

## Communication

- Always reply to the user in **Hebrew**. Code, code comments, commit messages and
  documentation stay in English.
- RTL-safe Hebrew: every paragraph, heading, table cell and list item starts with a
  Hebrew word. If a line must begin with a Latin character or a number, put the RLM
  mark (U+200F) first. Numbered lists are written as "‏1. **מילה** ...".
- The user works on **Windows (cmd)**: give Windows commands (`copy`, `py`,
  `venv-capcut\Scripts\activate`), not bash-only ones like `source` or `cp`.
- One command per code block, so each can be copied on its own.

## Project

- Goal: generate UGC-style product videos for social media with Claude + CapCut.
- Stage 1: connect Claude to CapCut via the community VectCutAPI MCP server.
  Guide: `docs/capcut-mcp-setup.md`; config templates in `mcp/`.
- The user's VectCutAPI clone lives at `C:\Users\User\VectCutAPI`.
