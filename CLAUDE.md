# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A **Windows-native installer bundle** for the Claude Code lecture environment used at 성우하이텍 (Sungwoo Hitech). It packages commands, skills, agents, and references that get copied into `~/.claude/` on a student's PC.

The main installer is `install-lecture-windows.ps1`. It runs in 5 phases:
1. Install prerequisites via winget (Git, Python 3.12, Node.js LTS, jq, GitHub CLI)
2. Install Claude Code via npm (`@anthropic-ai/claude-code`)
3. Authenticate Claude (`claude auth login`)
4. Copy bundle assets to `~/.claude/` and configure MCP servers
5. Verify installation (checks versions + asserts key asset paths exist)

## Running the Installer

```powershell
powershell -ExecutionPolicy Bypass -File .\install-lecture-windows.ps1
```

The script must be run from the repo root (it uses `$PSScriptRoot` as `$BundleDir`).

## Bundle Asset Structure

Assets under `.claude/` are copied verbatim into `~/.claude/` on the student's machine:

| Source | Destination | Purpose |
|--------|-------------|---------|
| `.claude/commands/*.md` | `~/.claude/commands/` | Slash commands (`/lecture`, `/start-mw4-1`, etc.) |
| `.claude/skills/` | `~/.claude/skills/` | Skill definitions invoked by commands |
| `.claude/agents/*.md` | `~/.claude/agents/` | Subagents (`dept-mentor`, `quality-reviewer`) |
| `.claude/reference/*.md` | `~/.claude/reference/` | Reference docs for Claude |

## Key Installer Behaviors

- **Idempotent package installs**: `Install-WingetPackage` skips if already installed
- **MCP servers registered**: playwright, notion, pdf-tools, notebooklm (via `claude mcp add-json`)
- **settings.local.json**: Always overwritten with `{"enableAllProjectMcpServers": true}`
- **settings.json**: Created as `{}` only if it doesn't already exist
- **Pinned Claude Code version**: Set via `$ClaudeCodeVersion` at the top of the script

## Verification Checks (Phase 5)

The installer asserts these assets exist after copying — if any are missing, the install is broken:
- `~/.claude/commands/lecture.md`
- `~/.claude/commands/start-mw4-1.md`
- `~/.claude/skills/lesson-a/SKILL.md`
- `~/.claude/agents/dept-mentor.md`
- `~/.claude/settings.local.json`

## Lesson Commands

After install, students use these slash commands in Claude Code:

- `/start-mw4-1` through `/start-mw4-4` — MW4 교시별 강의 시작
- `/start-mw5-1` through `/start-mw5-4` — MW5 교시별 강의 시작
- `/lecture` — Full lecture orchestration
- `/lesson` — Per-session lesson start

## Platform Scope

This installer is **Windows native only** (no WSL, no tmux). The `MIGRATION-GUIDE.md` documents other platforms (Linux/WSL/macOS) but those use separate scripts not in this repo.

## 도메인 판정 규칙

강의 실습에서 구조 해석 결과를 평가할 때 적용하는 기준:

- 응력 > 500 MPa → **불합격**
- 안전계수 < 2.0 → **불합격**

두 조건 중 하나라도 해당하면 불합격으로 표시한다.

## Sensitive Files — Do Not Commit

- `~/.claude/settings.local.json` (student machine only)
- `~/.mcp-secrets.env` (API keys)
- Any file containing actual Notion API tokens (use `YOUR_NOTION_TOKEN` as placeholder)
