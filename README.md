# Claude Code User Guide

> 📖 Official Documentation: https://code.claude.com/docs/en/
> 📋 Written for Claude Code v2.x, last updated: 2026/06/17
> Features described may differ in the latest version. Always refer to the [official documentation](https://code.claude.com/docs/en/) as the source of truth.

A comprehensive, battle-tested guide to Claude Code — the agentic coding assistant that reads your code, writes files, and runs terminal commands. This guide is organized by feature module and integrates real-world experience, aimed at developers who already have the basics down.

---

## Table of Contents

1. [Understanding Claude Code](#1-understanding-claude-code)
2. [Memory System](#2-memory-system)
3. [Permission Modes](#3-permission-modes)
4. [Skills System](#4-skills-system)
5. [Subagents](#5-subagents)
6. [Hooks](#6-hooks)
7. [MCP Servers](#7-mcp-servers)
8. [Plugin System](#8-plugin-system)
9. [Model Configuration & Effort](#9-model-configuration--effort)
10. [Common Workflows](#10-common-workflows)
11. [Dynamic Workflows](#11-dynamic-workflows)
12. [Code Review](#12-code-review)
13. [Context Cost](#13-context-cost)
14. [Practical Case Study](#14-practical-case-study)
15. [Key Takeaways](#15-key-takeaways)
- [Quick Reference Card](#quick-reference-card)

---

## 1. Understanding Claude Code

Claude Code is an **agent loop** — it reads context (your instructions, code files, history), decides which tool to invoke (read, write, search, run commands), checks the result, and continues until the task is done. It is not a "chat bot"; it is an agent with hands on your filesystem and terminal.

Key built-in tool categories:
- **File operations**: Read / Write / Edit
- **Search**: Grep / Glob
- **Terminal**: Bash
- **Web access**: WebFetch
- **Code intelligence**: LSP go-to-definition, find references, etc.

> 💡 Guide Claude's tool selection with natural language: "first search which files reference this function" works far better than "fix my code."

---

## 2. Memory System

Every new session starts from scratch. The memory system solves this.

### 2.1 CLAUDE.md — Your Instruction File

`CLAUDE.md` is read at the start of every session. Think of it as "what you'd say every time you sit down to work."

| Level | Path | Scope |
|-------|------|-------|
| User | `~/.claude/CLAUDE.md` | Personal preferences, all projects |
| Project | `./CLAUDE.md` | Team conventions, committed to git |
| Local | `./CLAUDE.local.md` | Personal prefs for this project, `.gitignore`'d |

**Path-scoped rules** (`.claude/rules/*.md`) load only when Claude handles matching files:

```markdown
# .claude/rules/api-rules.md
---
paths:
  - "src/api/**/*.ts"
---
All API endpoints must include input validation.
Error responses use { code, message, details } format.
```

**Key principles:**
1. Keep under 200 lines; push scenario-specific rules into `.claude/rules/`
2. Be specific and verifiable: "use 2-space indent" beats "format nicely"
3. Avoid contradictions across layers
4. Use `@path/to/file` to import external files into CLAUDE.md

### 2.2 Auto-Memory

Claude also takes its own notes — when you correct it or it discovers useful patterns, it saves them automatically. Stored in `~/.claude/projects/<project>/memory/` with a `MEMORY.md` index loaded each session. Use `/memory` to review and edit.

---

## 3. Permission Modes

| Mode | Command | Behavior | Use Case |
|------|---------|----------|----------|
| **Default** | `/permissions default` | Ask before every edit/command | First-time use |
| **Accept Edits** | `/permissions accept-edits` | Free file edits, commands need approval | Real-time review in IDE |
| **Plan** | `/plan` | Read-only — analyze, no changes | Major refactor planning |
| **Auto** | `/permissions auto` | Full autonomy with background safety classifier | Trust the direction, no interruptions |

**CI/CD headless mode:**
```bash
claude --dangerously-skip-permissions -p "Run tests and report results"
```
⚠️ Only in controlled environments (CI/CD containers).

**Protected paths** (`.git/`, `.vscode/`, `.claude/`) always require confirmation regardless of mode.

---

## 4. Skills System

Skills are **reusable workflow packages** — a directory with a `SKILL.md` placed in `.claude/skills/`.

```
.claude/skills/
├── code-review/SKILL.md
├── deploy-staging/SKILL.md
└── debug-test/SKILL.md
```

Each `SKILL.md` has YAML frontmatter (metadata) and Markdown body (instructions):

```yaml
---
name: code-review
description: Review code changes for quality, security, and performance
---
```

```markdown
# Code Review Process

1. Read the changed files (git diff)
2. Review each file for:
   - Type safety and boundary conditions
   - Error handling completeness
   - Performance issues (N+1 queries, unnecessary loops)
3. Provide tiered feedback:
   - 🔴 Must fix: bugs or security issues
   - 🟡 Suggested: quality improvements
   - 🟢 Optional: nice-to-have
```

**Trigger modes:** Explicit (`/code-review`) or automatic (Claude matches context).

### Advanced Features

- **Self-contained deep instructions**: When Skills run in subagents (fork mode), they can't see the main conversation — everything needed must be in the Skill body.
- **Subagent isolation**: Heavy analysis runs in a separate context; main session only receives the summary.
- **Effort-level signals**: Use `/effort max` for deep reasoning on complex tasks.
- **Bundled Skills**: Built-in Skills like `/run` (captures project launch recipes) and `/verify` (runtime validation) work together.

---

## 5. Subagents

Subagents run in independent context windows. Use them when auxiliary tasks produce lots of intermediate info you won't need.

| Subagent | Model | Purpose |
|----------|-------|---------|
| **Explore** | Haiku (fast, cheap) | Read-only search and code exploration |
| **Plan** | Inherits session model | Context gathering in Plan Mode |
| **General-purpose** | Inherits session model | Read/write/execute for multi-step tasks |

**Custom subagents** live in `.claude/agents/`:

```yaml
---
name: test-runner
description: Run tests and analyze failures
tools: Bash, Read, Grep
model: sonnet
---
```

Key config fields:
- `tools` — restrict what the subagent can do (reviewers only need `Read, Glob, Grep`)
- `model` — cheaper models for simpler tasks (`haiku`), smarter for complex analysis (`opus`)
- `isolation: "worktree"` — run in a git worktree to avoid affecting the main workspace

---

## 6. Hooks

If Skills teach Claude *how* to do something, Hooks ensure it **happens regardless**. Hooks are deterministic automation executed at specific lifecycle nodes.

| Scenario | Hook Type |
|----------|-----------|
| Auto-format after every edit | `PostToolUse` |
| Block dangerous commands | `PreToolUse` |
| Re-inject context after compaction | `SessionStart` (matcher: `compact`) |

**Example — auto-format with Prettier:**
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [{
          "type": "command",
          "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write"
        }]
      }
    ]
  }
}
```

**Hook types:** `command`, `http`, `mcp_tool`, `prompt`, `agent`

> 💡 CLAUDE.md *asks* Claude to do something (it might forget). Hooks *enforce* it (they always run).

---

## 7. MCP Servers

MCP (Model Context Protocol) lets Claude invoke external tools — database queries, browser automation, Jira integration, custom APIs.

```bash
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem /allowed/dir
claude mcp add github -- npx -y @modelcontextprotocol/server-github
claude mcp add my-api --url https://example.com/mcp
```

**MCP vs Hooks:**

| Dimension | MCP | Hooks |
|-----------|-----|-------|
| Trigger | Claude decides | Event-driven, automatic |
| Purpose | New capabilities | Enforced automation |
| Interaction | Bidirectional | Unidirectional |

**Practical scenarios:**
1. **Database queries** — Claude generates SQL, executes, and analyzes results
2. **Browser automation** — Playwright MCP for E2E testing and visual regression
3. **Project management** — Jira/Linear integration for issue tracking
4. **Custom APIs** — deploy systems, monitoring, internal tooling

---

## 8. Plugin System

Plugins package Skills, Agents, MCP configs, and Hooks for sharing and distribution.

```bash
claude plugin install <plugin-name>
claude plugin install ./my-plugin
claude plugin install https://github.com/user/plugin-repo
claude plugin list
claude plugin update <plugin-name>
```

**Plugin structure:**
```
my-plugin/
├── plugin.json
├── skills/code-review/SKILL.md
├── agents/researcher.json
├── mcp/servers.json
└── hooks/hooks.json
```

> 💡 Plugins are the best vessel for team knowledge — package verified best practices and new colleagues get them on day one.

---

## 9. Model Configuration & Effort

### Models

| Model | Alias | Characteristics |
|-------|-------|----------------|
| **Fable 5** | `claude-fable-5` | Latest generation, strongest overall |
| **Sonnet** | `claude-sonnet-4-6` | Daily coding workhorse, speed/capability balance |
| **Opus** | `claude-opus-4-8` | Complex reasoning, architecture, tough bugs |
| **Haiku** | `claude-haiku-4-5-20251001` | Simple tasks, fast searches, cheapest |

Switch with `/model` or `claude --model opus`.

### Effort Levels

| Level | Use Case |
|-------|----------|
| `low` | Simple transforms (rename, format) |
| `medium` | Regular coding, balanced cost/quality |
| `high` (default) | Most coding tasks |
| `xhigh` | Complex problems needing deep analysis |
| `max` | Deepest reasoning, diminishing returns |

### Fast Mode

`/fast` — same Opus model, up to 2.5× faster response, but higher per-token cost. Best enabled at session start.

---

## 10. Common Workflows

### 10.1 Codebase Exploration
Start broad ("give me an overview"), then narrow down ("how does auth work?").

### 10.2 Bug Fixing
Describe symptoms, not causes. Provide repro steps. Let Claude analyze before fixing. Use Plan Mode for complex bugs.

### 10.3 Refactoring
1. Plan Mode → analyze impact scope
2. Discuss approach
3. Execute step by step, testing after each file
4. Verify no regressions

### 10.4 Test Writing
Claude matches your existing framework, assertion style, and mock patterns. Ask it to explain each test case — if you can't understand the intent, the test needs improvement.

### 10.5 Session Management
- `claude --continue` — resume last interrupted session
- `claude --resume` — pick from session list
- `/compact` — compress context when it's getting full

### 10.6 Parallel Work with git worktrees
Each worktree gets its own directory — launch independent Claude sessions that don't interfere.

---

## 11. Dynamic Workflows

For large-scale tasks (audit entire codebases, migrate 500 files, cross-verify research), a single session hits context limits. Dynamic Workflows solve this: **the script holds the loop and intermediate state; Claude's context only keeps final results.**

| Dimension | Subagent | Skill | Agent Teams | Workflow |
|-----------|----------|-------|-------------|----------|
| What is it | Claude-generated tool author | Instructions Claude follows | Lead agent supervising peers | Runtime-executed script |
| Who decides next | Claude, turn by turn | Claude, following prompts | Lead agent, turn by turn | The script |
| Intermediate results | Claude's context | Claude's context | Shared task list | Script variables |
| What's repeatable | Worker definitions | Instructions | Team definitions | The orchestration itself |
| Scale | Same as subagents | Few per turn | Few long-running peers | Dozens to hundreds per run |

Built-in: `/deep-research` for multi-source investigation, `/workflows` for progress tracking.

---

## 12. Code Review

Run `/code-review` to get multi-dimension analysis of your git diff.

**Severity levels:**
- 🔴 **Important** — fix before merge (bugs, security, data loss)
- 🟡 **Minor** — quality improvements (non-blocking)
- 🟣 **Pre-existing** — already in the codebase, not this PR's fault

**Customize via `REVIEW.md`:** redefine "important" criteria, cap minor issues, skip paths, add project-specific checks.

**Usage:**
```
/code-review           # review current diff
/code-review main      # review against main
/code-review --fix     # auto-fix minor issues
/code-review --comment # post as GitHub PR comments
```

---

## 13. Context Cost

Claude's context window is finite (200K tokens). Every mechanism has a different "context cost":

| Mechanism | When Loaded | Cost |
|-----------|-------------|------|
| CLAUDE.md | Every session, automatically | High |
| Skills | On-demand when called | Medium |
| Subagents | Independent context | Zero (to main) |
| Hooks | Event-triggered | Usually zero |
| MCP | Tool definitions常驻 | Medium |
| Plugins | Combined, depends on content | Varies |
| Path rules | When matching files | Low |

**Decision tree:**
```
Need it every session? → CLAUDE.md
Multi-step reusable flow? → Skill
Lots of intermediate info? → Subagent
Must execute deterministically? → Hook
Need to call external tools? → MCP
Only for specific files? → .claude/rules/
Need to distribute to team? → Plugin
```

**Common mistakes:**
1. Stuffing everything into CLAUDE.md (compliance drops past ~200 lines)
2. Heavy analysis in main session (pollutes context)
3. "Remember to run lint" in CLAUDE.md (use a Hook instead)
4. Too many path-scoped rules (hard to manage, context-heavy)

---

## 14. Practical Case Study

A complete project walkthrough: building a Task Management API (Node.js + Express + TypeScript + SQLite) from scratch, demonstrating how all features work together across nine stages:

| Stage | Task | Feature Used | Why |
|-------|------|-------------|-----|
| 1 | Initialize project | CLAUDE.md + path rules | Set the tone, route specific rules |
| 2 | Create API endpoints | Skills (`/create-api`) | Encapsulate reusable workflows |
| 3 | Code quality | Hooks (PostToolUse + PreToolUse) | Deterministic formatting + safety |
| 4 | Debug & refactor | Subagents (isolated analysis) | Heavy tasks don't pollute main context |
| 5 | Major refactor | Plan Mode + model switching | Analyze before executing, Opus for planning |
| 6 | Code review | `/code-review` + effort tuning | Built-in, adjustable depth |
| 7 | Automated deploy | CI/CD + GitHub Actions | Cloud automation |
| 8 | External tools | MCP (database, project management) | Connect external systems |
| 9 | Team sharing | Plugins (Skills + MCP + Hooks) | Team knowledge distribution |

> Don't try to use everything at once. Start with CLAUDE.md, gradually add Skills and Hooks, then explore Subagents and MCP as you get comfortable.

---

## 15. Key Takeaways

### Memory & Context
- CLAUDE.md: keep it under 200 lines; push scenario-specific rules to `.claude/rules/`
- Auto-memory isn't always accurate — review with `/memory` regularly
- After `/compact`, important info can be lost — write critical instructions in CLAUDE.md

### Skills
- **Self-contained deep instructions** are the key: subagents can't see your conversation history
- Use subagent isolation for heavy tasks (codebase audits, large-scale refactors)
- `/run` codifies "only the old-timer knows how to start this project" knowledge

### Subagents
- Restrict tool permissions (reviewers don't need write access)
- Use `haiku` for simple subagent tasks — much cheaper than Opus
- Subagent results are self-reported — verify critical operations yourself

### Permissions & Workflows
- **Always use Plan Mode** before major changes
- Use **Hooks** for "must do" operations (lint, format, safety checks) — CLAUDE.md is only a suggestion

### MCP & Plugins
- Minimum privilege principle — expose only necessary tools
- Pass secrets via environment variables, never hardcode
- MCP tool definitions consume context even when unused — only add what you need
- Plugins are for team distribution; direct config is simpler for personal use

### Cost
- Enable `/fast` at session start (mid-session is more expensive)
- Reserve workflows for truly large-scale tasks

---

## Quick Reference Card

### Command Cheat Sheet

| Command | Purpose |
|---------|---------|
| `claude` | Launch Claude Code |
| `claude --model opus` | Launch with specific model |
| `claude --dangerously-skip-permissions -p "..."` | CI/CD headless mode |
| `/plan` | Switch to Plan Mode |
| `/permissions auto` | Switch to Auto permission mode |
| `/model` | Switch models |
| `/effort high\|max` | Adjust reasoning depth |
| `/compact` | Compress context |
| `/code-review` | Code review |
| `/deep-research` | Deep research |
| `/workflows` | View workflow progress |
| `/memory` | Manage auto-memory |
| `--continue` | Continue last session |
| `--resume` | Pick session from list |

### Architecture Overview

```
CLAUDE.md          → Sets the tone (always loaded)
Skills             → Encapsulate workflows (on-demand)
Subagents          → Isolate heavy tasks (independent context)
Hooks              → Enforce deterministic automation (event-driven)
MCP                → Extend capabilities (external tools)
Plugins            → Package & distribute team knowledge
Path Rules         → File-scoped conventions (conditional)
```

---

*This guide is based on hands-on experience with Claude Code v2.x. Features and commands may evolve. Always check the [official documentation](https://code.claude.com/docs/en/) for the latest information.*
