# Claude Code Usage Guide

> 📖 Official documentation: https://code.claude.com/docs/
> 📋 Written for Claude Code v2.x, last updated: 2026/06/17
> Features described may differ from the latest version; please refer to the [official documentation](https://code.claude.com/docs/) for the most current information.

This guide is aimed at developers who are already familiar with the basics, organized by functional modules, and incorporates hands-on experience.

---

## Table of Contents

- [Chapter 1: Understanding How Claude Code Works](#chapter-1-understanding-how-claude-code-works)
- [Chapter 2: Memory System — Making Claude Remember Your Project](#chapter-2-memory-system--making-claude-remember-your-project)
  - [2.1 CLAUDE.md: The Manual You Write for Claude](#21-claudemd-the-manual-you-write-for-claude)
  - [2.2 Automatic Memory: Notes Claude Takes on Its Own](#22-automatic-memory-notes-claude-takes-on-its-own)
- [Chapter 3: Permission Modes — Controlling Claude's Autonomy](#chapter-3-permission-modes--controlling-claudes-autonomy)
  - [3.1 Permission Modes, from Strictest to Most Permissive](#31-permission-modes-from-strictest-to-most-permissive)
  - [3.2 Protected Paths](#32-protected-paths)
- [Chapter 4: Skills System — Claude Code's Superpower](#chapter-4-skills-system--claude-codes-superpower)
  - [4.1 Why Skills Matter](#41-why-skills-matter)
  - [4.2 Creating Your First Skill](#42-creating-your-first-skill)
  - [4.3 Triggering Methods](#43-triggering-methods)
  - [4.4 Advanced Feature 1: Self-Contained In-Depth Instructions](#44-advanced-feature-1-self-contained-in-depth-instructions)
  - [4.5 Advanced Feature 2: Isolated Execution in Subagents](#45-advanced-feature-2-isolated-execution-in-subagents)
  - [4.6 Advanced Feature 3: Deep Reasoning Signals](#46-advanced-feature-3-deep-reasoning-signals)
  - [4.7 Advanced Feature 4: Bundled Skills Working Together](#47-advanced-feature-4-bundled-skills-working-together)
  - [4.8 Practical Recommendations for Skills](#48-practical-recommendations-for-skills)
- [Chapter 5: Subagents — Independent AI Workers](#chapter-5-subagents--independent-ai-workers)
  - [5.1 Built-in Subagents](#51-built-in-subagents)
  - [5.2 Creating Custom Subagents](#52-creating-custom-subagents)
- [Chapter 6: Hooks — Deterministic Automation](#chapter-6-hooks--deterministic-automation)
  - [6.1 When to Use Hooks](#61-when-to-use-hooks)
  - [6.2 Configuration Examples](#62-configuration-examples)
  - [6.3 Hook Types](#63-hook-types)
- [Chapter 7: MCP Servers — Extending Claude's Capabilities](#chapter-7-mcp-servers--extending-claudes-capabilities)
  - [7.1 Core Concepts](#71-core-concepts)
  - [7.2 Adding and Managing MCP Servers](#72-adding-and-managing-mcp-servers)
  - [7.3 MCP vs. Hooks](#73-mcp-vs-hooks)
  - [7.4 Practical Scenarios](#74-practical-scenarios)
  - [7.5 Best Practices](#75-best-practices)
- [Chapter 8: Plugin System — Sharing and Distributing Extensions (Preview / Unreleased)](#chapter-8-plugin-system--sharing-and-distributing-extensions-preview--unreleased)
  - [8.1 What Are Plugins](#81-what-are-plugins)
  - [8.2 Installing and Managing Plugins](#82-installing-and-managing-plugins)
  - [8.3 Creating Your Own Plugins](#83-creating-your-own-plugins)
  - [8.4 Distributing Plugins](#84-distributing-plugins)
  - [8.5 Plugins vs. Direct Configuration](#85-plugins-vs-direct-configuration)
- [Chapter 9: Model Configuration and Effort Tuning](#chapter-9-model-configuration-and-effort-tuning)
  - [9.1 Choosing the Right Model](#91-choosing-the-right-model)
  - [9.2 Effort Levels](#92-effort-levels)
  - [9.3 Quick Mode](#93-quick-mode)
- [Chapter 10: Common Workflows and Best Practices](#chapter-10-common-workflows-and-best-practices)
  - [10.1 Codebase Exploration: Quickly Understanding an Unfamiliar Project](#101-codebase-exploration-quickly-understanding-an-unfamiliar-project)
  - [10.2 Bug Fixing: From Symptoms to Root Cause](#102-bug-fixing-from-symptoms-to-root-cause)
  - [10.3 Refactoring: Safely Improving Code Structure](#103-refactoring-safely-improving-code-structure)
  - [10.4 Test Writing: Generating High-Quality Test Cases](#104-test-writing-generating-high-quality-test-cases)
  - [10.5 Session Management: Maintaining Continuity Across Sessions](#105-session-management-maintaining-continuity-across-sessions)
  - [10.6 Parallel Work: Handling Multiple Tasks with Git Worktrees](#106-parallel-work-handling-multiple-tasks-with-git-worktrees)
- [Chapter 11: Dynamic Workflows — Orchestrating Large-Scale Tasks](#chapter-11-dynamic-workflows--orchestrating-large-scale-tasks)
  - [11.1 Core Idea: Moving the Plan into Code](#111-core-idea-moving-the-plan-into-code)
  - [11.2 When to Use Workflows](#112-when-to-use-workflows)
  - [11.3 How to Use Them](#113-how-to-use-them)
  - [11.4 Quality Patterns for Workflows](#114-quality-patterns-for-workflows)
- [Chapter 12: Code Review — Automated Code Review](#chapter-12-code-review--automated-code-review)
  - [12.1 Review Mechanism](#121-review-mechanism)
  - [12.2 Review Results](#122-review-results)
  - [12.3 Customizing Review Behavior](#123-customizing-review-behavior)
  - [12.4 Local Usage](#124-local-usage)
  - [12.5 GitHub App Integration](#125-github-app-integration)
- [Chapter 13: Understanding Context Costs — Choosing the Right Extension Mechanism](#chapter-13-understanding-context-costs--choosing-the-right-extension-mechanism)
  - [13.1 What Is Context Cost](#131-what-is-context-cost)
  - [13.2 Context Costs of Each Mechanism](#132-context-costs-of-each-mechanism)
  - [13.3 Decision Flow](#133-decision-flow)
  - [13.4 Common Mistakes](#134-common-mistakes)
- [Chapter 14: Practical Case Study: Building a Task Management API from Scratch](#chapter-14-practical-case-study-building-a-task-management-api-from-scratch)
  - [14.1 Project Background](#141-project-background)
  - [14.2 Overall Workflow Steps](#142-overall-workflow-steps)
  - [14.3 Step 1: Project Initialization and Memory System Setup](#143-step-1-project-initialization-and-memory-system-setup)
  - [14.4 Step 2: Creating API Endpoints](#144-step-2-creating-api-endpoints)
  - [14.5 Step 3: Code Quality Assurance](#145-step-3-code-quality-assurance)
  - [14.6 Step 4: Debugging and Refactoring](#146-step-4-debugging-and-refactoring)
  - [14.7 Step 5: Major Refactoring](#147-step-5-major-refactoring)
  - [14.8 Step 6: Code Review](#148-step-6-code-review)
  - [14.9 Step 7: Automated Deployment](#149-step-7-automated-deployment)
  - [14.10 Step 8: Integrating External Tools](#1410-step-8-integrating-external-tools)
  - [14.11 Step 9: Sharing Team Workflows](#1411-step-9-sharing-team-workflows)
  - [14.12 Lessons Learned](#1412-lessons-learned)
- [Chapter 15: Key Takeaways](#chapter-15-key-takeaways)

---

## Chapter 1: Understanding How Claude Code Works

Before diving into individual features, it's important to understand Claude Code's underlying operating mechanism.

Claude Code is essentially an **agent loop**: it reads context (your instructions, code files, conversation history), decides which tool to invoke (read files, write files, run commands, search code), checks the results, and then proceeds to the next step — repeating this cycle until the task is complete.

This means Claude Code is not a "you ask, it answers" chatbot, but rather an **agent with hands and feet**. It can operate on your file system, execute terminal commands, and search your entire codebase. Understanding this is key to understanding the design intent behind all subsequent features — they all control and enhance this agent loop at different levels.

The built-in tools can be roughly categorized as follows:
- **File operations**: Read / Write / Edit
- **Search**: Grep / Glob
- **Terminal execution**: Bash
- **Web access**: WebFetch
- **Code intelligence**: LSP go-to-definition, find references, etc.

Claude automatically selects the appropriate combination of tools based on the task at hand.

> 💡 **Tip**: You can guide Claude's tool selection using natural language. For example, saying "first search for which files reference this function" will cause it to prioritize Grep over directly Reading the entire directory. Giving clear step-by-step guidance is far more effective than vaguely saying "help me fix the code."

---

## Chapter 2: Memory System — Making Claude Remember Your Project

Every new session, Claude starts from scratch. It doesn't remember the bug you had it fix last time, nor does it remember you said "use pnpm, not npm." The Memory System exists to solve this problem.

### 2.1 CLAUDE.md: The Manual You Write for Claude

CLAUDE.md is an instruction file that you proactively write, which Claude automatically reads at the start of every session. Think of it as "the things you'd have to explain every time you start work" — if you find yourself repeating the same instructions, they should go into CLAUDE.md.

It has several levels that take effect from broadest to narrowest scope:

| Level | Path | Purpose |
|-------|------|---------|
| User-level | `~/.claude/CLAUDE.md` | Your personal preferences, applicable across all projects |
| Project-level | `./CLAUDE.md` | Team-shared conventions, committed to git |
| Local-level | `./CLAUDE.local.md` | Your personal preferences for this project, added to `.gitignore` |

There's also an easily overlooked mechanism: the `.claude/rules/` directory. You can split rules into multiple files and scope them by file path. For example:

```markdown
# .claude/rules/api-rules.md
---
paths:
  - "src/api/**/*.ts"
---
All API endpoints must include input validation.
Error responses use a unified format: { code, message, details }.
```

This rule is only loaded when Claude processes TypeScript files under `src/api/`. This is very practical — your frontend rules won't interfere with backend code, and vice versa, while also saving context space.

#### 2.1.1 Key Principles for Writing CLAUDE.md

Writing CLAUDE.md is not about more is better. It essentially consumes the context window of every session, and writing too much can actually reduce Claude's compliance. A few key points:

1. **Keep it under 200 lines**. If it exceeds that, consider moving scenario-specific rules into `.claude/rules/` for on-demand loading.
2. **Make it specific enough to verify**. "Use 2-space indentation" is far more effective than "format code nicely"; "Run `npm test` before committing" is far more effective than "remember to test."
3. **Avoid contradictions**. If the project-level says "use ESLint" and the user-level says "use Prettier," Claude will pick one at random. Review regularly and clean up conflicts.
4. **Use `@path/to/file` to import external files**. For example `@README.md` or `@package.json` — Claude will expand them at startup. But note that imported content also consumes context.

### 2.2 Automatic Memory: Notes Claude Takes on Its Own

In addition to the CLAUDE.md you write, Claude also takes notes for itself. When you correct it ("this project uses Bun, not npm"), or when it discovers useful information on its own ("tests require a local Redis instance to be running first"), it saves these automatically.

> ⚠️ **Note**: The exact storage location and indexing mechanism for automatic memory may change with version updates. In the current version, memory files are stored in the `~/.claude/projects/<project>/memory/` directory, and the index file (`MEMORY.md`) is loaded at every session start.

The automatic memory index file (`MEMORY.md`) is loaded at every session start, providing an overview of all memory entries. Detailed memory content is stored in separate Markdown files, which Claude retrieves and loads based on semantic relevance when needed. This design avoids cramming all memories into the context at once while ensuring critical information is always accessible.

> 💡 **Tip**: Use the `/memory` command to view and edit these memories at any time. They are plain Markdown files — you can manually clean up inaccurate content. If you find Claude repeatedly making the same mistake, instead of correcting it each time, just say "remember this" or manually add it to CLAUDE.md.

---

## Chapter 3: Permission Modes — Controlling Claude's Autonomy

Claude Code can read/write files and execute commands, which means it could potentially perform operations you don't want. Permission modes are your "trust level dial" for controlling this.

### 3.1 Permission Modes, from Strictest to Most Permissive

| Mode | Command | Characteristics | Use Case |
|------|---------|-----------------|----------|
| **Default** | `/permissions default` | Asks you before every edit or command | Just getting started |
| **Accept Edits** | `/permissions accept-edits` | Freely edits files, but requires approval for running commands | Reviewing code changes in real time in your editor |
| **Plan** | `/plan` | Can only read files and analyze, cannot modify | Major refactoring or planning before fixing complex bugs |
| **Auto** | `/permissions auto` | Fully autonomous, but with a safety classifier reviewing in the background | Trusting the task direction without wanting to be interrupted repeatedly |

> 💡 **Tip**: When doing major refactoring or fixing complex bugs, first switch to Plan mode to let Claude propose a plan. Once you confirm the approach is sound, switch back to execution mode. This is far more efficient than letting it dive in directly and ending up with a bunch of unwanted changes.

Press the `m` key in the CLI to quickly switch modes.

#### 3.1.1 CI/CD Headless Mode

When running in CI/CD pipelines or containers, use command-line flags to skip permission confirmations:

```bash
claude --dangerously-skip-permissions -p "Run tests and report results"
```

⚠️ **Note**: This flag skips all permission checks. Use it only in controlled environments (such as CI/CD containers), not in local development environments.

### 3.2 Protected Paths

Regardless of which mode is active (except for CI/CD's `--dangerously-skip-permissions` mode), writes to directories like `.git/`, `.vscode/`, `.claude/`, etc. require confirmation. This is a safety net to prevent Claude from accidentally corrupting repository state or its own configuration.

---

## Chapter 4: Skills System — Claude Code's Superpower

Skills are the most powerful yet most underrated feature in Claude Code. Simply put, a Skill is a **reusable workflow package** — you bundle together the steps, caveats, and even real-time data queries for a particular task, and Claude loads it when needed, executing according to the workflow you defined.

📖 See the official documentation: Skills

### 4.1 Why Skills Matter

Imagine these scenarios:
- Every time you have Claude do a code review, you have to repeat "first check type safety, then error handling, then performance"
- You have a multi-step deployment process that you have to explain from scratch each time
- You want Claude to run a few commands to get the current state before analyzing code

These repetitive, multi-step workflows that require specific context are exactly what Skills are designed to solve.

### 4.2 Creating Your First Skill

A Skill is essentially a directory containing a `SKILL.md` file, placed under `.claude/skills/`:

```
.claude/skills/
├── code-review/
│   └── SKILL.md
├── deploy-staging/
│   └── SKILL.md
└── debug-test/
    └── SKILL.md
```

`SKILL.md` consists of two parts: YAML frontmatter (metadata) and Markdown body (specific instructions).

```yaml
---
name: code-review
description: Review code changes for quality, security, and performance
---
```

```markdown
# Code Review Process

When asked to review code, follow these steps:

1. First read the list of changed files (using git diff)
2. Review each file, focusing on:
   - Type safety and edge cases
   - Whether error handling is complete
   - Any performance concerns (N+1 queries, unnecessary loops, etc.)
3. Provide tiered feedback:
   - 🔴 Must fix: will cause bugs or security issues
   - 🟡 Suggested improvement: code quality can be enhanced
   - 🟢 Optional optimization: nice to have
```

### 4.3 Triggering Methods

Skills have two triggering methods:

#### 4.3.1 Explicit Invocation

In Claude Code, type `/code-review` and Claude will load this Skill and execute according to the workflow defined inside it.

#### 4.3.2 Automatic Triggering

Claude also automatically matches and loads relevant Skills based on the task context. For example, when you say "help me review this PR," Claude may automatically load the code-review Skill.

> 💡 **Tip**: The more accurate the `description` field of a Skill, the higher the hit rate for automatic matching.

### 4.4 Advanced Feature 1: Self-Contained In-Depth Instructions

When a Skill needs to run in a subagent (fork mode), the subagent cannot see the main conversation's context, so the Skill body must be **self-contained** — all necessary instructions, context data, and judgment criteria must be written explicitly.

```yaml
---
name: architecture-audit
description: Analyze project architecture, identify coupling issues and improvement directions
---
```

```markdown
Analyze the overall architecture of this project:

### Prerequisite Information Gathering
First, execute the following commands to get the project state:
1. `find src -type d -maxdepth 2` — Get directory structure
2. `cat package.json` — Get dependencies and tech stack
3. `git log --oneline -20` — Understand recent activity

### Analysis Steps
1. Scan the directory structure to identify module boundaries
2. Analyze inter-module dependencies (use Grep to search for import statements)
3. Find areas of high coupling and low cohesion
4. Provide specific improvement recommendations

### Output Format
Return a structured analysis report containing:
- Module inventory and their responsibilities
- Dependency graph (text format)
- Risk rating (High/Medium/Low)
- Specific improvement recommendations
```

#### 4.4.1 Key Design Decisions

- Explicitly write "Prerequisite Information Gathering" in the Skill so the subagent executes it itself, rather than assuming it knows the current state
- Define the output format to ensure results are structured and actionable
- Each step has a specific execution method (which tools to use, how to search), avoiding vague instructions

#### 4.4.2 Applicable Scenarios

- Workflows that need current environment information (git status, environment variables, dependency versions, etc.)
- Processes that need to make decisions based on real-time data
- Debugging workflows (first automatically collect logs, error messages, etc.)

### 4.5 Advanced Feature 2: Isolated Execution in Subagents

When a Skill needs to read a large number of files or perform deep analysis, its intermediate process will fill up your main conversation context. The solution is to have Claude execute this Skill in a **subagent**.

Subagents run in an independent context window and do not inherit the main conversation's history. When finished, they return only the final result to your main session.

#### 4.5.1 Caveats

- Results returned by subagents are self-reported; for important decisions, it's recommended that you verify key findings yourself
- For subagent self-containment requirements, see [5.2 Creating Custom Subagents](#52-creating-custom-subagents)

### 4.6 Advanced Feature 3: Deep Reasoning Signals

For particularly complex tasks, you can guide the model to perform deeper reasoning by adjusting the effort level or using specific prompt signals:

```
> /effort max
> This is a complex architecture refactoring task. Before starting:
> 1. Fully analyze all involved modules and their dependency relationships
> 2. Identify critical paths and risk points
> 3. Develop a step-by-step execution plan, ensuring code remains runnable after each step
> ...
```

The `/effort` level controls Claude's reasoning depth. For the complete effort level table, see [9.2 Effort Levels](#92-effort-levels).

> ⚠️ **Note**: The `ultrathink` keyword circulating in the community is not a feature documented in official docs — it's more of a psychological prompt signal used by users. The officially supported deep reasoning control method is the `/effort` command.

### 4.7 Advanced Feature 4: Bundled Skills Working Together

Claude Code comes with several interlinked Skills built in:

- `/run`: Starts your application. If the project startup process is complex (requiring specific environment variables, multi-step builds, dependent services), `/run` will automatically capture the startup recipe and generate a dedicated run skill
- `/verify`: Verifies code changes while the application is running — not just running tests, but actually checking runtime behavior

> 💡 **Tip**: Many projects have the "only veterans know how to start it" problem. Use `/run` to solidify this knowledge so that newcomers (including Claude) can start the app with one command.

### 4.8 Practical Recommendations for Skills

1. **Start with your repetitive work**: When you find yourself explaining the same process to Claude for the third time, it's time to turn it into a Skill
2. **Maintain single responsibility**: One Skill does one thing. Don't write a "universal Skill" trying to cover all scenarios
3. **Make steps specific**: "`npm test -- --coverage`" is far better than "run tests"
4. **Include verification steps**: Tell Claude how to confirm the task is done — "all tests pass and coverage is no less than 80%"
5. **Document pitfalls you've encountered**: Write in the Skill "Note: this API times out when concurrency exceeds 100" so Claude won't make the same mistake again
6. **Isolate heavy analysis in subagents**: Tasks like codebase audits and large-scale searches should be executed in isolation to avoid polluting your main conversation

---

## Chapter 5: Subagents — Independent AI Workers

Subagents are AI assistants that run in independent context windows. When a supporting task would generate a large amount of intermediate information (search results, log analysis, file contents) that you won't need the details of afterward, hand it to a subagent — it returns only a summary.

📖 See the official documentation: Sub-agents

### 5.1 Built-in Subagents

Claude Code comes with several subagents that are used automatically:

| Subagent | Model | Purpose |
|----------|-------|---------|
| **Explore** | Haiku (fast, cheap) | Read-only operations only, suitable for searching and code exploration |
| **Plan** | Inherits main session model | Collects context in Plan Mode to inform plan development |
| **General-purpose** | Inherits main session model | Can read, write, and execute — handles complex multi-step tasks |

### 5.2 Creating Custom Subagents

Subagents are also Markdown files, placed under `.claude/agents/` or `~/.claude/agents/`:

```yaml
---
name: test-runner
description: Run tests and analyze failure causes
tools: Bash, Read, Grep
model: sonnet
---
```

```markdown
You are a test analysis expert. When asked to run tests:
1. Execute the project's test command
2. If there are failing tests, analyze the failure causes
3. Distinguish between "real bugs" and "tests that need updating"
4. Provide specific fix recommendations
```

#### 5.2.1 Key Configuration Fields Explained

| Field | Description |
|-------|-------------|
| `tools` | Restricts which tools the subagent can use. For review-type tasks, `Read, Glob, Grep` is sufficient — no write permissions needed |
| `model` | You can assign different models to subagents. Use `haiku` for simple tasks (fast and cheap), `opus` for complex analysis |
| `isolation: "worktree"` | Runs the subagent in a git worktree (an isolated repository copy), suitable for scenarios that need actual file modifications without affecting the main workspace |

> ⚠️ **Note**:
> - Confirm current version support for the `isolation` field
> - The `context: fork` field mentioned in earlier documentation is not a standard subagent configuration

> 💡 **Tip**: The biggest value of subagents isn't just "isolation" — it's also **cost control**. A subagent using the Haiku model for code search costs far less than doing the same thing in the main session with Opus. Hand off exploratory tasks with lots of intermediate output to subagents to keep the main session clean.

---

## Chapter 6: Hooks — Deterministic Automation

If Skills are "teaching Claude how to do something," then Hooks are "regardless of what Claude thinks, this thing will definitely happen." Hooks are commands that execute automatically at specific points in the Claude Code lifecycle, providing **deterministic control**.

📖 See the official documentation: Hooks

### 6.1 When to Use Hooks

| Scenario | Hook Type |
|----------|-----------|
| Every time Claude finishes editing a file, you want to automatically run a formatting tool | `PostToolUse` |
| You want to prevent Claude from executing certain dangerous commands | `PreToolUse` |
| You want to receive a desktop notification after Claude finishes its work | `Notification` (⚠️ This hook type may change with version updates; please confirm current version support) |
| After context compression, you want to re-inject critical information | `SessionStart` (with matcher `compact`) |

### 6.2 Configuration Examples

Hooks are configured in `.claude/settings.json`. For example, to automatically format with Prettier every time Claude edits a file:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write"
          }
        ]
      }
    ]
  }
}
```

> ⚠️ **Prerequisite**: This Hook requires the `jq` tool to be installed on the system. If `file_path` parsing fails, the command may format the wrong file. A safer approach is to add path validation in the script.

Another example, preventing Claude from executing `rm -rf` commands:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": ".claude/hooks/block-destructive.sh"
          }
        ]
      }
    ]
  }
}
```

The script reads JSON-formatted tool call information from stdin, checks the command content, and if it contains `rm -rf`, exits with exit code 2 (indicating blocking).

### 6.3 Hook Types

In addition to the most common `command` (Shell command), the following are also supported:

| Type | Description |
|------|-------------|
| `command` | Shell command |
| `http` | POST to a specified URL, suitable for integrating external services |
| `mcp_tool` | Calls a tool on an MCP server |
| `prompt` | Has the model make a single-turn judgment (e.g., "is this operation safe") |
| `agent` | Has a subagent perform multi-turn verification ⚠️ This feature may change in future versions |

> 💡 **Tip**: The difference between Hooks and CLAUDE.md is important. CLAUDE.md is a "request" for Claude to do something (it might forget), while Hooks are **enforced execution** (they run regardless of what Claude thinks). For linting, formatting, security checks, and other operations that must be executed, use Hooks rather than writing "remember to run lint" in CLAUDE.md.

---

## Chapter 7: MCP Servers — Extending Claude's Capabilities

MCP (Model Context Protocol) is the core mechanism of Claude Code's extension ecosystem. Through MCP, you can add any external tool capabilities to Claude — from database queries to browser automation, from Jira integration to custom API calls.

📖 See the official documentation: MCP Servers

### 7.1 Core Concepts

The essence of MCP is enabling Claude to call external tools. Think of it as installing "plugins" for Claude — each MCP server provides a set of tools that Claude can proactively invoke based on task needs.

Unlike Hooks, MCP is **bidirectional and interactive**: Claude decides when to call and with what parameters, the MCP server executes and returns results. Hooks, on the other hand, are event-driven — they execute automatically when specific events occur, and Claude is not involved in the decision.

### 7.2 Adding and Managing MCP Servers

```bash
# Add a filesystem MCP server (allowing access to a specified directory)
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem /path/to/allowed/dir

# Add a GitHub MCP server (requires token configuration)
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# Add an MCP server with a custom URL
claude mcp add my-api --url https://example.com/mcp

# List all configured MCP servers
claude mcp list

# Remove an MCP server
claude mcp remove filesystem
```

**Configuration scope**:
- `--scope user`: Global configuration, applies to all projects (default)
- `--scope project`: Project-level configuration, applies only to the current project, saved in `.claude/settings.json`

> 💡 **Tip**: Sensitive information (such as API tokens) should be passed via environment variables rather than hardcoded in configuration. Most MCP servers support reading configuration from environment variables.

### 7.3 MCP vs. Hooks

| Dimension | MCP Servers | Hooks |
|-----------|-------------|-------|
| Trigger method | Claude proactively calls | Automatically triggered by events |
| Purpose | Provides new tools/capabilities | Automates predetermined operations |
| Interactivity | Bidirectional — Claude decides when to use | One-way execution — runs when triggered |
| Context | Results return to Claude, entering context | Typically does not enter context |
| Examples | Database queries, API calls, browser operations | Auto-formatting, blocking dangerous commands, notifications |

**When to use MCP vs. Hooks**:
- **Use MCP**: When you need Claude to proactively decide when to call and with what parameters
- **Use Hooks**: When an operation must be executed without Claude's decision-making

### 7.4 Practical Scenarios

#### 7.4.1 Scenario 1: Database Queries

Through PostgreSQL/MySQL MCP, Claude can directly query databases, helping you debug SQL and analyze data:

```bash
# Add PostgreSQL MCP
claude mcp add postgres -- npx -y @modelcontextprotocol/server-postgres

# Configure connection information (via environment variables)
export POSTGRES_HOST=localhost
export POSTGRES_PORT=5432
export POSTGRES_USER=<your_user>
export POSTGRES_PASSWORD=<your_password>
export POSTGRES_DB=<your_db>
```

> ⚠️ **Note**: The MCP ecosystem evolves rapidly, and specific server package names may be updated. Please visit [github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) to confirm the latest available package names.

Usage examples:

```
> Query the number of users in the users table who registered in the last 7 days
> Analyze the average order amount per user in the orders table and find the top 10
```

Claude will generate SQL, execute the query, and analyze the results — all without you leaving the terminal.

#### 7.4.2 Scenario 2: Browser Automation

Through Playwright MCP, Claude can directly operate the browser to test the actual running state of web applications:

```bash
claude mcp add playwright -- npx -y @modelcontextprotocol/server-playwright
```

Usage examples:

```
> Open http://localhost:3000, click the login button, enter username and password, verify if it redirects to the homepage
> Take a screenshot of the current page and check for console errors
```

This is particularly useful for end-to-end testing and visual regression testing. Claude can simulate user operations and verify the actual behavior of the application.

#### 7.4.3 Scenario 3: Project Management Integration

Through Jira/Linear MCP, Claude can read issues, update statuses, and create tasks, seamlessly connecting coding work with project management:

```bash
# Add Jira MCP
claude mcp add jira -- npx -y @modelcontextprotocol/server-jira

# Configure Jira information
export JIRA_BASE_URL=https://your-domain.atlassian.net
export JIRA_EMAIL=<your_email>
export JIRA_API_TOKEN=<your_api_token>
```

> ⚠️ **Note**: The MCP ecosystem evolves rapidly, and specific server package names may be updated. Please visit [github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) to confirm the latest available package names.

Usage examples:

```
> View the details of issue PROJ-123
> Create a new issue titled "Fix CORS error on login page" and assign it to me
> Change the status of PROJ-456 to "In Progress"
```

This way you can manage tasks while coding without switching to a browser.

#### 7.4.4 Scenario 4: Custom API Integration

If your team has internal APIs (such as deployment systems, monitoring systems, configuration centers), you can develop custom MCP servers:

```python
# my-mcp-server.py
from mcp.server import Server
from mcp.types import Tool

server = Server("my-api")

@server.tool("deploy")
async def deploy(service: str, environment: str):
    """Deploy a service to a specified environment"""
    # Call internal deployment API
    response = await http_post(f"https://deploy-api.internal/deploy", {
        "service": service,
        "environment": environment
    })
    return {"status": response.status, "message": response.text}

@server.tool("get_metrics")
async def get_metrics(service: str):
    """Get monitoring metrics for a service"""
    # Call monitoring API
    metrics = await http_get(f"https://metrics-api.internal/{service}")
    return metrics
```

Then add it in Claude Code:

```bash
claude mcp add my-api -- python my-mcp-server.py
```

Usage examples:

```
> Deploy user-service to the staging environment
> Check user-service's CPU and memory usage
```

### 7.5 Best Practices

1. **Principle of least privilege**: MCP servers should only expose necessary tools. For example, a filesystem MCP should restrict access to specific directories, not the entire root directory
2. **Environment variable management**: Pass sensitive information (API tokens, passwords) via environment variables, don't hardcode them. You can use `.env` files or system environment variables
3. **Error handling**: MCP servers should return clear error messages. If a tool call fails, Claude needs to know the reason to decide the next step
4. **Performance considerations**: MCP tool calls block Claude's execution. If a tool takes a long time to execute (e.g., large-scale data processing), consider returning a task ID and letting Claude poll for status
5. **Local development**: When developing custom MCP servers, first test with the `mcp dev` command to ensure tool definitions and implementations are correct before integrating into Claude Code

> 💡 **Tip**: MCP is the key to transforming Claude Code from a "code assistant" into an "all-purpose workstation." By connecting databases, browsers, and project management tools via MCP, Claude can directly operate external systems, truly achieving end-to-end automation. But be careful — the more MCP tools you add, the larger Claude's decision space becomes, and the longer decision-making may take. Only add tools you truly need.

---

## Chapter 8: Plugin System — Sharing and Distributing Extensions (Preview / Unreleased)

> ⚠️ **Preview Feature**: This chapter describes functionality based on early/unofficial information. Commands (`claude plugin install/update/list/uninstall`), `plugin.json` format, and directory structures described here **may not match the current official implementation** and could change without notice. **Do not rely on these commands in production workflows.** Please refer to the [official documentation](https://code.claude.com/docs/plugins) for the most current information.

Claude Code's plugin system allows you to install, share, and distribute extension packages containing Skills, Agents, and MCP configurations. This is the core mechanism for team collaboration and community sharing.

📖 See the official documentation: Plugins

### 8.1 What Are Plugins

A plugin is a packaged extension that can contain:
- **Skills**: Reusable workflow instructions
- **Agents**: Custom subagent definitions
- **MCP server configurations**: Pre-configured external tool connections
- **Hooks**: Event-driven automated operations

The essence of a plugin is bundling multiple extension mechanisms together for easy distribution and installation. Think of it as a "feature pack" — install one plugin and you get an entire suite of workflows.

### 8.2 Installing and Managing Plugins

```bash
# Install a plugin (from the official repository)
claude plugin install <plugin-name>

# Install a plugin (from a local path)
claude plugin install ./my-plugin

# Install a plugin (from a Git repository)
claude plugin install https://github.com/user/plugin-repo

# List installed plugins
claude plugin list

# Uninstall a plugin
claude plugin uninstall <plugin-name>

# Update a plugin
claude plugin update <plugin-name>
```

After installation, Skills in the plugin are automatically registered as `/skill-name` commands, Agents automatically appear in the available subagent list, MCP servers are automatically configured, and Hooks are automatically registered.

### 8.3 Creating Your Own Plugins

Plugin directory structure:

```
my-plugin/
├── plugin.json          # Plugin metadata
├── skills/              # Skills directory
│   ├── code-review/
│   │   └── SKILL.md
│   └── deploy/
│       └── SKILL.md
├── agents/              # Agents directory
│   ├── researcher.json
│   └── tester.json
├── mcp/                 # MCP configuration
│   └── servers.json
└── hooks/               # Hooks configuration
    └── hooks.json
```

#### 8.3.1 plugin.json Example

```json
{
  "name": "my-team-workflow",
  "version": "1.0.0",
  "description": "Team standard workflow plugin",
  "author": "Your Team",
  "skills": [
    "skills/code-review",
    "skills/deploy"
  ],
  "agents": [
    "agents/researcher.json",
    "agents/tester.json"
  ],
  "mcp": {
    "servers": "mcp/servers.json"
  },
  "hooks": {
    "config": "hooks/hooks.json"
  }
}
```

#### 8.3.2 Skills Definition

In `skills/code-review/SKILL.md`:

```yaml
---
name: code-review
description: Team standard code review process
---
```

```markdown
# Code Review

### Review Steps
1. Check type safety and boundary conditions
2. Check if error handling is complete
3. Check for performance issues
4. Check for security vulnerabilities

### Output Format
- 🔴 Critical: Must fix before merging
- 🟡 Minor: Suggested improvements
- 🟣 Pre-existing: Not introduced by this change
```

#### 8.3.3 Agents Definition

In `agents/researcher.json`:

```json
{
  "name": "researcher",
  "description": "Codebase research expert",
  "model": "sonnet",
  "tools": ["Read", "Glob", "Grep"],
  "instructions": "You are a codebase research expert. Your task is to understand code structure, trace execution flow, and analyze dependencies. Return only facts, make no modifications."
}
```

#### 8.3.4 MCP Configuration

In `mcp/servers.json`:

```json
{
  "jira": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-jira"],
    "env": {
      "JIRA_BASE_URL": "${JIRA_BASE_URL}",
      "JIRA_EMAIL": "${JIRA_EMAIL}",
      "JIRA_API_TOKEN": "${JIRA_API_TOKEN}"
    }
  }
}
```

#### 8.3.5 Hooks Configuration

In `hooks/hooks.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "prettier --write $CLAUDE_FILE_PATH"
      }
    ]
  }
}
```

### 8.4 Distributing Plugins

Plugins can be distributed in the following ways:

1. **Local path**: `claude plugin install ./my-plugin`
2. **Git repository**: `claude plugin install https://github.com/user/plugin-repo`
3. **Official repository**: Submit a PR to the Claude Code official plugin repository (requires review)

**Team collaboration scenario**:

Place the plugin in your team's private Git repository, and team members can share the same workflow after installing it:

```bash
# Team member installation
claude plugin install https://github.com/your-team/claude-plugin

# After plugin update, team members update
claude plugin update your-team-plugin
```

### 8.5 Plugins vs. Direct Configuration

| Dimension | Plugin | Direct Configuration |
|-----------|--------|---------------------|
| Use case | Needs to be distributed to multiple people | Personal use |
| Update method | `plugin update` | Manually modify files |
| Version management | Has version numbers | None |
| Dependency management | ⚠️ Please confirm current version | Manual handling |

> 💡 **Tip**: Plugins are the best vehicle for team knowledge accumulation. Package your verified best practices (code review processes, deployment steps, debugging techniques) into plugins, and new team members can use them right away upon onboarding. But keep in mind — plugins aren't a cure-all. If it's just for personal use, directly configuring Skills and Hooks is simpler.

---

## Chapter 9: Model Configuration and Effort Level

### 9.1 Choosing the Right Model

Claude Code supports several model options:

| Model | Alias | Features |
|-------|-------|----------|
| **Fable 5** | `claude-fable-5` | Latest generation model, strongest overall capability, suitable for complex tasks |
| **Sonnet** | `claude-sonnet-4-6` | Main workhorse for daily coding, balance of speed and capability |
| **Opus** | `claude-opus-4-8` | Complex reasoning, architecture design, tricky bugs. More expensive but smarter |
| **Haiku** | `claude-haiku-4-5-20251001` | Simple tasks, quick searches. Cheapest and fastest |

Use `/model` to switch at any time, or specify at startup with `claude --model opus`.

> ⚠️ **Note**: `opusplan` is not an officially supported model alias. This combined model does not exist in the official documentation. If you want to use Opus for the planning phase and Sonnet for execution, you can manually switch to `/model opus` in Plan Mode, then switch back to `/model sonnet` for execution after planning is complete.

### 9.2 Effort Levels

Effort levels control how deeply Claude "thinks":

| Level | Use case |
|-------|----------|
| `low` | Simple, straightforward tasks like renaming variables, formatting code |
| `medium` | Regular coding, balancing cost and quality |
| `high` (default) | A suitable choice for most coding tasks |
| `xhigh` | When complex problems require deep analysis |
| `max` | Deepest reasoning, but may overthink with diminishing returns |

Use `/effort` to switch. A practical tip: for particularly complex one-off requests, you can use `/effort max` to trigger deeper reasoning without changing the global setting.

### 9.3 Fast Mode

`/fast` enables fast mode — still Opus, but with response speeds up to 2.5x faster, at the cost of being more expensive per token. Suitable for latency-sensitive scenarios like interactive debugging and real-time iteration. For long-running autonomous tasks, standard mode is more cost-effective.

> 💡 **Tip**: Fast mode is most cost-effective when enabled at the start of a session. Enabling it mid-session requires paying the fast mode premium for the entire existing context.

---

## Chapter 10 Common Workflows and Best Practices

This chapter walks through the most commonly used scenarios in daily development, providing verified prompts and workflows for each. This isn't theory — it's stuff you can use today.

### 10.1 Codebase Exploration: Quickly Understanding Unfamiliar Projects

When you've just taken over a project, or you're looking at old code after a long time, the biggest headache is "how does this thing actually work?" Claude Code can help you tremendously here.

#### 10.1.1 Quickly Getting a Global View

```
> Give me an overall overview of this codebase, including main modules, tech stack, and architectural patterns
```

Claude will scan the directory structure, read key configuration files (`package.json`, `tsconfig.json`, etc.), and give you a structured overview. This is much faster than going through files one by one yourself.

#### 10.1.2 Tracing Execution Flow for a Specific Feature

```
> What's the user authentication flow? Trace it completely from the frontend login button click to backend authentication completion
```

Claude will use Grep to search for relevant keywords (`login`, `auth`, `token`, etc.), Read the involved files, and give you a complete call chain from frontend to backend. This is especially useful when understanding complex business logic.

#### 10.1.3 Locating Specific Types of Code

```
> Find all code that directly operates on the database, I want to understand the data access layer structure
> Which files have hardcoded configuration values?
> Find all places using deprecated APIs
```

> 💡 **Tip**: When exploring code, start with broad questions ("what's the overall architecture"), then gradually narrow down to specific areas ("how does the authentication module work"). Don't ask overly detailed questions right away — build a global mental model first.

### 10.2 Bug Fixing: From Symptoms to Root Cause

Bug fixing is one of the most practical scenarios for Claude Code. The key is giving Claude enough context so it can quickly locate the problem.

#### 10.2.1 Recommended Workflow

**1. Describe the symptom, not the guessed cause**:

❌ Wrong example:

```
> There's a bug on line 42 of user.ts, help me fix it
```

✅ Correct example:

```
> When running npm test, auth.test.ts reports a timeout error, the test times out waiting for the database connection
```

Tell Claude what you see (error messages, stack traces, abnormal behavior), not where you think the problem is. Let Claude determine the root cause itself.

**2. Provide reproduction steps**:

```
> Reproduction steps:
> 1. Start the local server with npm run dev
> 2. Navigate to the /login page
> 3. Enter username and password, click login
> 4. Page gets stuck in loading state, console shows a CORS error
```

**3. Let Claude analyze first, then fix**:

For complex bugs, first enter Plan Mode and have Claude analyze the root cause:

```
> /plan What are the possible causes of this timeout error? Help me analyze
```

Review Claude's analysis, confirm the approach is correct, then let it execute the fix.

**4. Verify the fix**:

```
> Run the tests to confirm the fix doesn't introduce new issues
```

> 💡 **Tip**: If the bug is intermittent, tell Claude this. Intermittent and persistent problems require completely different debugging approaches. Also, if the error message is long, paste the complete stack trace — don't trim what you think are the "important parts" yourself, you might cut the wrong thing.

### 10.3 Refactoring: Safely Improving Code Structure

Refactoring is one of Claude Code's strongest scenarios, because it requires understanding the relationships between multiple pieces of code simultaneously — and that's exactly where Claude excels.

#### 10.3.1 Standard Workflow

**1. First enter Plan Mode to analyze the impact scope**:

```
> /plan I want to split UserService into UserService and AuthService, help me analyze the impact scope
```

Claude will search for all references to UserService, list the files that need modification, and estimate the workload.

**2. Discuss the approach**:

```
> After splitting, do you think authentication-related logic should go in AuthService or a separate AuthModule?
```

This step is important — Claude can give you multiple approaches, and you choose the most suitable one.

**3. Execute the refactoring**:

After confirming the approach, switch back to normal mode:

```
> The approach looks good, go ahead and execute
```

**4. Verify no regressions**:

```
> Run the full test suite to make sure nothing is broken
```

#### 10.3.2 Prompting Tips for Refactoring

- "Maintain backward compatibility": If you need to keep the API unchanged
- "Proceed step by step, code should be runnable after each step": Avoid changing too much at once
- "Change the most core parts first, then gradually expand": Reduce risk

> 💡 **Tip**: For large-scale refactoring, have Claude run tests after each file change, rather than changing all files at once and then testing. That way, if something goes wrong, you'll immediately know which step introduced it.

### 10.4 Test Writing: Generating High-Quality Test Cases

Claude can generate tests that follow your project's existing patterns and conventions. The key is giving it enough context.

#### 10.4.1 Identifying Untested Code

```
> Find functions in NotificationService that aren't covered by tests
```

#### 10.4.2 Generating Tests

```
> Add tests for the notification service, following the project's existing test style
```

Claude will read your existing test files, matching the framework used (Jest, Mocha, etc.), assertion style, and mocking patterns.

#### 10.4.3 Covering Edge Cases

```
> Analyze the code paths of these functions and find edge cases and error conditions I might have missed
```

> 💡 **Tip**: After generating tests, have Claude explain what each test case verifies. If you can't understand a test's intent, it's not well-written — ask Claude to improve it.

### 10.5 Session Management: Maintaining Continuity Across Sessions

Claude Code sessions can be paused and resumed, which is especially useful for tasks that span multiple days.

#### 10.5.1 Continuing from a Previously Interrupted Session

```bash
claude --continue
```

This restores your last interrupted session with the full context preserved. Ideal for when you stopped halfway through yesterday and want to continue today.

#### 10.5.2 Selecting from a Session List to Resume

```bash
claude --resume
```

This displays a list of recent sessions, and you can choose which one to resume. Ideal for when you have multiple parallel tasks and want to switch to a different session.

#### 10.5.3 Context Compaction

When the context is getting full (Claude will notify you), use `/compact` to manually compress:

```
> /compact
```

This retains key information and discards less important intermediate processes. After compaction, Claude may have "forgotten" some details, so important information is best written in CLAUDE.md.

> 💡 **Tip**: If you find Claude repeatedly "forgetting" certain instructions after compaction, those instructions should be written in CLAUDE.md rather than relying on session context.

### 10.6 Parallel Work: Using git worktrees to Handle Multiple Tasks Simultaneously

If you need to do feature development and bug fixes simultaneously, or want to compare different approaches, you can use git worktrees to work on multiple branches at the same time.

#### 10.6.1 Setting Up Worktrees

```bash
# Create a new worktree
git worktree add ../project-feature-a feature-a

# Start Claude in the new directory
cd ../project-feature-a
claude
```

Each worktree is an independent working directory with its own copy of files. You can start a Claude session in each worktree, and they won't interfere with each other.

#### 10.6.2 Typical Scenarios

- One worktree for feature development, another for fixing an urgent bug
- Comparing two different refactoring approaches
- Handling review comments on multiple PRs simultaneously

> 💡 **Tip**: The benefit of worktrees isn't just parallelism, it's also **context isolation**. Each Claude session only sees its own branch's code and isn't polluted by changes from other branches.

---

## Chapter 11 Dynamic Workflows — Orchestrating Large-Scale Tasks

When the scope of what you need to do is large — auditing an entire codebase, batch-migrating 500 files, cross-validating research from multiple sources — a single Claude session may not be enough. Dynamic Workflows are designed for these scenarios.

📖 See the official documentation: Dynamic Workflows

### 11.1 Core Idea: Move the Plan into Code

In a normal Claude session, Claude is the orchestrator: it decides what to do next turn by turn, and every intermediate result goes into the context window. This runs into two problems when the task scale is large:

1. **Context explosion**: Too many intermediate results, the context window can't hold them all
2. **Not reproducible**: If you want to do it again next time, you have to explain everything from scratch

The Dynamic Workflow solution is: **the script holds the loop and intermediate state, Claude's context only retains the final results**.

For example, in a code audit task, the script would:
1. List all modules that need auditing
2. Launch a subagent for each module
3. Collect each subagent's findings
4. Compile into a final report

Claude doesn't need to remember the intermediate process, it only outputs the final conclusions. And this script can be saved and re-run directly next time.

### 11.2 When to Use Workflows

Workflows, Subagents, Skills, and Agent Teams can all run multi-step tasks. The difference is who holds the plan:

| Dimension | Subagents | Skills | Agent Teams | Workflows |
|-----------|-----------|--------|-------------|-----------|
| What it is | Tool authors generated by Claude | Instructions Claude follows | Lead agent overseeing peer sessions | Script executed at runtime |
| Who decides what's next | Claude, turn by turn | Claude, following the prompt | Lead agent, turn by turn | The script |
| Where intermediate results live | Claude's context window | Claude's context window | Shared task list | Script variables |
| What's reproducible | Worker definition | Instructions | Team definition | The orchestration itself |
| Scale | Same as subagents | A few delegations per turn | A few long-running peers | Dozens to hundreds of agents per run |

#### 11.2.1 Typical Scenarios for Using Workflows

- **Codebase-wide bug scanning**: Scan all modules looking for specific patterns of issues
- **Large-scale migration**: 500 files need API call patterns updated
- **Cross-checking research**: Investigate a problem from multiple angles, then cross-validate sources
- **Difficult planning**: Plans worth drafting from multiple independent angles, then weighing against each other

### 11.3 Usage

#### 11.3.1 Built-in Workflow Tools

Claude Code provides some built-in workflow tools:

| Tool | Purpose |
|------|---------|
| `/deep-research` | Investigate questions across multiple sources, automatically fan out searches and synthesize reports |
| `/workflows` | View running workflow progress |

> 💡 Workflow reasoning depth is affected by the `/effort` command, see [9.2 Effort Levels](#92-effort-levels) for details.

#### 11.3.2 Built-in Workflow: /deep-research

The quickest way to experience this is to run `/deep-research`, a built-in Claude Code workflow for investigating questions across multiple sources:

```
> /deep-research What changed in the permission model between Node.js v20 and v22?
```

Claude will:
1. Fan out web searches across multiple angles
2. Fetch and cross-check the sources found
3. Synthesize a report with citations

The process runs in the background while your session stays idle. At the end you get a report, not a turn-by-turn search log.

#### Having Claude Write Workflows for Your Tasks

For custom tasks, you can have Claude write workflow scripts for you:

```
> /effort max
> Audit the entire codebase, find all places using deprecated APIs, generate a report organized by module
```

Claude will automatically analyze your task, determine whether an orchestration workflow is needed, and generate the appropriate orchestration script. The script runs in the background while you continue doing other things.

#### Viewing Workflow Progress

```
> /workflows
```

This displays all running workflows, and you can choose to view detailed progress of a specific one.

### 11.4 Quality Patterns for Workflows

Workflows aren't just about "running more agents" — they can also apply quality patterns:

- **Adversarial review**: Have independent agents review each other's findings before reporting, to catch omissions
- **Multi-angle drafting**: Draft plans from multiple angles, then weigh them against each other for results more credible than a single pass
- **Cross-validation**: Multiple agents independently investigate the same question, then cross-check sources

> 💡 **Tip**: Large-scale tasks are suited for scenarios where you'd instinctively think "this needs to be broken into several steps." You just need to describe the goal, and Claude will decide how to break it down and orchestrate it. Don't try to manually control every step — at that point you might as well not use workflows.

---

## Chapter 12 Code Review — Automated Code Review

Claude Code has a built-in code review system. It's not just a simple lint check — it performs multi-dimensional analysis of your code changes, looking for logic errors, security vulnerabilities, and potential regressions.

📖 See the official documentation: Code Review

### 12.1 Review Mechanism

When you run `/code-review`, Claude will:

1. **Get changes**: Run `git diff` to get all code changes
2. **Multi-dimensional analysis**: Review file by file, checking:
   - Type safety and boundary conditions
   - Whether error handling is complete
   - Performance issues (N+1 queries, unnecessary loops, etc.)
   - Security vulnerabilities (SQL injection, XSS, etc.)
3. **Generate report**: Output a graded review report

### 12.2 Review Results

Each finding is marked with severity:

| Marker | Meaning |
|--------|---------|
| 🔴 **Critical** | Issues that should be fixed before merging. Will cause bugs, security vulnerabilities, or data loss |
| 🟡 **Minor** | Worth improving but not blocking. Code quality can be enhanced but functionality isn't affected |
| 🟣 **Pre-existing** | Issues already in the codebase but not introduced by this PR. Noted for awareness but not required to fix in this PR |

#### 12.2.1 Example Report

```
Code review complete. Found the following issues:

🔴 Critical (2)
- src/api/users.ts:45 - SQL query directly concatenates user input, SQL injection risk
  Suggestion: Use parameterized queries
- src/services/payment.ts:78 - No transaction rollback on payment failure
  Suggestion: Call transaction.rollback() in the catch block

🟡 Minor (3)
- src/utils/logger.ts:12 - Log level hardcoded to debug, should use info in production
- src/routes/auth.ts:34 - Error message exposes internal implementation details
- tests/user.test.ts:56 - Test data not cleaned up, may affect other tests

🟣 Pre-existing (1)
- src/db/connection.ts:8 - Database connection pool has no max connections configured (not introduced by this PR)
```

### 12.3 Customizing Review Behavior

> ⚠️ **Note**: The `REVIEW.md` mechanism is a community practice; the following examples are based on community usage. The official documentation may use different configuration methods — please consult the [Code Review official documentation](https://code.claude.com/docs/code-review) to confirm.

Review behavior can be finely controlled through the `REVIEW.md` file:

#### 12.3.1 Redefining "Critical" Standards

```markdown
# REVIEW.md
For prototype projects, all style issues are downgraded to "Minor."
Only issues that would cause data loss or security vulnerabilities are marked as "Critical."
```

#### 12.3.2 Limiting the Number of Minor Issues

```markdown
# REVIEW.md
Report at most 5 minor issues. Prioritize the most severe ones.
Avoid drowning out truly important findings with review comments.
```

#### 12.3.3 Skipping Specific Paths

```markdown
# REVIEW.md
Skip review for the following paths:
- generated/** (auto-generated code)
- *.lock (lockfiles)
- migrations/** (database migration scripts)
```

#### 12.3.4 Adding Project-Specific Checks

```markdown
# REVIEW.md
Additional checks:
- New API routes must have integration tests
- All database queries must use parameterized queries
- Password-related operations must use bcrypt with at least 10 rounds
```

### 12.4 Local Usage

You don't need to install the GitHub App to use it. Run `/code-review` in any session to review the current diff:

```
> /code-review
```

This reviews all changes since the last commit.

#### 12.4.1 Reviewing Differences Against a Specific Branch

```
> /code-review main
```

This reviews all changes on the current branch relative to the main branch.

#### 12.4.2 Auto-Fixing Discovered Issues

```
> /code-review --fix
```

Claude will attempt to automatically fix discovered issues (mainly minor ones; critical issues will be presented for your confirmation).

#### 12.4.3 Reviewing at Different Depth Levels

```
> /code-review --effort high
```

Adjust the review depth. `high` or `max` will perform more thorough analysis but consume more resources.

#### 12.4.4 Publishing Review Results as PR Comments

```
> /code-review --comment
```

Publish review findings as inline comments on the GitHub PR.

> ⚠️ **Note**: This feature requires the GitHub App or appropriate permission configuration.

### 12.5 GitHub App Integration

If you want automatic review on every PR, you can install the Claude Code GitHub App:

1. Install the App on GitHub
2. Authorize access to your repositories
3. When each PR is opened, Claude automatically reviews and posts comments

> 💡 **Tip**: The value of Code Review isn't just finding bugs — it's also **knowledge transfer**. The review report explains "why this is a problem," helping both you and others learn. Regularly review the reports, even when no issues are found, to understand what aspects Claude focuses on.

---

## Chapter 13 Understanding Context Costs — Choosing the Right Extension Mechanism

Claude Code provides multiple extension mechanisms (CLAUDE.md, Skills, Subagents, Hooks, path-scoped rules), but their "context costs" vary greatly. Understanding this is essential for making the right architectural choices — not every feature should be blindly piled on.

### 13.1 What Is Context Cost

Claude's context window is limited (200K tokens). At the start of each session, Claude loads various information: your instructions, code files, conversation history, CLAUDE.md, Skills, etc. All of this occupies context space.

**Context cost** is the amount of context space a feature occupies in each session. The higher the cost, the less space is available for actual work.

### 13.2 Context Costs of Each Mechanism

#### 13.2.1 CLAUDE.md: Always Loaded, Highest Cost

CLAUDE.md is automatically loaded at the start of every session. Regardless of what you're doing in that conversation, CLAUDE.md content is in the context.

This means:
- CLAUDE.md content occupies context space in every session
- If you open multiple sessions in a day, these tokens are consumed multiple times
- Worse, if CLAUDE.md is too long, it reduces Claude's compliance — it can't "remember" all the instructions

**Therefore**: CLAUDE.md should only contain information that's truly needed every time — build commands, core conventions, project structure. If an instruction is only needed in specific scenarios (e.g., "use a specific format when handling API routes"), it shouldn't be in CLAUDE.md.

#### 13.2.2 Skills: Loaded on Demand, Medium Cost

Skills are only loaded into context when you invoke them or Claude auto-matches them. They don't occupy space otherwise.

This means:
- If you have a code-review Skill but didn't do code review today, it doesn't occupy context
- If you invoke `/code-review`, the Skill's content is injected into context, occupying some space
- After use, the Skill's content gradually gets compressed or discarded as the conversation continues

**Therefore**: Skills are suitable for multi-step workflows and reference documentation — they don't occupy space normally, and are fully injected when needed. But note, a single Skill shouldn't be too long, or it will occupy a lot of context when loaded.

#### 13.2.3 Subagents: Independent Context, Zero Cost to Main Session

Subagents run in independent context windows. Their file reading, code analysis intermediate processes all happen in the subagent's context — the main session only receives the final summary.

This means:
- A subagent might have read 50 files, but the main session only sees a 200-word summary
- The main session's context cost is zero (except for the few hundred tokens the summary occupies)
- The subagent's context is independent and doesn't affect the main session

**Therefore**: Subagents are suitable for tasks with many intermediate artifacts — large amounts of file reading, deep analysis, parallel research. The main session only receives the final conclusions.

#### 13.2.4 Hooks: Event-Triggered, Usually Zero Cost

Hooks execute when specific events occur (e.g., before/after tool calls). Unless the Hook returns output that gets injected into context, they don't occupy context space.

This means:
- A PostToolUse Hook that automatically runs Prettier after every file edit doesn't occupy context
- If a Hook returns error messages, that information enters the context, occupying a small amount of space
- Most Hooks have zero context cost

**So**: Hooks are suitable for deterministic operations — formatting, linting, notifications. These operations must run, but Claude doesn't need to "know" they exist.

#### 13.2.5 MCP Servers: Tool Definitions Always Present, Call Results Enter Context

MCP server tool definitions are loaded into context at session start, but only when you call a tool does the result enter context.

This means:
- If you configure multiple MCP servers, the tool definitions occupy context space
- If you haven't called any MCP tools today, those definitions still occupy context
- If you call an MCP tool, the tool's return result enters context, occupying additional space

**So**: MCP is suitable for external tools you need to use frequently. If you only occasionally run a database query, it may not be worth configuring MCP — using an SQL client directly in the terminal is faster. Only when you need Claude to proactively and repeatedly call an external tool does the MCP context cost become worthwhile.

#### 13.2.6 Plugins: Combined Cost, Depends on Content

Plugins themselves don't directly occupy context, but the Skills, Agents, and MCP configurations contained within a plugin each occupy context according to their own mechanisms.

This means:
- If a plugin contains 3 Skills, those Skill definitions are registered in the system, but only loaded when called
- If a plugin contains MCP configurations, the MCP tool definitions are loaded at session start
- If a plugin contains Hooks, Hooks typically don't occupy context

**So**: A plugin's context cost is a combination of its contents. Before installing a plugin, check what it contains — if it's only Skills, the cost is low (loaded on demand); if it contains many MCP tools, the cost is higher (always present in context).

#### 13.2.7 Path-Scoped Rules: Loaded on Demand, Lower Cost

Path-scoped rules (`.claude/rules/*.md`) are only loaded when Claude processes matching files.

This means:
- If you have an `api-rules.md`, it's only loaded when Claude processes files under `src/api/`
- If you only changed frontend code today, the API rules don't occupy context
- Multiple path-scoped rules can coexist without interfering with each other

**So**: Path-scoped rules are suitable for conventions specific to certain file types — API rules don't interfere with frontend code, test rules don't interfere with production code.

### 13.3 Decision Flow

A practical decision flow:

1. **Does this rule need to apply every time?**
   - Yes → CLAUDE.md
   - No → Continue

2. **Is this a multi-step, reusable process?**
   - Yes → Skill
   - No → Continue

3. **Will this task generate a lot of intermediate information?**
   - Yes → Subagent
   - No → Continue

4. **Must this operation run without relying on Claude to remember?**
   - Yes → Hook
   - No → Continue

5. **Does Claude need to proactively call external tools (database, API, browser)?**
   - Yes → MCP Server
   - No → Continue

6. **Does this rule only apply to specific files?**
   - Yes → `.claude/rules/`
   - No → Continue

7. **Do you need to package multiple extensions and distribute them to the team?**
   - Yes → Plugin
   - No → Probably not needed

### 13.4 Common Mistakes

**Mistake 1: Stuffing all rules into CLAUDE.md**
- Consequence: CLAUDE.md exceeds 200 lines, Claude's compliance drops, context space runs out
- Solution: Move scenario-specific rules to `.claude/rules/` or Skills

**Mistake 2: Doing heavy analysis in the main session**
- Consequence: The intermediate process of reading 50 files fills up the context, leaving insufficient space for subsequent conversation
- Solution: Delegate heavy analysis to Subagents; the main session only receives summaries

**Mistake 3: Writing "remember to run lint" in CLAUDE.md**
- Consequence: Claude may forget, because CLAUDE.md is only a suggestion, not enforced
- Solution: Use a PostToolUse Hook to automatically run lint

**Mistake 4: Writing path-scoped rules for every file type**
- Consequence: Too many rules, complex management, heavy context usage when loaded
- Solution: Only write rules for file types that truly need differentiation; put the rest in CLAUDE.md

---

## Chapter 14 Practical Example: Building a Task Management API from Scratch

We've covered a lot of theory. Let's see how these features work together in actual development through a complete project example.

### 14.1 Project Background

Suppose you want to build a Task Management API from scratch, with the tech stack of Node.js + Express + TypeScript + SQLite. This is a common CRUD application, but we'll use it to demonstrate the practical application of Claude Code's various features.

Project goals:
- Support task creation, querying, updating, and deletion
- User authentication (JWT)
- Complete test coverage
- Code quality assurance (linting, formatting, security checks)

### 14.2 Overall Work Steps

Before starting to code, let's plan the entire project workflow. This project will go through nine phases, each using different Claude Code features:

| Phase | Core Task | Features Used | Why This Feature |
|-------|-----------|---------------|------------------|
| Step 1 | Initialize project and configure memory system | CLAUDE.md + Path-scoped rules | Set project tone, divert specific rules |
| Step 2 | Create API endpoints | Skills (`/create-api`) | Encapsulate reusable multi-step workflows |
| Step 3 | Code quality assurance | Hooks (PostToolUse + PreToolUse) | Deterministic execution of formatting and security checks |
| Step 4 | Debugging and refactoring | Subagents (isolated analysis) | Heavy tasks don't pollute main context |
| Step 5 | Major refactoring | Plan Mode + Model switching | Analyze first, execute later; use Opus for complex tasks |
| Step 6 | Code review | `/code-review` + Effort adjustment | Built-in process, adjustable depth |
| Step 7 | Automated deployment | CI/CD integration + GitHub Actions | Automated CI/CD, runs in the cloud |
| Step 8 | Integrate external tools | MCP Servers (database, project management) | Connect external systems, end-to-end automation |
| Step 9 | Share team workflows | Plugins (package Skills + MCP + Hooks) | Team knowledge accumulation and distribution |

This order is not arbitrary — it reflects the natural evolution of a project from "establishing foundations" to "continuous delivery." Each step builds on the previous one, and features work together synergistically. Below, we'll go through each phase in order, detailing the specific operations.

### 14.3 Step 1: Initialize Project and Configure Memory System

#### 14.3.1 Your Operations

```bash
mkdir task-api && cd task-api
npm init -y
npm install express typescript @types/express @types/node sqlite3
npm install --save-dev jest @types/jest ts-jest prettier eslint
npx tsc --init
```

After the project structure is initialized, you open Claude Code and start configuring the memory system.

#### 14.3.2 Configure CLAUDE.md

```markdown
# Task API Project

### Build and Run
- Build: `npm run build` (compile TypeScript)
- Start: `npm start` (run compiled code)
- Dev mode: `npm run dev` (hot reload with ts-node-dev)
- Test: `npm test`

### Code Conventions
- Use 2-space indentation
- All API endpoints prefixed with `/api/v1`
- Error response format: `{ error: { code: string, message: string } }`
- All async operations must have try-catch error handling
- Database operations encapsulated in `src/db/` directory; don't write SQL directly in routes

### Testing Requirements
- Each API endpoint must have a corresponding integration test
- Test files go in `tests/` directory, named `*.test.ts`
- Ensure the database is clean before tests (use beforeAll to clear tables)

### Security Rules
- Don't hardcode JWT_SECRET in code; read from environment variables
- All user input must be validated (use zod)
- Passwords must be encrypted with bcrypt, at least 10 rounds
```

Configure path-scoped rule `.claude/rules/api-rules.md`:

```markdown
---
paths:
  - "src/routes/**/*.ts"
---
# API Route Rules
- Each route file must export an Express Router
- Route naming: GET /tasks (list), POST /tasks (create), GET /tasks/:id (detail), etc.
- All routes must have corresponding validation middleware (using zod schemas)
- Error handling uniformly wrapped with asyncHandler
```

#### 14.3.3 Why Do It This Way

- **Initial configuration** contains information needed every time (build commands, core conventions)
- **Path-scoped rules** are only loaded when Claude processes route files, avoiding context pollution for other files
- **Specific, verifiable instructions** ("2-space indentation" instead of "format well")

### 14.4 Step 2: Creating API Endpoints — Skills Encapsulating Workflows

#### 14.4.1 Your Requirements

You need to create multiple API endpoints (task CRUD, user authentication), each following a similar creation process: define routes, write validation, implement logic, write tests.

```yaml
---
name: create-api
description: Create a new API endpoint, including routes, validation, logic, and tests
---
```

```markdown
# Create API Endpoint

When asked to create a new API endpoint, follow these steps:

### 1. Define Routes and Validation
Create or update a route file under `src/routes/`, including validation schemas and route handlers.

### 2. Implement Business Logic
Create a corresponding service file under `src/services/`, encapsulating database operations.

### 3. Write Integration Tests
Create a test file under `tests/`, including test cases for both normal flow and error handling.

### 4. Verify
- Run `npm test` to ensure all tests pass
- Run `npm run lint` to ensure consistent code style
- Manually test the endpoint (using curl or Postman)
```

#### 14.4.2 Using the Skill

Now you can simply say:

```
> /create-api Create an endpoint for fetching the task list, supporting pagination and filtering
```

Claude will load this Skill and execute according to your defined process: create routes, write validation, implement services, write tests. You don't need to repeatedly explain these steps every time.

#### 14.4.3 Why Do It This Way

- **Encapsulates a reusable multi-step workflow**
- Each step has specific code examples, so Claude won't deviate
- Includes verification steps to ensure the task is truly complete
- Next time you create an endpoint, just call `/create-api` — efficiency greatly improved

### 14.5 Step 3: Code Quality Assurance — Hooks Automation

#### 14.5.1 Your Requirements

You want Claude to automatically format files after every edit, and prevent it from executing dangerous commands.

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": ".claude/hooks/block-dangerous.sh"
          }
        ]
      }
    ]
  }
}
```

#### 14.5.2 Results

- Every time Claude edits a file, Prettier runs automatically, keeping code formatting consistent
- If Claude tries to execute `rm -rf` or dangerous SQL, the Hook will block it
- These are deterministic operations, not dependent on Claude "remembering" to do them

> ⚠️ **Prerequisites**: `jq` needs to be installed in advance. Common installation methods:
> - macOS: `brew install jq`
> - Linux: `apt install jq` or `yum install jq`
> - Windows: `choco install jq`
>
> It's recommended to use more robust path validation scripts in actual production environments.

#### 14.5.3 Why Do It This Way

- **Provides enforced execution** — "remember to run lint" in CLAUDE.md is only a suggestion
- `PostToolUse` triggers after tool execution, suitable for formatting and checking
- `PreToolUse` triggers before tool execution, suitable for blocking dangerous operations
- **Zero context cost**, unless the Hook returns output

### 14.6 Step 4: Debugging and Refactoring — Subagents for Isolated Analysis

#### 14.6.1 Scenario

Your application has been running for a while, and memory usage keeps growing. You need to locate memory leaks.

```
> The application's memory usage keeps growing after running for a while. Help me analyze potential memory leak points
```

#### 14.6.2 Claude's Response

Claude determines this is a task requiring deep analysis and launches a subagent to scan. You don't see the intermediate process, only the final summary.

#### 14.6.3 Subagent Returns Summary

```
Analysis complete. Found the following potential memory leak points:

1. src/services/cache.ts:15 - Global Map has no size limit, will grow indefinitely
   Suggestion: Use an LRU cache or set a maximum entry count

2. src/routes/websocket.ts:23 - WebSocket event listeners not removed when connection closes
   Suggestion: Call removeAllListeners() in the close event

3. src/db/connection.ts:8 - Database connection pool has no maximum connection count configured
   Suggestion: Set the max parameter to limit pool size
```

#### 14.6.4 Why Do It This Way

- **Isolated analysis**: Memory leak investigation requires reading many files, producing lots of intermediate artifacts
- If done in the main session, these intermediate results would fill the context window
- Subagents run in independent contexts; the main session only receives the final summary
- Subagents can use the Haiku model (faster and cheaper) since this is a pattern-matching task

### 14.7 Step 5: Major Refactoring — Plan Mode + Model Switching

#### 14.7.1 Scenario

You decide to migrate the database from SQLite to PostgreSQL. This is a major refactoring affecting all database operations.

1. Switch to Plan Mode (type `/plan`)
2. Optional: Switch model to Opus (`/model opus`) for deeper planning capability
3. Let Claude analyze the scope of impact
4. Review Claude's migration plan
5. After confirming the plan, switch back to Sonnet for execution
6. During execution, use `/code-review` to check change quality

#### 14.7.2 Why Do It This Way

- **Lets you see the plan before acting**, avoiding many unwanted changes
- Major refactoring has broad impact and requires comprehensive analysis; Plan Mode won't miss anything
- **Model switching** lets you use the strongest model where intelligence matters most (planning) and save costs during execution
- Manually switching between Opus and Sonnet gives flexible cost control

### 14.8 Step 6: Code Review — Code Review + Effort Adjustment

#### 14.8.1 Scenario

You've completed the migration and are ready to submit a PR. You want Claude to do a final code review.

```
> /code-review
```

Claude will run `git diff` to get all changes, review file by file, check type safety, error handling, and performance, verify test coverage, and finally provide tiered feedback.

#### 14.8.2 Why Do It This Way

- `/code-review` is a built-in review process, no need to explain every time
- Tiered feedback lets you quickly locate the most important issues
- Effort levels can adjust review depth
- `/effort max` is suitable for deep analysis of specific aspects

### 14.9 Step 7: Automated Deployment — CI/CD Integration

#### 14.9.1 Scenario

You want tests and deployment to run automatically after every PR merge.

#### 14.9.2 Using GitHub Actions + Claude Code

Claude Code can integrate with CI/CD systems to implement automated workflows. The recommended approach is to call Claude Code CLI through GitHub Actions:

```yaml
# .github/workflows/claude-review.yml
name: Claude Code Review
on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Claude Code
        run: npm install -g @anthropic-ai/claude-code
      - name: Run Code Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude --dangerously-skip-permissions -p "/code-review main"
```

> ⚠️ **Note**: The `npm install -g @anthropic-ai/claude-code` installation command above is for example purposes only. Please refer to the [official installation guide](https://code.claude.com/docs/) for the actual installation method.

#### 14.9.3 Typical Scenarios

- **Automated PR review**: Automatically review code quality when each PR is opened
- **Deployment verification**: Automatically trigger smoke tests after CD pipeline deployment completes
- **Documentation drift detection**: Weekly scan of merged PRs to flag outdated documentation

> 💡 **Tip**: Through CI/CD integration, you can extend Claude Code's capabilities into the automation domain. Combined with GitHub Actions or other CI tools, you can implement complex automated workflows without writing complex scripts.

### 14.10 Step 8: Integrating External Tools — MCP for Database and Project Management

After the project is basically complete, you want Claude to be able to directly query the database and manage Jira tasks, instead of switching to other tools every time.

#### 14.10.1 Configure Database MCP

```bash
# Add SQLite MCP (since the project uses SQLite)
claude mcp add sqlite -- npx -y @modelcontextprotocol/server-sqlite

# Configure database path
export SQLITE_DB_PATH=./task-api.db
```

Usage examples:

```
> Query the number of users in the users table who registered in the last 7 days
> Analyze task completion for each user in the tasks table, find the top 5 most active users
> Check if there are tasks created more than 30 days ago that haven't been updated
```

Claude will generate SQL, execute queries, and analyze results. You don't need to leave the terminal or remember the table structure — Claude will automatically read the database schema.

#### 14.10.2 Configure Jira MCP

```bash
# Add Jira MCP
claude mcp add jira -- npx -y @modelcontextprotocol/server-jira

# Configure Jira information
export JIRA_BASE_URL=https://your-team.atlassian.net
export JIRA_EMAIL=<your_email>
export JIRA_API_TOKEN=<your_api_token>
```

Usage examples:

```
> View the details of issue PROJ-123
> Create a new issue with the title "Fix CORS error on login page", assign it to me
> Change the status of PROJ-456 to "In Progress"
> List all issues marked as "Urgent"
```

This way you can manage tasks while coding, without switching to a browser. Claude will automatically call the Jira API based on your instructions.

#### 14.10.3 Why Do It This Way

- MCP transforms Claude from a "code assistant" into an "all-in-one workstation" that can operate external systems
- Database MCP lets you quickly query and analyze data without writing SQL clients
- Project management MCP lets you manage tasks while coding, improving efficiency
- But be careful — the more MCP tools you have, the higher the context cost. Only add tools you truly need

### 14.11 Step 9: Sharing Team Workflows — Plugin Packaging and Distribution

After the project is complete, you want to share this workflow (Skills + MCP + Hooks) with the team so new colleagues can get up to speed quickly.

#### 14.11.1 Create Plugin

Create the `.claude/plugins/task-api-workflow/` directory in the project root:

```
task-api-workflow/
├── plugin.json
├── skills/
│   └── create-api/
│       └── SKILL.md
├── mcp/
│   └── servers.json
└── hooks/
    └── hooks.json
```

`plugin.json`:

```json
{
  "name": "task-api-workflow",
  "version": "1.0.0",
  "description": "Task API project standard workflow",
  "author": "Your Team",
  "skills": ["skills/create-api"],
  "mcp": {
    "servers": "mcp/servers.json"
  },
  "hooks": {
    "config": "hooks/hooks.json"
  }
}
```

`mcp/servers.json`:

```json
{
  "sqlite": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-sqlite"],
    "env": {
      "SQLITE_DB_PATH": "${SQLITE_DB_PATH}"
    }
  },
  "jira": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-jira"],
    "env": {
      "JIRA_BASE_URL": "${JIRA_BASE_URL}",
      "JIRA_EMAIL": "${JIRA_EMAIL}",
      "JIRA_API_TOKEN": "${JIRA_API_TOKEN}"
    }
  }
}
```

`hooks/hooks.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "prettier --write $CLAUDE_FILE_PATH"
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "node .claude/hooks/check-dangerous-commands.js"
      }
    ]
  }
}
```

#### 14.11.2 Distribute Plugin

Push the plugin to your team's Git repository:

```bash
cd .claude/plugins
git init
git add .
git commit -m "Initial plugin release"
git remote add origin https://github.com/your-team/task-api-workflow-plugin.git
git push -u origin main
```

Team members install with:

```bash
claude plugin install https://github.com/your-team/task-api-workflow-plugin
```

#### 14.11.3 Why Do It This Way

- Plugins package Skills, MCP, and Hooks together for one-click installation
- Team members share the same workflow, reducing configuration differences
- Plugins have version numbers, enabling change tracking and rollbacks
- New colleagues can immediately use proven best practices from day one

### 14.12 Experience Review

| Phase | Features Used | Why This Feature |
|-------|---------------|------------------|
| Initialization | CLAUDE.md + Path-scoped rules | Set project tone, divert specific rules |
| Creating APIs | Skills (`/create-api`) | Encapsulate reusable multi-step workflows |
| Code quality | Hooks (PostToolUse + PreToolUse) | Deterministic execution of formatting and security checks |
| Debugging | Subagents (isolated analysis) | Heavy tasks don't pollute main context |
| Refactoring | Plan Mode + Model switching | Analyze first, execute later; use Opus for complex tasks |
| Review | `/code-review` + Effort adjustment | Built-in process, adjustable depth |
| Deployment | CI/CD integration + GitHub Actions | Automated CI/CD, runs in the cloud |
| Tool integration | MCP Servers (database, Jira) | Connect external systems, end-to-end automation |
| Team sharing | Plugins (package Skills + MCP + Hooks) | Team knowledge accumulation and distribution |

#### 14.12.1 Final Advice

Don't try to use all features at once. Start with CLAUDE.md, gradually introduce Skills and Hooks, and only try Subagents and CI/CD integration once you're comfortable. Each feature has a learning curve — gradual progression is the key to mastery.

---

## Chapter 15 Key Takeaways

### 15.1 About Memory and Context

- CLAUDE.md is not better just because it has more content. After 200 lines, Claude's compliance drops noticeably. Divert scenario-specific rules to `.claude/rules/` to keep CLAUDE.md concise and focused
- After context compression (`/compact`), important information may be lost. If you notice Claude "forgetting" instructions after compression, consider using a SessionStart hook (with matcher set to `compact`) to re-inject critical context ⚠️ Please verify the specific behavior for your current version, or alternatively write these instructions directly into the project root CLAUDE.md
- Auto-memory is Claude's notes to itself and isn't always accurate. Periodically review with `/memory` to clean up incorrect or outdated content

### 15.2 About Skills

- The most critical use of Skills is **self-contained, in-depth instructions**. When a Skill runs in a subagent, it can't see your conversation history, so all prerequisite information-gathering steps, judgment criteria, and output formats must be written in the Skill body
- **Subagent isolation** is a powerful tool for handling "heavy" tasks. Codebase audits, large-scale refactoring analysis — let them run in subagents, and the main session only receives the report. But remember — subagents can't see your conversation history, so the Skill body must be self-contained
- Use `/run` to standardize how complex projects are started. Every team has projects that "only one person knows how to start." Turn that knowledge into a Skill so everyone can use it

### 15.3 About Subagents

- **Restrict tool permissions** for subagents. A code review subagent only needs `Read, Glob, Grep` — don't give it write permissions. That way, even if it "wants" to modify code, it can't
- Use the `haiku` model for subagents on simple tasks — much cheaper than using Opus in the main session for the same work
- Subagent results are "self-reported" — it saying it's done doesn't mean it actually is. For critical operations (deployments, database changes), verify the subagent's output yourself

### 15.4 About Permissions and Workflows

- Always use **Plan Mode** before major changes. Let Claude propose a plan, confirm the approach is right, then execute. This is far more efficient than letting it act directly and then spending lots of time rolling back unwanted changes
- For operations that "must happen" (linting, formatting, security checks), use **Hooks** instead of writing "remember to run lint" in CLAUDE.md. Hooks are deterministic; CLAUDE.md is only a suggestion

### 15.5 About MCP and Plugins

**MCP Usage Tips**:

- **Principle of least privilege**: MCP servers should only expose necessary tools. For example, a filesystem MCP should restrict access directories — don't give access to the entire root. A database MCP should only have read permissions unless write operations are truly needed
- **Environment variable management**: Pass sensitive information (API tokens, passwords) via environment variables, don't hardcode them. You can use `.env` files or system environment variables. Add `.env` to `.gitignore` to prevent leaks
- **Context cost awareness**: MCP tool definitions are loaded into context at session start. If you configure 15 MCP tools, even if you only use one today, the other 14 definitions still occupy context. Only add tools you truly need
- **Error handling**: MCP servers should return clear error messages. If a tool call fails, Claude needs to know why to decide the next step. Vague error messages (like "operation failed") prevent Claude from handling things effectively
- **Performance considerations**: MCP tool calls block Claude's execution. If a tool takes a long time (e.g., large-scale data processing), consider returning a task ID and letting Claude poll for status instead of waiting indefinitely

**Plugin Usage Tips**:

- **Plugins vs. direct configuration**: If it's just for personal use, directly configuring Skills and Hooks is simpler. Plugins are suited for scenarios requiring distribution to multiple people — team collaboration, open-source projects, standardized workflows
- **Plugin content review**: Before installing a plugin, check what it contains. If it's only Skills, the cost is low (loaded on demand); if it contains many MCP tools, the cost is higher (always present in context). Don't install plugins you don't need
- **Version management**: Plugins have version numbers; be cautious when updating. Major updates may change workflow behavior — verify in a test environment first before rolling out to the team
- **Team knowledge accumulation**: Package proven best practices (code review processes, deployment steps, debugging techniques) into plugins so new colleagues can use them immediately. This is far more efficient than having everyone figure things out on their own
- **Plugin maintenance**: Plugins are not a one-time setup. As the project evolves, plugins need updating too. Assign someone to be responsible for plugin maintenance, with regular reviews and updates

### 15.6 About Costs

- Fast mode (`/fast`) is most cost-effective when enabled at session start. Enabling it mid-session requires paying the fast mode premium for the entire existing context
- Large-scale tasks consume more resources. Don't use them for daily small edits — reserve them for scenarios that truly need orchestration, like codebase audits and large-scale migrations

---

## Final Words

The power of Claude Code lies not in any single feature, but in the flexible combination of these extension layers:

- **CLAUDE.md** sets the tone
- **Skills** encapsulate processes
- **Subagents** isolate heavy work
- **Hooks** handle deterministic operations
- **MCP** extends capability boundaries
- **Plugins** package and distribute team knowledge

Understanding each layer's role and cost allows you to build development workflows that are both efficient and controllable. I hope this practical example helps you connect these features and form your own best practices.

---

## Appendix: Quick Reference Card

### Common Command Quick Reference

| Command | Purpose |
|---------|---------|
| `claude` | Launch Claude Code |
| `claude --model opus` | Launch with specified model |
| `claude --dangerously-skip-permissions -p "..."` | CI/CD headless mode |
| `/plan` | Switch to plan mode |
| `/permissions auto` | Switch to auto permissions mode |
| `/model` | Switch model |
| `/effort high\|max` | Adjust reasoning depth |
| `/compact` | Compress context |
| `/code-review` | Code review |
| `/deep-research` | Deep research |
| `/workflows` | View workflow progress |
| `/memory` | Manage auto-memory |
| `claude --continue` | Continue last session |
| `claude --resume` | Resume from session list |

### Extension Mechanism Selection Decision Tree

```
Need a rule that applies every time?
├─ Yes → CLAUDE.md
└─ No → Multi-step reusable process?
         ├─ Yes → Skill
         └─ No → Generates lots of intermediate information?
                  ├─ Yes → Subagent
                  └─ No → Must be strictly enforced?
                           ├─ Yes → Hook
                           └─ No → Need to call external tools?
                                    ├─ Yes → MCP
                                    └─ No → Only applies to specific files?
                                             ├─ Yes → .claude/rules/
                                             └─ No → Need to package and distribute?
                                                      ├─ Yes → Plugin
                                                      └─ No → No extension needed
```

### Context Cost Comparison

| Mechanism | Loading Timing | Cost Level |
|-----------|----------------|------------|
| CLAUDE.md | Auto-loaded every session | High |
| Skills | Loaded on demand when called | Medium |
| Subagents | Independent context | Zero (for main session) |
| Hooks | Event-triggered | Usually zero |
| MCP | Tool definitions always present | Medium |
| Plugins | Combined cost | Depends on content |
| Path-scoped rules | Loaded when matched | Low |
