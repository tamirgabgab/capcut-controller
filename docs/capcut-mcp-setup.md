# Connecting Claude to CapCut via MCP

Stage 1 of `capcut-controller`: connect Claude Code (or Claude Desktop) to CapCut
through an MCP server, so Claude can assemble UGC-style product videos as CapCut
drafts that you then polish and export.

> Researched November 2026. CapCut has **no official public API and no official MCP
> server**. Every option below is community-built and works by writing CapCut
> *draft folders* on disk. Expect breakage when CapCut updates its draft format.

---

## 1. How it works

```
Claude Code / Claude Desktop  (your computer)
        │  MCP over stdio
        ▼
VectCutAPI  mcp_server.py      ← Python, runs locally
        │  writes files
        ▼
dfd_xxx/ draft folder  ──copy──►  CapCut drafts directory
                                         │
                                         ▼
                              CapCut Desktop → open → tweak → export MP4
```

Key consequences:

- The MCP server **must run on the same computer as CapCut Desktop**. A cloud
  session (claude.ai/code) cannot reach your local CapCut.
- Claude does not "click" inside CapCut. It builds a project file; CapCut renders it.
- Media (videos, images, audio) is passed **by URL or local path**.

## 2. Choosing a server

| Option | What it is | Verdict |
|---|---|---|
| [sun-guannan/VectCutAPI](https://github.com/sun-guannan/VectCutAPI) (formerly CapCutAPI) | Python backend: HTTP API on port 9001 + stdio MCP server, 11 tools. Apache-2.0, actively maintained (commits in Oct 2026). | **Recommended** — the backend nearly every other wrapper depends on. |
| [Atx-Guy/capcut-mcp-server](https://github.com/Atx-Guy/capcut-mcp-server) | TypeScript MCP wrapper over the VectCutAPI HTTP server. | Optional extra layer; needs VectCutAPI running anyway. |
| [MigueDuque/capcut-mcp-server-extended](https://github.com/MigueDuque/capcut-mcp-server-extended) | Fork of the above with typography/animation presets for Claude Code. | Worth a look later for UGC presets. |
| [jmsldrn/capcut-mcp](https://github.com/jmsldrn/capcut-mcp) | Node server that edits **existing** CapCut desktop drafts (move/trim/split clips). | Useful for editing a template you designed by hand. |

This guide uses **VectCutAPI's built-in MCP server** — fewest moving parts.

## 3. Prerequisites

- CapCut Desktop (international version) on **Windows or macOS**
- Python **3.10+**
- FFmpeg on `PATH`
  - macOS: `brew install ffmpeg`
  - Windows: `winget install Gyan.FFmpeg`
- Git
- Claude Code CLI (`npm install -g @anthropic-ai/claude-code`) or Claude Desktop

## 4. Install VectCutAPI

### macOS / Linux (Terminal)

```bash
git clone https://github.com/sun-guannan/VectCutAPI.git
cd VectCutAPI
python3 -m venv venv-capcut
source venv-capcut/bin/activate
pip install -r requirements.txt
pip install -r requirements-mcp.txt
cp config.json.example config.json
```

### Windows (Command Prompt / cmd)

`source` and `cp` do not exist in cmd. On Windows, `python3` is often a Microsoft
Store alias that silently does nothing, so use the `py` launcher:

```bat
git clone https://github.com/sun-guannan/VectCutAPI.git
cd VectCutAPI
py -0
py -3 -m venv venv-capcut
venv-capcut\Scripts\activate
python --version
pip install -r requirements.txt
pip install -r requirements-mcp.txt
copy config.json.example config.json
```

- `py -0` lists installed Pythons; you need 3.10 or newer. If `py` is missing,
  install Python from python.org and tick **"Add python.exe to PATH"**.
- After `activate`, the prompt starts with `(venv-capcut)`.
- PowerShell instead of cmd: activate with `venv-capcut\Scripts\Activate.ps1`. If
  scripts are blocked, run `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` once.

### `config.json`

The example file contains `//` comments; keep it as is or strip them. The fields
that matter for local use:

```jsonc
{
  "draft_profile": "capcut_legacy",   // CapCut international. Other values: jianying_legacy, jianying_pro_10
  "port": 9001,                       // local HTTP server port
  "is_upload_draft": false            // keep false: no cloud (OSS) upload
}
```

Leave the `oss_config` / `mp4_oss_config` blocks untouched; they are only used when
`is_upload_draft` is `true`.

## 5. Smoke-test locally

```bash
python test_mcp_client.py
```

Expected: the client connects and reports **11 available tools**:
`create_draft`, `add_video`, `add_audio`, `add_image`, `add_text`, `add_subtitle`,
`add_effect`, `add_sticker`, `add_video_keyframe`, `get_video_duration`, `save_draft`.

## 6. Register the MCP server with Claude

Always use the **venv's Python** (absolute path), so the MCP dependencies are found.

### Claude Code (recommended)

macOS / Linux:

```bash
claude mcp add --scope user capcut \
  --env PYTHONPATH=/ABS/PATH/VectCutAPI \
  -- /ABS/PATH/VectCutAPI/venv-capcut/bin/python /ABS/PATH/VectCutAPI/mcp_server.py
```

Windows (cmd or PowerShell, one line; example clone at `C:\Users\User\VectCutAPI`):

```bat
claude mcp add --scope user capcut --env PYTHONPATH=C:\Users\User\VectCutAPI -- C:\Users\User\VectCutAPI\venv-capcut\Scripts\python.exe C:\Users\User\VectCutAPI\mcp_server.py
```

Verify:

```bash
claude mcp list
```

Inside a session, `/mcp` shows `capcut` as connected with 11 tools.

### Claude Desktop

Edit `claude_desktop_config.json`
(macOS: `~/Library/Application Support/Claude/`, Windows: `%APPDATA%\Claude\`).
See [`mcp/claude_desktop_config.example.json`](../mcp/claude_desktop_config.example.json)
in this repo, then fully quit and reopen Claude Desktop.

### Project-scoped (this repo)

To share the setup with anyone who clones this repo, copy
[`mcp/.mcp.example.json`](../mcp/.mcp.example.json) to `.mcp.json` at the repo root
and fix the paths. `.mcp.json` is git-ignored because paths are machine-specific.

## 7. Getting the draft into CapCut

`save_draft` writes a `dfd_<id>` folder and, with `auto_deploy` (default `true`),
copies it into CapCut's drafts directory **if it finds it**:

| OS | CapCut drafts directory | Auto-deploy? |
|---|---|---|
| Windows | `%LOCALAPPDATA%\CapCut\User Data\Projects\com.lveditor.draft` | Yes |
| macOS | `~/Movies/CapCut/User Data/Projects/com.lveditor.draft` | **No** — the code only checks the JianyingPro container path |

On macOS, either copy the `dfd_*` folder manually, or ask Claude to call
`save_draft` with `draft_folder` set to the macOS path above.
Confirm the exact path in CapCut: *Settings → Draft location*.

Then: **close CapCut before saving** (its autosave can overwrite the new draft),
reopen it, and the project appears in the drafts list.

On Windows, `desktop_companion.py` in VectCutAPI can auto-reload CapCut after a deploy.

## 8. First test prompt

```
Use the capcut MCP tools to:
1. create_draft at 1080x1920 (9:16).
2. add_video from <URL of a product clip>.
3. add_text "This changed my morning routine" at the top, 0–3s, bold white with shadow.
4. add_text "Link in bio" at the bottom, last 3 seconds.
5. save_draft with project_name "ugc-test-01".
Report the draft_id and where the draft was saved.
```

Success = the project opens in CapCut with the clip and both captions.

## 9. Troubleshooting

| Symptom | Fix |
|---|---|
| `capcut` missing in `/mcp` | Run `claude mcp list`; use absolute paths; use the venv Python, not system `python3`. |
| `ModuleNotFoundError: mcp` | `pip install -r requirements-mcp.txt` inside the venv. |
| Draft not visible in CapCut | Check the drafts path (section 7); close and reopen CapCut. |
| Draft opens empty / errors | CapCut version changed the draft format. Check [VectCutAPI issues](https://github.com/sun-guannan/VectCutAPI/issues) for your version; keep CapCut auto-update off once a version works. |
| Media fails to load | URL must be directly downloadable (not a share page). FFmpeg must be on `PATH`. |
| Verbose logs | Add `DEBUG=1` to the server env. |

## 10. Limitations to plan around

- Unofficial: CapCut updates can break draft compatibility at any time.
- No headless rendering — final export is done in CapCut Desktop.
- `draft_domain` in `config.json` points at a third-party host used for preview
  links; nothing is uploaded while `is_upload_draft` is `false`.
- CapCut's terms of service apply to automated use; review them for commercial work.

## Next stages (proposed)

1. UGC templates: hook → problem → product demo → CTA, as reusable prompt/skill.
2. Asset pipeline: product images/clips, AI voiceover, auto-subtitles (SRT → `add_subtitle`).
3. Batch generation: one script, N products → N drafts.
