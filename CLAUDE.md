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

---

## 행동 규칙 — Always / Ask / Never

> 수강생: 윤용식 (성우하이텍 A-RnD, CAE 구조해석 담당)  
> Claude Code 첫 사용자. 채팅형 AI 경험은 있음.

### Always (항상 한다)

- **한국어로 응답한다.** 코드·명령어는 영어 그대로, 설명은 한국어로.
- **파일을 수정하기 전에 반드시 Read로 먼저 읽는다.**
- **판정 기준을 명시한 분석 요청을 우선한다.** 응력/안전계수 기준(500 MPa, 2.0)을 분석 요청에 포함시킨다.
- **CAE 결과 데이터(응력·변형·안전계수)를 다룰 때 판정 기준을 자동 적용한다.**
  - 응력 > 500 MPa → 불합격
  - 안전계수 < 2.0 → 불합격
  - 두 조건 중 하나라도 해당하면 불합격
- **Git 저장 후 "아직 내 컴퓨터에만 있어요" 리마인드를 붙인다.** Push와 Commit의 차이를 체감하게 한다.
- **Windows 네이티브 환경 기준으로 안내한다.** PowerShell/CMD 명령어 사용. WSL·tmux는 이 환경에 없다.

### Ask (먼저 확인한다)

- **판정 기준이 기본값(500 MPa / 2.0)과 다를 때** — 새 기준을 명시적으로 확인한 뒤 적용한다.
- **여러 파일을 한꺼번에 삭제하거나 디렉터리 구조를 재편할 때** — 범위를 보여주고 승인을 받는다.
- **installer 스크립트(`install-lecture-windows.ps1`)를 수정할 때** — Phase 번호와 변경 영향을 설명한 뒤 진행한다.
- **`.claude/` 번들 구조(commands·skills·agents)를 바꿀 때** — 학생 PC 배포에 영향을 주므로 확인 먼저.

### Never (절대 하지 않는다)

- `~/.claude/settings.local.json` 커밋 금지 — 학생 머신 전용 파일.
- `~/.mcp-secrets.env` 또는 실제 API 토큰이 담긴 파일 커밋 금지.
- 실제 Notion API 토큰 파일에 포함 금지 — 플레이스홀더 `YOUR_NOTION_TOKEN` 사용.
- WSL·tmux·Linux 전용 명령어를 이 레포 안내에 사용 금지 — Windows native only.
- 요청 범위를 벗어난 리팩터링·기능 추가 금지 — 요청한 것만 한다.
