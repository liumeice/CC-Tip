# Claude Code 使用指南

> 📖 官方文档：https://code.claude.com/docs/zh-CN/
> 📋 基于 Claude Code v2.x 编写，最后更新：2026/06/17
> 文中功能可能与最新版本存在差异，请以 [官方文档](https://code.claude.com/docs/zh-CN/) 为准。

本指南面向已上手的开发者，按功能模块展开，融入实战经验。

---

## 目录

- [第1章 理解 Claude Code 的工作方式](#第1章-理解-claude-code-的工作方式)
- [第2章 记忆系统——让 Claude 记住你的项目](#第2章-记忆系统让-claude-记住你的项目)
  - [2.1 CLAUDE.md：你写给 Claude 的说明书](#21-claudemd你写给-claude-的说明书)
  - [2.2 自动记忆：Claude 自己做的笔记](#22-自动记忆claude-自己做的笔记)
- [第3章 权限模式——控制 Claude 的自主程度](#第3章-权限模式控制-claude-的自主程度)
  - [3.1 权限模式，从最严到最松](#31-权限模式从最严到最松)
  - [3.2 受保护路径](#32-受保护路径)
- [第4章 Skills 技能系统——Claude Code 的超级武器](#第4章-skills-技能系统claude-code-的超级武器)
  - [4.1 为什么 Skills 重要](#41-为什么-skills-重要)
  - [4.2 创建你的第一个 Skill](#42-创建你的第一个-skill)
  - [4.3 触发方式](#43-触发方式)
  - [4.4 高级特性一：自包含的深度指令](#44-高级特性一自包含的深度指令)
  - [4.5 高级特性二：在子代理中隔离执行](#45-高级特性二在子代理中隔离执行)
  - [4.6 高级特性三：深度推理信号](#46-高级特性三深度推理信号)
  - [4.7 高级特性四：捆绑 Skills 联动](#47-高级特性四捆绑-skills-联动)
  - [4.8 Skills 的实战建议](#48-skills-的实战建议)
- [第5章 Subagents 子代理——独立的 AI 工作者](#第5章-subagents-子代理独立的-ai-工作者)
  - [5.1 内置子代理](#51-内置子代理)
  - [5.2 创建自定义子代理](#52-创建自定义子代理)
- [第6章 Hooks——确定性的自动化](#第6章-hooks确定性的自动化)
  - [6.1 什么时候用 Hooks](#61-什么时候用-hooks)
  - [6.2 配置示例](#62-配置示例)
  - [6.3 Hook 类型](#63-hook-类型)
- [第7章 MCP 服务器——扩展 Claude 的能力边界](#第7章-mcp-服务器扩展-claude-的能力边界)
  - [7.1 核心概念](#71-核心概念)
  - [7.2 添加和管理 MCP 服务器](#72-添加和管理-mcp-服务器)
  - [7.3 MCP 与 Hooks 的区别](#73-mcp-与-hooks-的区别)
  - [7.4 实战场景](#74-实战场景)
  - [7.5 最佳实践](#75-最佳实践)
- [第8章 插件系统——共享和分发扩展](#第8章-插件系统共享和分发扩展)
  - [8.1 插件是什么](#81-插件是什么)
  - [8.2 安装和管理插件](#82-安装和管理插件)
  - [8.3 创建自己的插件](#83-创建自己的插件)
  - [8.4 分发插件](#84-分发插件)
  - [8.5 插件 vs 直接配置](#85-插件-vs-直接配置)
- [第9章 模型配置与工作量调节](#第9章-模型配置与工作量调节)
  - [9.1 选择合适的模型](#91-选择合适的模型)
  - [9.2 工作量级别](#92-工作量级别)
  - [9.3 快速模式](#93-快速模式)
- [第10章 常见工作流与最佳实践](#第10章-常见工作流与最佳实践)
  - [10.1 代码库探索：快速理解陌生项目](#101-代码库探索快速理解陌生项目)
  - [10.2 Bug 修复：从症状到根因](#102-bug-修复从症状到根因)
  - [10.3 重构：安全地改进代码结构](#103-重构安全地改进代码结构)
  - [10.4 测试编写：生成高质量的测试用例](#104-测试编写生成高质量的测试用例)
  - [10.5 会话管理：跨会话保持连续性](#105-会话管理跨会话保持连续性)
  - [10.6 并行工作：用 git worktrees 同时处理多个任务](#106-并行工作用-git-worktrees-同时处理多个任务)
- [第11章 动态工作流——编排大规模任务](#第11章-动态工作流编排大规模任务)
  - [11.1 核心思想：把计划移入代码](#111-核心思想把计划移入代码)
  - [11.2 何时使用工作流](#112-何时使用工作流)
  - [11.3 使用方式](#113-使用方式)
  - [11.4 工作流的质量模式](#114-工作流的质量模式)
- [第12章 Code Review——自动化代码审查](#第12章-code-review自动化代码审查)
  - [12.1 审查机制](#121-审查机制)
  - [12.2 审查结果](#122-审查结果)
  - [12.3 自定义审查行为](#123-自定义审查行为)
  - [12.4 本地使用](#124-本地使用)
  - [12.5 GitHub App 集成](#125-github-app-集成)
- [第13章 理解上下文成本——选对扩展机制](#第13章-理解上下文成本选对扩展机制)
  - [13.1 上下文成本是什么](#131-上下文成本是什么)
  - [13.2 各机制的上下文成本](#132-各机制的上下文成本)
  - [13.3 决策流程](#133-决策流程)
  - [13.4 常见错误](#134-常见错误)
- [第14章 实战案例：从零搭建任务管理 API](#第14章-实战案例从零搭建任务管理-api)
  - [14.1 项目背景](#141-项目背景)
  - [14.2 总体工作步骤](#142-总体工作步骤)
  - [14.3 第一步：初始化项目与配置记忆系统](#143-第一步初始化项目与配置记忆系统)
  - [14.4 第二步：创建 API 端点](#144-第二步创建-api-端点skills-封装工作流)
  - [14.5 第三步：代码质量保障](#145-第三步代码质量保障hooks-自动化)
  - [14.6 第四步：调试和重构](#146-第四步调试和重构subagents-隔离分析)
  - [14.7 第五步：重大重构](#147-第五步重大重构plan-mode--模型切换)
  - [14.8 第六步：代码审查](#148-第六步代码审查code-review--模型调节)
  - [14.9 第七步：自动化部署](#149-第七步自动化部署cicd-集成)
  - [14.10 第八步：集成外部工具](#1410-第八步集成外部工具mcp-连接数据库和项目管理)
  - [14.11 第九步：团队工作流共享](#1411-第九步团队工作流共享插件打包和分发)
  - [14.12 经验回顾](#1412-经验回顾)
- [第15章 关键经验总结](#第15章-关键经验总结)

---

## 第1章 理解 Claude Code 的工作方式

在深入各功能之前，有必要先理解 Claude Code 的底层运行机制。

Claude Code 本质上是一个**代理循环**：它读取上下文（你的指令、代码文件、对话历史），决定调用哪个工具（读文件、写文件、跑命令、搜索代码），检查结果，然后继续下一步——如此往复，直到任务完成。

这意味着 Claude Code 不是一个"你问它答"的聊天机器人，而是一个**有手有脚的 agent**。它能操作你的文件系统、执行终端命令、搜索整个代码库。理解这一点，才能理解后面所有功能的设计意图——它们都是在不同层面控制和增强这个代理循环。

内置工具大致分几类：
- **文件操作**：Read / Write / Edit
- **搜索**：Grep / Glob
- **终端执行**：Bash
- **网络访问**：WebFetch
- **代码智能**：LSP 跳转定义、查找引用等

Claude 会根据任务需要自动选择合适的工具组合。

> 💡 **经验**：你可以用自然语言引导 Claude 的工具选择。比如说"先搜索一下哪些文件引用了这个函数"，它会优先用 Grep 而不是直接 Read 整个目录。给出清晰的步骤指引，比笼统地说"帮我改一下代码"效果好得多。

---

## 第2章 记忆系统——让 Claude 记住你的项目

每次新会话，Claude 都是从零开始的。它不记得上次你让它修了什么 bug，也不记得你说过"用 pnpm 不用 npm"。记忆系统就是为了解决这个问题。

### 2.1 CLAUDE.md：你写给 Claude 的说明书

CLAUDE.md 是你主动编写的指令文件，Claude 在每个会话开始时自动读取。你可以把它理解为"每次开工前都要交代一遍的话"——如果有些话你发现自己每次都要说，那就该写进 CLAUDE.md。

它有几个层级，从广到窄依次生效：

| 层级 | 路径 | 用途 |
|------|------|------|
| 用户级 | `~/.claude/CLAUDE.md` | 你个人的偏好，所有项目通用 |
| 项目级 | `./CLAUDE.md` | 团队共享的约定，提交到 git |
| 本地级 | `./CLAUDE.local.md` | 你个人在这个项目里的偏好，加到 `.gitignore` |

还有一个容易忽略的机制：`.claude/rules/` 目录。你可以把规则拆分成多个文件，并且按文件路径限定生效范围。比如：

```markdown
# .claude/rules/api-rules.md
---
paths:
  - "src/api/**/*.ts"
---
所有 API 端点必须包含输入验证。
错误响应使用统一格式 { code, message, details }。
```

这条规则只在 Claude 处理 `src/api/` 下的 TypeScript 文件时才会加载。这非常实用——你的前端规则不会干扰后端代码，反之亦然，同时节省了上下文空间。

#### 2.1.1 写好 CLAUDE.md 的关键原则

写 CLAUDE.md 不是越多越好。它本质上是在消耗每次会话的上下文窗口，写太多反而会降低 Claude 的遵守度。几个要点：

1. **控制在 200 行以内**。超出的话，考虑把特定场景的规则移到 `.claude/rules/` 里按需加载。
2. **具体到可以验证**。"使用 2 空格缩进"比"格式化好代码"有效得多；"提交前运行 `npm test`"比"记得测试"有效得多。
3. **避免矛盾**。如果项目级说"用 ESLint"，用户级说"用 Prettier"，Claude 会随机选一个。定期审查，清理冲突。
4. **用 `@path/to/file` 导入外部文件**。比如 `@README.md` 或 `@package.json`，Claude 会在启动时展开它们。但注意，导入的内容同样占用上下文。

### 2.2 自动记忆：Claude 自己做的笔记

除了你写的 CLAUDE.md，Claude 还会自己给自己记笔记。当你纠正它（"这个项目用 Bun 不用 npm"）、或者它自己发现了有用的信息（"测试需要先启动本地 Redis"），它会自动保存。

> ⚠️ **注意**：自动记忆的具体存储位置和索引机制可能随版本更新而变化。当前版本中，记忆文件存储在 `~/.claude/projects/<project>/memory/` 目录下，索引文件（`MEMORY.md`）会在每次会话加载。

自动记忆的索引文件（`MEMORY.md`）会在每次会话加载，提供所有记忆条目的概览。详细的记忆内容存储在独立的 Markdown 文件中，Claude 在需要时根据语义相关性检索加载。这种设计避免了把所有记忆一次性塞进上下文，同时保证了关键信息随时可用。

> 💡 **经验**：用 `/memory` 命令可以随时查看和编辑这些记忆。它们就是普通的 Markdown 文件，你完全可以手动清理不准确的内容。如果发现 Claude 反复犯同一个错误，与其每次纠正，不如直接说"把这个记下来"或者手动加到 CLAUDE.md 里。

---

## 第3章 权限模式——控制 Claude 的自主程度

Claude Code 能读写文件、执行命令，这意味着它有可能做出你不想要的操作。权限模式就是你对它的"信任度调节器"。

### 3.1 权限模式，从最严到最松

| 模式 | 命令 | 特点 | 适用场景 |
|------|------|------|----------|
| **Default** | `/permissions default` | 每次编辑或运行命令都会问你 | 刚开始使用 |
| **Accept Edits** | `/permissions accept-edits` | 自由编辑文件，但运行命令需批准 | 在编辑器里实时审查代码变更 |
| **Plan** | `/plan` | 只能读文件和分析，不能修改 | 重大重构或修复杂 bug 前先出方案 |
| **Auto** | `/permissions auto` | 完全自主，但有安全分类器后台审查 | 信任任务方向，不想被反复打断 |

> 💡 **经验**：做重大重构或修复杂 bug 时，先进 Plan 模式让 Claude 出方案，你确认思路没问题再切回去执行。这比让它直接动手然后一堆不想要的改动要高效得多。

在 CLI 中按 `m` 键可以快速切换模式。

#### 3.1.1 CI/CD 无头模式

在 CI/CD 管道或容器中运行时，使用命令行标志跳过权限确认：

```bash
claude --dangerously-skip-permissions -p "运行测试并报告结果"
```

⚠️ **注意**：此标志会跳过所有权限检查，仅在受控环境（如 CI/CD 容器）中使用，不要在本地开发环境使用。

### 3.2 受保护路径

无论哪种模式（除 CI/CD 的 `--dangerously-skip-permissions` 模式外），对 `.git/`、`.vscode/`、`.claude/` 等目录的写入都需要确认。这是防止 Claude 意外破坏仓库状态或自身配置的安全网。

---

## 第4章 Skills 技能系统——Claude Code 的超级武器

Skills 是 Claude Code 中最强大也最被低估的功能。简单来说，Skill 就是一个**可复用的工作流包**——你把某个任务的步骤、注意事项、甚至实时数据查询打包在一起，Claude 在需要时加载它，按照你定义的流程执行。

📖 详见官方文档：Skills

### 4.1 为什么 Skills 重要

想象一下这些场景：
- 你每次让 Claude 做代码审查，都要重复说"先检查类型安全，再看错误处理，最后看性能"
- 你有一个多步部署流程，每次都要从头解释
- 你想让 Claude 在分析代码前先跑几个命令获取当前状态

这些重复的、多步骤的、需要特定上下文的工作流，就是 Skills 要解决的问题。

### 4.2 创建你的第一个 Skill

Skill 本质上就是一个包含 `SKILL.md` 文件的目录，放在 `.claude/skills/` 下：

```
.claude/skills/
├── code-review/
│   └── SKILL.md
├── deploy-staging/
│   └── SKILL.md
└── debug-test/
    └── SKILL.md
```

`SKILL.md` 由两部分组成：YAML frontmatter（元数据）和 Markdown 正文（具体指令）。

```yaml
---
name: code-review
description: 审查代码变更的质量、安全性和性能
---
```

```markdown
# 代码审查流程

当被要求审查代码时，按以下步骤进行：

1. 先读取变更的文件列表（用 git diff）
2. 逐个文件审查，关注：
   - 类型安全和边界条件
   - 错误处理是否完整
   - 是否有性能隐患（N+1 查询、不必要的循环等）
3. 给出分级反馈：
   - 🔴 必须修复：会导致 bug 或安全问题
   - 🟡 建议改进：代码质量可以提升
   - 🟢 可选优化：锦上添花
```

### 4.3 触发方式

Skill 有两种触发方式：

#### 4.3.1 显式调用

在 Claude Code 中输入 `/code-review`，Claude 会加载这个 Skill 并按照里面的流程执行。

#### 4.3.2 自动触发

Claude 也会根据任务上下文自动匹配并加载相关 Skill。例如，当你说"帮我审查一下这个 PR"时，Claude 可能会自动加载 code-review Skill。

> 💡 **经验**：Skill 的 `description` 字段写得越准确，自动匹配的命中率越高。

### 4.4 高级特性一：自包含的深度指令

当 Skill 需要在子代理（fork 模式）中运行时，子代理看不到主对话的上下文，因此 Skill 正文必须**自包含**——把所有需要的指令、上下文数据、判断标准都写清楚。

```yaml
---
name: architecture-audit
description: 分析项目架构，识别耦合问题和改进方向
---
```

```markdown
分析这个项目的整体架构：

### 前置信息收集
先执行以下命令获取项目状态：
1. `find src -type d -maxdepth 2` — 获取目录结构
2. `cat package.json` — 获取依赖和技术栈
3. `git log --oneline -20` — 了解最近的活动

### 分析步骤
1. 扫描目录结构，识别模块边界
2. 分析模块间的依赖关系（用 Grep 搜索 import 语句）
3. 找出高耦合、低内聚的区域
4. 给出具体的改进建议

### 输出格式
返回结构化的分析报告，包含：
- 模块清单及其职责
- 依赖关系图（文本格式）
- 风险评级（高/中/低）
- 具体改进建议
```

#### 4.4.1 关键设计决策

- 把"前置信息收集"明确写在 Skill 里，让子代理自己去执行，而不是假设它知道当前状态
- 定义输出格式，确保结果结构化、可操作
- 每个步骤都有具体的执行方法（用什么工具、怎么搜索），避免模糊指令

#### 4.4.2 适用场景

- 需要当前环境信息的工作流（git 状态、环境变量、依赖版本等）
- 需要根据实时数据做决策的流程
- 调试流程（先自动收集日志、错误信息等）

### 4.5 高级特性二：在子代理中隔离执行

当一个 Skill 需要读取大量文件、做深度分析时，它的中间过程会占满你的主对话上下文。解决方案是让 Claude 在**子代理**中执行这个 Skill。

子代理在独立的上下文窗口中运行，不继承主对话的历史。它完成后只把最终结果返回给你的主会话。

#### 4.5.1 注意事项

- 子代理返回的结果是自报告，对于重要决策，建议你自己验证关键发现
- 关于子代理的自包含要求，详见 [5.2 创建自定义子代理](#52-创建自定义子代理)

### 4.6 高级特性三：深度推理信号

对于特别复杂的任务，你可以通过调节 effort 级别或使用特定的提示词信号，引导模型进行更深入的推理：

```
> /effort max
> 这是一个复杂的架构重构任务。在开始之前：
> 1. 完整分析所有涉及的模块和它们的依赖关系
> 2. 识别关键路径和风险点
> 3. 制定分步执行计划，确保每一步之后代码都是可运行的
> ...
```

`/effort` 级别控制 Claude 的推理深度：

| 级别 | 适用场景 |
|------|----------|
| `low` | 简单直接的任务 |
| `medium` | 常规编码 |
| `high`（默认） | 大多数编码任务 |
| `xhigh` | 复杂问题需要深入分析 |
| `max` | 最深入的推理，但可能过度思考，收益递减 |

> ⚠️ **注意**：社区中流传的 `ultrathink` 关键字并非官方文档中记载的特性，它更像是用户在 prompt 中使用的心理暗示信号。官方支持的深度推理控制方式是 `/effort` 命令。

### 4.7 高级特性四：捆绑 Skills 联动

Claude Code 内置了几个相互联动的 Skills：

- `/run`：启动你的应用。如果项目启动方式复杂（需要特定的环境变量、多步构建、依赖服务），`/run` 会自动捕获启动配方并生成专属的 run skill
- `/verify`：在应用运行状态下验证代码变更——不仅仅是跑测试，而是真正检查运行时的行为

> 💡 **经验**：很多项目都有"只有老手才知道怎么启动"的问题。用 `/run` 把这个知识固化下来，新人（包括 Claude）都能一键启动。

### 4.8 Skills 的实战建议

1. **从你的重复工作开始**：你发现自己第三次向 Claude 解释同一个流程时，就该把它做成 Skill
2. **保持单一职责**：一个 Skill 做一件事。不要写一个"万能 Skill"试图覆盖所有场景
3. **步骤要具体**："`npm test -- --coverage`"比"跑测试"好得多
4. **包含验证步骤**：告诉 Claude 怎么确认任务完成了——"所有测试通过且覆盖率不低于 80%"
5. **记录踩过的坑**：在 Skill 里写上"注意：这个 API 在并发超过 100 时会超时"，Claude 就不会再犯同样的错
6. **重型分析用子代理隔离**：代码库审计、大规模搜索这类任务，隔离执行不会污染你的主对话

---

## 第5章 Subagents 子代理——独立的 AI 工作者

子代理是在独立上下文窗口中运行的 AI 助手。当某个辅助任务会产生大量中间信息（搜索结果、日志分析、文件内容），而你不会再用到这些细节时，交给子代理——它只返回摘要。

📖 详见官方文档：Sub-agents

### 5.1 内置子代理

Claude Code 自带几个子代理，会自动使用：

| 子代理 | 模型 | 用途 |
|--------|------|------|
| **Explore** | Haiku（快速、便宜） | 只做只读操作，适合搜索和代码探索 |
| **Plan** | 继承主会话模型 | 在 Plan Mode 下收集上下文，为制定计划提供依据 |
| **General-purpose** | 继承主会话模型 | 能读能写能执行，处理复杂的多步骤任务 |

### 5.2 创建自定义子代理

子代理也是 Markdown 文件，放在 `.claude/agents/` 或 `~/.claude/agents/` 下：

```yaml
---
name: test-runner
description: 运行测试并分析失败原因
tools: Bash, Read, Grep
model: sonnet
---
```

```markdown
你是测试分析专家。当被要求运行测试时：
1. 执行项目的测试命令
2. 如果有失败的测试，分析失败原因
3. 区分"真正的 bug"和"测试本身需要更新"
4. 给出具体的修复建议
```

#### 5.2.1 关键配置字段解读

| 字段 | 说明 |
|------|------|
| `tools` | 限制子代理能用的工具。审查类任务只给 `Read, Glob, Grep` 就够了，不给写权限 |
| `model` | 可以给子代理指定不同的模型。简单任务用 `haiku`（快且便宜），复杂分析用 `opus` |
| `isolation: "worktree"` | 让子代理在 git worktree（隔离的仓库副本）中运行，适合需要实际修改文件但不想影响主工作区的场景 |

> ⚠️ **注意**：
> - `isolation` 字段的支持情况请确认当前版本
> - 早期文档中提到的 `context: fork` 字段并非子代理定义的标准配置

> 💡 **经验**：子代理最大的价值不只是"隔离"，还有**成本控制**。一个用 Haiku 模型跑代码搜索的子代理，成本远低于在主会话中用 Opus 做同样的事。把探索性的、中间产物多的任务交给子代理，主会话保持干净。

---

## 第6章 Hooks——确定性的自动化

如果说 Skills 是"教 Claude 怎么做"，那 Hooks 就是"不管 Claude 怎么想，这件事一定会发生"。Hooks 是在 Claude Code 生命周期特定节点自动执行的命令，提供的是**确定性控制**。

📖 详见官方文档：Hooks

### 6.1 什么时候用 Hooks

| 场景 | Hook 类型 |
|------|-----------|
| Claude 每次编辑完文件，你都想自动跑格式化工具 | `PostToolUse` |
| 你想阻止 Claude 执行某些危险命令 | `PreToolUse` |
| 你想在 Claude 完成工作后收到桌面通知 | `Notification`（⚠️ 该 Hook 类型可能随版本更新，请确认当前版本支持） |
| 上下文压缩后你想重新注入关键信息 | `SessionStart`（matcher 为 `compact`） |

### 6.2 配置示例

Hooks 在 `.claude/settings.json` 中配置。比如，每次 Claude 编辑文件后自动用 Prettier 格式化：

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

> ⚠️ **前提条件**：这个 Hook 需要系统已安装 `jq` 工具。如果 `file_path` 解析失败，命令可能格式化错误的文件。更安全的做法是在脚本中加入路径验证。

再比如，阻止 Claude 执行 `rm -rf` 命令：

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

脚本从 stdin 读取 JSON 格式的工具调用信息，检查命令内容，如果包含 `rm -rf` 就以退出码 2 退出（表示阻止）。

### 6.3 Hook 类型

除了最常见的 `command`（Shell 命令），还支持：

| 类型 | 说明 |
|------|------|
| `command` | Shell 命令 |
| `http` | POST 到指定 URL，适合集成外部服务 |
| `mcp_tool` | 调用 MCP 服务器上的工具 |
| `prompt` | 让模型做单轮判断（比如"这个操作是否安全"） |
| `agent` | 让子代理做多轮验证 ⚠️ 该功能可能在未来版本中变更 |

> 💡 **经验**：Hooks 和 CLAUDE.md 的区别很重要。CLAUDE.md 是"请求" Claude 做某事（它可能会忘），Hooks 是**强制执行**（不管 Claude 怎么想都会跑）。对于 lint、格式化、安全检查这类必须执行的操作，用 Hooks 而不是在 CLAUDE.md 里写"记得跑 lint"。

---

## 第7章 MCP 服务器——扩展 Claude 的能力边界

MCP（Model Context Protocol）是 Claude Code 扩展生态的核心机制。通过 MCP，你可以为 Claude 添加任意外部工具的能力——从数据库查询到浏览器自动化，从 Jira 集成到自定义 API 调用。

📖 详见官方文档：MCP Servers

### 7.1 核心概念

MCP 的本质是让 Claude 能够调用外部工具。你可以把它理解为给 Claude 装上了"插件"——每个 MCP 服务器提供一组工具，Claude 可以根据任务需要主动调用这些工具。

与 Hooks 不同，MCP 是**双向交互**的：Claude 决定何时调用、调用什么参数，MCP 服务器执行后返回结果。而 Hooks 是事件驱动的，在特定事件发生时自动执行，Claude 不参与决策。

### 7.2 添加和管理 MCP 服务器

```bash
# 添加一个文件系统 MCP 服务器（允许访问指定目录）
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem /path/to/allowed/dir

# 添加一个 GitHub MCP 服务器（需要配置 token）
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# 添加一个自定义 URL 的 MCP 服务器
claude mcp add my-api --url https://example.com/mcp

# 列出所有已配置的 MCP 服务器
claude mcp list

# 删除一个 MCP 服务器
claude mcp remove filesystem
```

**配置作用域**：
- `--scope user`：全局配置，对所有项目生效（默认）
- `--scope project`：项目级配置，只对当前项目生效，配置保存在 `.claude/settings.json`

> 💡 **经验**：敏感信息（如 API token）应该用环境变量传递，而不是硬编码在配置里。大多数 MCP 服务器支持从环境变量读取配置。

### 7.3 MCP 与 Hooks 的区别

| 维度 | MCP 服务器 | Hooks |
|------|-----------|-------|
| 触发方式 | Claude 主动调用 | 事件自动触发 |
| 用途 | 提供新工具/能力 | 自动化既定操作 |
| 交互性 | 双向交互，Claude 决定何时用 | 单向执行，到点就跑 |
| 上下文 | 结果返回给 Claude，进入上下文 | 通常不进入上下文 |
| 例子 | 数据库查询、API 调用、浏览器操作 | 自动格式化、阻止危险命令、通知 |

**何时用 MCP，何时用 Hooks**：
- **用 MCP**：当你需要 Claude 主动决定何时调用、调用什么参数时
- **用 Hooks**：当某个操作必须执行、不需要 Claude 决策时

### 7.4 实战场景

#### 7.4.1 场景 1：数据库查询

通过 PostgreSQL/MySQL MCP，Claude 可以直接查询数据库，帮你调试 SQL、分析数据：

```bash
# 添加 PostgreSQL MCP
claude mcp add postgres -- npx -y @modelcontextprotocol/server-postgres

# 配置连接信息（通过环境变量）
export POSTGRES_HOST=localhost
export POSTGRES_PORT=5432
export POSTGRES_USER=<your_user>
export POSTGRES_PASSWORD=<your_password>
export POSTGRES_DB=<your_db>
```

> ⚠️ **注意**：MCP 生态快速变化，具体 server 包名可能更新。请前往 [github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) 确认最新可用包名。

使用示例：

```
> 查询 users 表中注册时间在最近 7 天的用户数量
> 分析 orders 表中每个用户的平均订单金额，找出 top 10
```

Claude 会生成 SQL、执行查询、分析结果，整个过程你不需要离开终端。

#### 7.4.2 场景 2：浏览器自动化

通过 Playwright MCP，Claude 可以直接操作浏览器，测试 Web 应用的实际运行状态：

```bash
claude mcp add playwright -- npx -y @modelcontextprotocol/server-playwright
```

使用示例：

```
> 打开 http://localhost:3000，点击登录按钮，输入用户名密码，验证是否跳转到首页
> 截图当前页面，检查是否有控制台错误
```

这对于端到端测试、视觉回归测试特别有用。Claude 可以模拟用户操作，验证应用的实际行为。

#### 7.4.3 场景 3：项目管理集成

通过 Jira/Linear MCP，Claude 可以读取 issue、更新状态、创建任务，把代码工作与项目管理无缝连接：

```bash
# 添加 Jira MCP
claude mcp add jira -- npx -y @modelcontextprotocol/server-jira

# 配置 Jira 信息
export JIRA_BASE_URL=https://your-domain.atlassian.net
export JIRA_EMAIL=<your_email>
export JIRA_API_TOKEN=<your_api_token>
```

> ⚠️ **注意**：MCP 生态快速变化，具体 server 包名可能更新。请前往 [github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) 确认最新可用包名。

使用示例：

```
> 查看 PROJ-123 这个 issue 的详细信息
> 创建一个新 issue，标题是"修复登录页面的 CORS 错误"，分配给我
> 把 PROJ-456 的状态改为"进行中"
```

这样你可以在编码的同时管理任务，不需要切换到浏览器。

#### 7.4.4 场景 4：自定义 API 集成

如果你的团队有内部 API（如部署系统、监控系统、配置中心），可以开发自定义 MCP 服务器：

```python
# my-mcp-server.py
from mcp.server import Server
from mcp.types import Tool

server = Server("my-api")

@server.tool("deploy")
async def deploy(service: str, environment: str):
    """部署服务到指定环境"""
    # 调用内部部署 API
    response = await http_post(f"https://deploy-api.internal/deploy", {
        "service": service,
        "environment": environment
    })
    return {"status": response.status, "message": response.text}

@server.tool("get_metrics")
async def get_metrics(service: str):
    """获取服务的监控指标"""
    # 调用监控 API
    metrics = await http_get(f"https://metrics-api.internal/{service}")
    return metrics
```

然后在 Claude Code 中添加：

```bash
claude mcp add my-api -- python my-mcp-server.py
```

使用示例：

```
> 部署 user-service 到 staging 环境
> 查看 user-service 的 CPU 和内存使用率
```

### 7.5 最佳实践

1. **最小权限原则**：MCP 服务器应该只暴露必要的工具。比如文件系统 MCP 应该限制访问目录，不要给整个根目录
2. **环境变量管理**：敏感信息（API token、密码）用环境变量传递，不要硬编码。可以用 `.env` 文件或系统环境变量
3. **错误处理**：MCP 服务器应该返回清晰的错误信息。如果工具调用失败，Claude 需要知道原因才能决定下一步
4. **性能考虑**：MCP 工具调用会阻塞 Claude 的执行。如果某个工具执行时间很长（如大规模数据处理），考虑返回任务 ID，让 Claude 轮询状态
5. **本地开发**：开发自定义 MCP 服务器时，先用 `mcp dev` 命令测试，确保工具定义和实现正确，再集成到 Claude Code

> 💡 **经验**：MCP 是让 Claude Code 从"代码助手"变成"全能工作台"的关键。通过 MCP 连接数据库、浏览器、项目管理工具，Claude 可以直接操作外部系统，真正实现端到端的自动化。但要注意——MCP 工具越多，Claude 的选择空间越大，决策时间可能越长。只添加真正需要的工具。

---

## 第8章 插件系统——共享和分发扩展

> ⚠️ **重要提示**：本章内容基于早期/非官方信息编写，部分命令和格式可能与当前官方实现不一致。Claude Code 的插件系统仍在快速演进中，请以 [官方文档](https://code.claude.com/docs/zh-CN/plugins) 为准。

Claude Code 的插件系统允许你安装、共享和分发包含 Skills、Agents 和 MCP 配置的扩展包。这是团队协作和社区分享的核心机制。

📖 详见官方文档：Plugins

### 8.1 插件是什么

一个插件是一个打包好的扩展，可以包含：
- **Skills**：可复用的工作流指令
- **Agents**：自定义子代理定义
- **MCP 服务器配置**：预配置的外部工具连接
- **Hooks**：事件驱动的自动化操作

插件的本质是把多个扩展机制打包在一起，方便分发和安装。你可以把它理解为"功能包"——安装一个插件，就获得了一整套工作流。

### 8.2 安装和管理插件

```bash
# 安装插件（从官方仓库）
claude plugin install <plugin-name>

# 安装插件（从本地路径）
claude plugin install ./my-plugin

# 安装插件（从 Git 仓库）
claude plugin install https://github.com/user/plugin-repo

# 列出已安装的插件
claude plugin list

# 卸载插件
claude plugin uninstall <plugin-name>

# 更新插件
claude plugin update <plugin-name>
```

安装后，插件中的 Skills 会自动注册为 `/skill-name` 命令，Agents 会自动出现在可用子代理列表中，MCP 服务器会自动配置，Hooks 会自动注册。

### 8.3 创建自己的插件

插件的目录结构：

```
my-plugin/
├── plugin.json          # 插件元数据
├── skills/              # Skills 目录
│   ├── code-review/
│   │   └── SKILL.md
│   └── deploy/
│       └── SKILL.md
├── agents/              # Agents 目录
│   ├── researcher.json
│   └── tester.json
├── mcp/                 # MCP 配置
│   └── servers.json
└── hooks/               # Hooks 配置
    └── hooks.json
```

#### 8.3.1 plugin.json 示例

```json
{
  "name": "my-team-workflow",
  "version": "1.0.0",
  "description": "团队标准工作流插件",
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

#### 8.3.2 Skills 定义

在 `skills/code-review/SKILL.md` 中：

```yaml
---
name: code-review
description: 团队标准代码审查流程
---
```

```markdown
# 代码审查

### 审查步骤
1. 检查类型安全和边界条件
2. 检查错误处理是否完整
3. 检查性能隐患
4. 检查安全漏洞

### 输出格式
- 🔴 重要：合并前必须修复
- 🟡 小问题：建议改进
- 🟣 预先存在：不是本次变更引入的
```

#### 8.3.3 Agents 定义

在 `agents/researcher.json` 中：

```json
{
  "name": "researcher",
  "description": "代码库研究专家",
  "model": "sonnet",
  "tools": ["Read", "Glob", "Grep"],
  "instructions": "你是一个代码库研究专家。你的任务是理解代码结构、追踪执行流程、分析依赖关系。只返回事实，不做修改。"
}
```

#### 8.3.4 MCP 配置

在 `mcp/servers.json` 中：

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

#### 8.3.5 Hooks 配置

在 `hooks/hooks.json` 中：

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

### 8.4 分发插件

插件可以通过以下方式分发：

1. **本地路径**：`claude plugin install ./my-plugin`
2. **Git 仓库**：`claude plugin install https://github.com/user/plugin-repo`
3. **官方仓库**：提交 PR 到 Claude Code 官方插件仓库（需要审核）

**团队协作场景**：

把插件放在团队的私有 Git 仓库，团队成员安装后就能共享同一套工作流：

```bash
# 团队成员安装
claude plugin install https://github.com/your-team/claude-plugin

# 插件更新后，团队成员更新
claude plugin update your-team-plugin
```

### 8.5 插件 vs 直接配置

| 维度 | 插件 | 直接配置 |
|------|------|----------|
| 适用场景 | 需要分发给多人 | 个人使用 |
| 更新方式 | `plugin update` | 手动修改文件 |
| 版本管理 | 有版本号 | 无 |
| 依赖管理 | ⚠️ 请确认当前版本 | 手动处理 |

> 💡 **经验**：插件是团队知识沉淀的最佳载体。把你验证过的最佳实践（代码审查流程、部署步骤、调试套路）打包成插件，新同事入职就能直接用上。但要注意——插件不是万能的，如果只是个人使用，直接配置 Skills 和 Hooks 更简单。

---

## 第9章 模型配置与工作量调节

### 9.1 选择合适的模型

Claude Code 支持几个模型选项：

| 模型 | 别名 | 特点 |
|------|------|------|
| **Fable 5** | `claude-fable-5` | 最新一代模型，综合能力最强，适合复杂任务 |
| **Sonnet** | `claude-sonnet-4-6` | 日常编码的主力，速度和能力的平衡点 |
| **Opus** | `claude-opus-4-8` | 复杂推理、架构设计、疑难 bug。更贵但更聪明 |
| **Haiku** | `claude-haiku-4-5-20251001` | 简单任务、快速搜索。最便宜最快 |

用 `/model` 随时切换，或者在启动时指定 `claude --model opus`。

> ⚠️ **注意**：`opusplan` 并非官方支持的模型别名。官方文档中没有这个组合模型。如果你想在规划阶段用 Opus、执行阶段用 Sonnet，可以手动在 Plan Mode 下切换 `/model opus`，规划完成后切回 `/model sonnet` 再执行。

### 9.2 工作量级别

工作量级别控制 Claude "想多深"：

| 级别 | 适用场景 |
|------|----------|
| `low` | 简单直接的任务，比如重命名变量、格式化代码 |
| `medium` | 常规编码，平衡成本和质量 |
| `high`（默认） | 大多数编码任务的合适选择 |
| `xhigh` | 复杂问题需要深入分析时 |
| `max` | 最深入的推理，但可能过度思考，收益递减 |

用 `/effort` 切换。一个实用的技巧：对于特别复杂的单次请求，可以用 `/effort max` 触发更深推理，不需要改变全局设置。

### 9.3 快速模式

`/fast` 开启快速模式——同样是 Opus，但响应速度提升最多 2.5 倍，代价是每个令牌更贵。适合交互式调试、实时迭代这类对延迟敏感的场景。对于长时间自主运行的任务，标准模式更划算。

> 💡 **经验**：快速模式在会话开始时开启最划算。中途开启需要为整个已有上下文支付快速模式的溢价。

---

## 第10章 常见工作流与最佳实践

这一章把日常开发中最常用的场景串一遍，每个场景都给出经过验证的提示词和操作流程。不是理论，是你今天就能用的东西。

### 10.1 代码库探索：快速理解陌生项目

刚接手一个项目，或者隔了很久再看以前的代码，最头疼的就是"这玩意儿到底怎么跑的"。Claude Code 在这里能帮你大忙。

#### 10.1.1 快速获取全局视图

```
> 给我一个这个代码库的整体概览，包括主要模块、技术栈和架构模式
```

Claude 会扫描目录结构、读取关键配置文件（`package.json`、`tsconfig.json` 等），然后给你一个结构化的概述。这比你自己一个个文件翻快得多。

#### 10.1.2 追踪特定功能的执行流程

```
> 用户认证的流程是怎样的？从前端登录按钮点击到后端鉴权完成，完整追踪一下
```

Claude 会用 Grep 搜索相关关键词（`login`、`auth`、`token` 等），Read 读取涉及的文件，然后给你一个从前端到后端的完整调用链。这在你理解复杂业务逻辑时特别有用。

#### 10.1.3 定位特定类型的代码

```
> 找到所有直接操作数据库的代码，我想了解数据访问层的结构
> 哪些文件里有硬编码的配置值？
> 找到所有使用了 deprecated API 的地方
```

> 💡 **经验**：探索代码时，从宽泛的问题开始（"整体架构是什么"），然后逐步缩小到具体领域（"认证模块怎么工作的"）。不要一上来就问特别细节的问题，先建立全局心智模型。

### 10.2 Bug 修复：从症状到根因

Bug 修复是 Claude Code 最实用的场景之一。关键是给 Claude 足够的上下文，让它能快速定位问题。

#### 10.2.1 推荐的流程

**1. 描述症状，而不是猜测原因**：

❌ 错误示范：

```
> user.ts 第 42 行有个 bug，帮我修一下
```

✅ 正确示范：

```
> 运行 npm test 时，auth.test.ts 报了 timeout 错误，测试在等待数据库连接时超时了
```

告诉 Claude 你看到了什么（错误信息、堆栈跟踪、异常行为），而不是你认为问题在哪里。让 Claude 自己判断根因。

**2. 提供重现步骤**：

```
> 重现步骤：
> 1. 启动本地服务器 npm run dev
> 2. 访问 /login 页面
> 3. 输入用户名密码点击登录
> 4. 页面卡在 loading 状态，控制台报 CORS 错误
```

**3. 让 Claude 先分析，再修复**：

对于复杂的 bug，先进 Plan Mode 让 Claude 分析根因：

```
> /plan 这个 timeout 错误可能的原因有哪些？帮我分析一下
```

审查 Claude 的分析，确认思路对了再让它执行修复。

**4. 验证修复**：

```
> 跑一下测试，确认修复没有引入新的问题
```

> 💡 **经验**：如果 bug 是间歇性的，告诉 Claude 这一点。间歇性问题和持续性问题排查思路完全不同。另外，如果错误信息很长，直接贴完整的堆栈跟踪，不要自己截取"看起来重要"的部分——你可能截错了。

### 10.3 重构：安全地改进代码结构

重构是 Claude Code 最擅长的场景之一，因为它需要同时理解多处代码的关联关系，而这正是 Claude 的强项。

#### 10.3.1 标准流程

**1. 先进 Plan Mode 分析影响范围**：

```
> /plan 我想把 UserService 拆分成 UserService 和 AuthService，帮我分析一下影响范围
```

Claude 会搜索所有引用 UserService 的地方，列出需要修改的文件，评估工作量。

**2. 讨论方案**：

```
> 你觉得拆分后，认证相关的逻辑应该放在 AuthService 还是单独的 AuthModule 里？
```

这一步很重要——Claude 可以给你多个方案，你选择最合适的。

**3. 执行重构**：

确认方案后，切回正常模式：

```
> 方案没问题，开始执行吧
```

**4. 验证无回归**：

```
> 跑一下完整测试套件，确保没有破坏现有功能
```

#### 10.3.2 重构时的提示词技巧

- "保持向后兼容"：如果你需要保持 API 不变
- "分步进行，每一步之后代码都应该是可运行的"：避免一次性改太多
- "先改最核心的部分，然后逐步扩展"：降低风险

> 💡 **经验**：大规模重构时，让 Claude 每改一个文件就跑一次测试，而不是一口气改完所有文件再测试。这样如果出了问题，你能立刻知道是哪一步引入的。

### 10.4 测试编写：生成高质量的测试用例

Claude 可以生成遵循你项目现有模式和约定的测试。关键是给它足够的上下文。

#### 10.4.1 识别未测试的代码

```
> 找到 NotificationsService 中没有被测试覆盖的函数
```

#### 10.4.2 生成测试

```
> 为 notification service 添加测试，遵循项目现有的测试风格
```

Claude 会读取你现有的测试文件，匹配使用的框架（Jest、Mocha 等）、断言风格和 mock 模式。

#### 10.4.3 覆盖边界情况

```
> 分析这些函数的代码路径，找出我可能遗漏的边界情况和错误条件
```

> 💡 **经验**：生成测试后，让 Claude 解释每个测试用例验证的是什么。如果你看不懂某个测试的意图，说明它写得不够好，让 Claude 改进。

### 10.5 会话管理：跨会话保持连续性

Claude Code 的会话可以暂停和恢复，这对于需要跨多天完成的任务特别有用。

#### 10.5.1 继续上次中断的会话

```bash
claude --continue
```

这会恢复你上次中断的会话，上下文完整保留。适合你昨天做到一半，今天继续的场景。

#### 10.5.2 从会话列表中选择恢复

```bash
claude --resume
```

这会显示最近的会话列表，你可以选择恢复哪一个。适合你有多个并行任务，想切换到另一个会话的场景。

#### 10.5.3 上下文压缩

当上下文快满时（Claude 会提示你），用 `/compact` 手动压缩：

```
> /compact
```

这会保留关键信息，丢弃不重要的中间过程。压缩后，Claude 可能"忘了"某些细节，所以重要信息最好写在 CLAUDE.md 里。

> 💡 **经验**：如果你发现压缩后 Claude 反复"忘了"某些指令，说明这些指令应该写在 CLAUDE.md 里，而不是依赖会话上下文。

### 10.6 并行工作：用 git worktrees 同时处理多个任务

如果你需要同时做功能开发和 bug 修复，或者想对比不同方案，可以用 git worktrees 在多个分支上同时工作。

#### 10.6.1 设置 worktrees

```bash
# 创建一个新的 worktree
git worktree add ../project-feature-a feature-a

# 在新目录里启动 Claude
cd ../project-feature-a
claude
```

每个 worktree 是一个独立的工作目录，有自己的文件副本。你可以在每个 worktree 里启动一个 Claude 会话，它们互不干扰。

#### 10.6.2 典型场景

- 一个 worktree 做功能开发，另一个修紧急 bug
- 对比两个不同的重构方案
- 同时处理多个 PR 的审查意见

> 💡 **经验**：worktrees 的好处不只是并行，还有**上下文隔离**。每个 Claude 会话只看到自己分支的代码，不会被其他分支的改动污染。

---

## 第11章 动态工作流——编排大规模任务

当你需要做的事情规模很大——审计整个代码库、批量迁移 500 个文件、交叉验证多个来源的研究——单个 Claude 会话可能不够用。动态工作流（Dynamic Workflows）就是为这种场景设计的。

📖 详见官方文档：Dynamic Workflows

### 11.1 核心思想：把计划移入代码

在普通的 Claude 会话中，Claude 是编排者：它逐轮决定接下来做什么，每个中间结果都进入上下文窗口。这在任务规模大时会遇到两个问题：

1. **上下文爆炸**：中间结果太多，上下文窗口装不下
2. **不可重复**：下次想再做一遍，得从头解释

动态工作流的解决方案是：**脚本持有循环和中间状态，Claude 的上下文只保留最终结果**。

比如一个代码审计任务，脚本会：
1. 列出所有需要审计的模块
2. 为每个模块启动一个子代理
3. 收集每个子代理的发现
4. 汇总成最终报告

Claude 不需要记住中间过程，只输出最终结论。而且这个脚本可以保存下来，下次直接重新运行。

### 11.2 何时使用工作流

工作流、子代理、Skills 和 Agent Teams 都可以运行多步骤任务，区别在于谁掌握计划：

| 维度 | 子代理 | Skills | Agent Teams | 工作流 |
|------|--------|--------|-------------|--------|
| 它是什么 | Claude 生成的工具作者 | Claude 遵循的指令 | 监督对等会话的主导代理 | 运行时执行的脚本 |
| 谁决定接下来做什么 | Claude，逐轮 | Claude，遵循提示 | 主导代理，逐轮 | 脚本 |
| 中间结果在哪 | Claude 的上下文窗口 | Claude 的上下文窗口 | 共享任务列表 | 脚本变量 |
| 什么是可重复的 | 工作者定义 | 指令 | 团队定义 | 编排本身 |
| 规模 | 与子代理相同 | 每轮几个委派任务 | 少数几个长期运行的对等体 | 每次运行数十到数百个代理 |

#### 11.2.1 使用工作流的典型场景

- **代码库范围的错误扫描**：扫描所有模块，寻找特定模式的问题
- **大规模迁移**：500 个文件需要更新 API 调用方式
- **交叉检查研究**：从多个角度调查一个问题，然后交叉验证来源
- **困难计划**：值得从多个独立角度起草，然后相互权衡的方案

### 11.3 使用方式

#### 11.3.1 内置工作流工具

Claude Code 提供了一些内置的工作流工具：

| 工具 | 用途 |
|------|------|
| `/deep-research` | 跨多个来源调查问题，自动扇出搜索并综合报告 |
| `/workflows` | 查看正在运行的工作流进度 |

> 💡 工作流的推理深度受 `/effort` 命令影响，详见 [9.2 工作量级别](#92-工作量级别)。

#### 11.3.2 内置工作流：/deep-research

最快的体验方式是运行 `/deep-research`，这是 Claude Code 内置的工作流，用于跨多个来源调查问题：

```
> /deep-research Node.js v20 到 v22 之间权限模型有什么变化？
```

Claude 会：
1. 在多个角度上扇出网络搜索
2. 获取并交叉检查找到的来源
3. 综合一份带引用的报告

运行在后台进行，你的会话保持空闲。最后你得到一份报告，而不是逐轮的搜索记录。

#### 让 Claude 为你的任务编写工作流

对于自定义任务，你可以让 Claude 为你编写工作流脚本：

```
> /effort max
> 审计整个代码库，找出所有使用了 deprecated API 的地方，按模块生成报告
```

Claude 会自动分析你的任务，判断是否需要编排工作流，并生成相应的编排脚本。脚本在后台运行，你可以继续做其他事情。

#### 查看工作流进度

```
> /workflows
```

这会显示所有正在运行的工作流，你可以选择查看某个工作的详细进度。

### 11.4 工作流的质量模式

工作流不只是"运行更多代理"，它还可以应用质量模式：

- **对抗性审查**：让独立的代理在报告之前对彼此的发现进行审查，找出遗漏
- **多角度起草**：从多个角度起草计划，然后相互权衡，得到比单次通过更可信的结果
- **交叉验证**：多个代理独立调查同一个问题，然后交叉检查来源

> 💡 **经验**：大规模任务适合那些你会本能地觉得"这事儿得拆成好几步做"的场景。你只需要描述目标，Claude 会自己决定怎么拆分和编排。不要试图手动控制每一步——那还不如不用工作流。

---

## 第12章 Code Review——自动化代码审查

Claude Code 内置了代码审查系统。它不是简单的 lint 检查，而是多维度分析你的代码变更，寻找逻辑错误、安全漏洞和潜在的回归问题。

📖 详见官方文档：Code Review

### 12.1 审查机制

当你运行 `/code-review` 时，Claude 会：

1. **获取变更**：运行 `git diff` 获取所有代码变更
2. **多维度分析**：逐个文件审查，检查：
   - 类型安全和边界条件
   - 错误处理是否完整
   - 性能隐患（N+1 查询、不必要的循环等）
   - 安全漏洞（SQL 注入、XSS 等）
3. **生成报告**：输出分级的审查报告

### 12.2 审查结果

每个发现都标记严重程度：

| 标记 | 含义 |
|------|------|
| 🔴 **重要** | 合并前应该修复的问题。会导致 bug、安全漏洞或数据丢失 |
| 🟡 **小问题** | 值得改进但不阻塞。代码质量可以提升，但不影响功能 |
| 🟣 **预先存在** | 代码库里本来就有、但不是这个 PR 引入的问题。提醒你注意，但不要求在这个 PR 里修复 |

#### 12.2.1 示例报告

```
代码审查完成。发现以下问题：

🔴 重要（2 个）
- src/api/users.ts:45 - SQL 查询直接拼接用户输入，存在 SQL 注入风险
  建议：使用参数化查询
- src/services/payment.ts:78 - 支付失败时没有回滚事务
  建议：在 catch 块中调用 transaction.rollback()

🟡 小问题（3 个）
- src/utils/logger.ts:12 - 日志级别硬编码为 debug，生产环境应该用 info
- src/routes/auth.ts:34 - 错误消息暴露了内部实现细节
- tests/user.test.ts:56 - 测试数据没有清理，可能影响其他测试

🟣 预先存在（1 个）
- src/db/connection.ts:8 - 数据库连接池没有配置最大连接数（不是这个 PR 引入的）
```

### 12.3 自定义审查行为

> ⚠️ **注意**：`REVIEW.md` 机制为社区实践，以下示例基于社区用法。官方文档中可能使用不同的配置方式，请查阅 [Code Review 官方文档](https://code.claude.com/docs/zh-CN/code-review) 确认。

通过 `REVIEW.md` 文件可以精细控制审查行为：

#### 12.3.1 重新定义"重要"的标准

```markdown
# REVIEW.md
对于原型项目，所有风格问题都降级为"小问题"。
只有会导致数据丢失或安全漏洞的问题才标记为"重要"。
```

#### 12.3.2 限制小问题的数量

```markdown
# REVIEW.md
小问题最多报告 5 个。优先报告最严重的。
避免审查意见淹没真正重要的发现。
```

#### 12.3.3 跳过特定路径

```markdown
# REVIEW.md
跳过以下路径的审查：
- generated/** （自动生成的代码）
- *.lock （lockfile）
- migrations/** （数据库迁移脚本）
```

#### 12.3.4 添加项目特有的检查项

```markdown
# REVIEW.md
额外检查：
- 新 API 路由必须有集成测试
- 所有数据库查询必须使用参数化查询
- 密码相关操作必须使用 bcrypt，至少 10 轮
```

### 12.4 本地使用

不需要安装 GitHub App 也能用。在任何会话中运行 `/code-review` 就能审查当前差异：

```
> /code-review
```

这会审查自上次提交以来的所有变更。

#### 12.4.1 审查特定分支的差异

```
> /code-review main
```

这会审查当前分支相对于 main 分支的所有变更。

#### 12.4.2 自动修复发现的问题

```
> /code-review --fix
```

Claude 会尝试自动修复发现的问题（主要是小问题，重要问题会让你确认）。

#### 12.4.3 使用不同深度进行审查

```
> /code-review --effort high
```

调节审查深度，`high` 或 `max` 会进行更全面的分析，但消耗更多资源。

#### 12.4.4 将审查结果作为 PR 评论发布

```
> /code-review --comment
```

将审查发现发布为 GitHub PR 的 inline 行内评论。

> ⚠️ **注意**：此功能需要 GitHub App 或相应的权限配置。

### 12.5 GitHub App 集成

如果你想在每个 PR 上自动审查，可以安装 Claude Code GitHub App：

1. 在 GitHub 上安装 App
2. 授权访问你的仓库
3. 每个 PR 打开时，Claude 自动审查并发布评论

> 💡 **经验**：Code Review 的价值不只是找 bug，还有**知识传递**。审查报告会解释"为什么这是个问题"，帮助你和他人都能学到东西。定期查看审查报告，即使没有发现问题，也能了解 Claude 关注哪些方面。

---

## 第13章 理解上下文成本——选对扩展机制

Claude Code 提供了多种扩展机制（CLAUDE.md、Skills、Subagents、Hooks、路径范围规则），但它们的"上下文成本"差异很大。理解这一点，才能做出正确的架构选择——不是所有功能都应该无脑堆上去。

### 13.1 上下文成本是什么

Claude 的上下文窗口是有限的（200K tokens）。每次会话开始时，Claude 会加载各种信息：你的指令、代码文件、对话历史、CLAUDE.md、Skills 等。这些信息都要占用上下文空间。

**上下文成本**就是某个功能在每次会话中占用的上下文空间。成本越高，留给实际工作的空间就越少。

### 13.2 各机制的上下文成本

#### 13.2.1 CLAUDE.md：始终加载，成本最高

CLAUDE.md 在每个会话开始时自动加载。无论你这轮对话做什么，CLAUDE.md 的内容都在上下文里。

这意味着：
- CLAUDE.md 的内容每次会话都要占用上下文空间
- 如果你一天开多个会话，这些 tokens 就被消耗多次
- 更糟的是，CLAUDE.md 太长会降低 Claude 的遵守度——它"记不住"所有指令

**所以**：CLAUDE.md 应该只包含真正每次都需要的信息——构建命令、核心约定、项目结构。如果某条指令只在特定场景下才需要（比如"处理 API 路由时要用特定格式"），它不应该在 CLAUDE.md 里。

#### 13.2.2 Skills：按需加载，成本中等

Skills 只在你调用或 Claude 自动匹配时才加载到上下文中。平时不占空间。

这意味着：
- 如果你有一个 code-review Skill，但今天没做代码审查，它就不占上下文
- 如果你调用了 `/code-review`，Skill 的内容会注入上下文，占用一定空间
- 用完后，Skill 的内容会随着对话进行逐渐被压缩或丢弃

**所以**：Skills 适合多步骤工作流和参考文档——平时不占空间，需要时完整注入。但注意，一个 Skill 不要太长，否则加载时会占用大量上下文。

#### 13.2.3 Subagents：独立上下文，对主会话零成本

Subagents 在独立的上下文窗口中运行。它们读取文件、分析代码的中间过程都在子代理的上下文里，主会话只接收最终摘要。

这意味着：
- 一个子代理可能读取了 50 个文件，但主会话只看到一段 200 字的摘要
- 主会话的上下文成本为零（除了摘要占用的几百 tokens）
- 子代理的上下文是独立的，不会影响主会话

**所以**：Subagents 适合中间产物多的任务——大量文件读取、深度分析、并行研究。主会话只接收最终结论。

#### 13.2.4 Hooks：事件触发，通常零成本

Hooks 在特定事件发生时执行（如工具调用前后）。除非 Hook 返回输出并注入上下文，否则不占用上下文空间。

这意味着：
- 一个 PostToolUse Hook 在每次文件编辑后自动运行 Prettier，但不占用上下文
- 如果 Hook 返回错误信息，这些信息会进入上下文，占用少量空间
- 大多数 Hooks 的上下文成本为零

**所以**：Hooks 适合确定性操作——格式化、检查、通知。这些操作必须执行，但不需要 Claude "知道"它们的存在。

#### 13.2.5 MCP 服务器：工具定义常驻，调用结果进入上下文

MCP 服务器的工具定义在会话开始时加载到上下文中，但只有当你调用工具时，结果才会进入上下文。

这意味着：
- 如果你配置了多个 MCP 服务器，这些工具的定义会占用上下文空间
- 如果你今天没有调用任何 MCP 工具，这些定义仍然占用上下文
- 如果你调用了某个 MCP 工具，工具的返回结果会进入上下文，占用额外空间

**所以**：MCP 适合需要频繁使用的外部工具。如果你只是偶尔用一次数据库查询，可能不值得配置 MCP——直接在终端里用 SQL 客户端更快。只有当你需要 Claude 主动、多次调用某个外部工具时，MCP 的上下文成本才值得。

#### 13.2.6 插件：组合成本，取决于内容

插件本身不直接占用上下文，但插件包含的 Skills、Agents、MCP 配置会按照各自的机制占用上下文。

这意味着：
- 如果插件包含 3 个 Skills，这些 Skills 的定义会注册到系统中，但只有调用时才加载
- 如果插件包含 MCP 配置，MCP 工具的定义会在会话开始时加载
- 如果插件包含 Hooks，Hooks 通常不占上下文

**所以**：插件的上下文成本是其内容的组合。安装插件前，看看它包含什么——如果只是 Skills，成本很低（按需加载）；如果包含大量 MCP 工具，成本较高（常驻上下文）。

#### 13.2.7 路径范围规则：按需加载，成本较低

路径范围规则（`.claude/rules/*.md`）只在 Claude 处理匹配文件时加载。

这意味着：
- 如果你有一个 `api-rules.md`，只在 Claude 处理 `src/api/` 下的文件时加载
- 如果你今天只改了前端代码，API 规则就不占上下文
- 多个路径范围规则可以共存，互不干扰

**所以**：路径范围规则适合特定文件类型的约定——API 规则不干扰前端代码，测试规则不干扰生产代码。

### 13.3 决策流程

一个实用的决策流程：

1. **这条规则每次都需要吗？**
   - 是 → CLAUDE.md
   - 否 → 继续

2. **这个流程多步骤且可复用吗？**
   - 是 → Skill
   - 否 → 继续

3. **这个任务会产生大量中间信息吗？**
   - 是 → Subagent
   - 否 → 继续

4. **这个操作必须执行、不能靠 Claude 自觉吗？**
   - 是 → Hook
   - 否 → 继续

5. **需要 Claude 主动调用外部工具（数据库、API、浏览器）吗？**
   - 是 → MCP 服务器
   - 否 → 继续

6. **这条规则只对特定文件有效吗？**
   - 是 → `.claude/rules/`
   - 否 → 继续

7. **需要把多个扩展打包分发给团队吗？**
   - 是 → 插件
   - 否 → 可能不需要

### 13.4 常见错误

**错误 1：把所有规则都塞进 CLAUDE.md**
- 后果：CLAUDE.md 超过 200 行，Claude 遵守度下降，上下文空间不足
- 解决：把特定场景的规则移到 `.claude/rules/` 或 Skills 里

**错误 2：在主会话中做重型分析**
- 后果：读取 50 个文件的中间过程占满上下文，后续对话空间不足
- 解决：把重型分析交给 Subagents，主会话只收摘要

**错误 3：在 CLAUDE.md 里写"记得跑 lint"**
- 后果：Claude 可能忘记，因为 CLAUDE.md 只是建议，不是强制执行
- 解决：用 PostToolUse Hook 自动跑 lint

**错误 4：给所有文件类型都写路径范围规则**
- 后果：规则太多，管理复杂，加载时占用大量上下文
- 解决：只给真正需要区分的文件类型写规则，其他放在 CLAUDE.md 里

---

## 第14章 实战案例：从零搭建任务管理 API

理论讲了很多，让我们通过一个完整的项目实例，看看这些功能如何在实际开发中协同工作。

### 14.1 项目背景

假设你要从零开始构建一个任务管理 API（Task Management API），技术栈选择 Node.js + Express + TypeScript + SQLite。这是一个常见的 CRUD 应用，但我们会通过它展示 Claude Code 各功能的实战应用。

项目目标：
- 支持任务的创建、查询、更新、删除
- 用户认证（JWT）
- 完整的测试覆盖
- 代码质量保障（lint、格式化、安全检查）

### 14.2 总体工作步骤

在开始编码之前，我们先规划一下整个项目的工作流程。这个项目会经历以下九个阶段，每个阶段都会用到不同的 Claude Code 功能：

| 阶段 | 核心任务 | 使用的功能 | 为什么用这个 |
|------|----------|-----------|-------------|
| 第一步 | 初始化项目与配置记忆系统 | CLAUDE.md + 路径范围规则 | 设定项目基调，分流特定规则 |
| 第二步 | 创建 API 端点 | Skills（`/create-api`） | 封装可复用的多步骤工作流 |
| 第三步 | 代码质量保障 | Hooks（PostToolUse + PreToolUse） | 确定性执行格式化和安全检查 |
| 第四步 | 调试和重构 | Subagents（隔离分析） | 重型任务不污染主上下文 |
| 第五步 | 重大重构 | Plan Mode + 模型切换 | 先分析后执行，复杂任务用 Opus |
| 第六步 | 代码审查 | `/code-review` + 工作量调节 | 内置流程，可调深度 |
| 第七步 | 自动化部署 | CI/CD 集成 + GitHub Actions | 自动化 CI/CD，云端运行 |
| 第八步 | 集成外部工具 | MCP 服务器（数据库、项目管理） | 连接外部系统，端到端自动化 |
| 第九步 | 团队工作流共享 | 插件（打包 Skills + MCP + Hooks） | 团队知识沉淀和分发 |

这个顺序不是随意的——它反映了项目从"建立基础"到"持续交付"的自然演进。每一步都在前一步的基础上工作，功能之间相互协同。下面我们就按这个顺序，逐步展开每个阶段的具体操作。

### 14.3 第一步：初始化项目与配置记忆系统

#### 14.3.1 你的操作

```bash
mkdir task-api && cd task-api
npm init -y
npm install express typescript @types/express @types/node sqlite3
npm install --save-dev jest @types/jest ts-jest prettier eslint
npx tsc --init
```

项目结构初始化完成后，你打开 Claude Code，开始配置记忆系统。

#### 14.3.2 配置 CLAUDE.md

```markdown
# Task API 项目

### 构建和运行
- 构建：`npm run build`（编译 TypeScript）
- 启动：`npm start`（运行编译后的代码）
- 开发模式：`npm run dev`（使用 ts-node-dev 热重载）
- 测试：`npm test`

### 代码约定
- 使用 2 空格缩进
- 所有 API 端点以 `/api/v1` 为前缀
- 错误响应格式：`{ error: { code: string, message: string } }`
- 所有异步操作必须有 try-catch 错误处理
- 数据库操作封装在 `src/db/` 目录，不直接在路由中写 SQL

### 测试要求
- 每个 API 端点必须有对应的集成测试
- 测试文件放在 `tests/` 目录，命名 `*.test.ts`
- 测试前确保数据库是干净的（使用 beforeAll 清空表）

### 安全规则
- 不要在代码中硬编码 JWT_SECRET，从环境变量读取
- 所有用户输入必须验证（使用 zod）
- 密码必须用 bcrypt 加密，至少 10 轮
```

配置路径范围规则 `.claude/rules/api-rules.md`：

```markdown
---
paths:
  - "src/routes/**/*.ts"
---
# API 路由规则
- 每个路由文件必须导出一个 Express Router
- 路由命名：GET /tasks（列表）、POST /tasks（创建）、GET /tasks/:id（详情）等
- 所有路由要有对应的验证中间件（用 zod schema）
- 错误处理统一用 asyncHandler 包装
```

#### 14.3.3 为什么这样做

- **初始化配置**包含每次都需要的信息（构建命令、核心约定）
- **路径范围规则**只在 Claude 处理路由文件时加载，避免污染其他文件的上下文
- **具体到可验证的指令**（"2 空格缩进"而不是"格式化好"）

### 14.4 第二步：创建 API 端点——Skills 封装工作流

#### 14.4.1 你的需求

你要创建多个 API 端点（任务 CRUD、用户认证），每个端点的创建流程类似：定义路由、写验证、实现逻辑、写测试。

```yaml
---
name: create-api
description: 创建新的 API 端点，包括路由、验证、逻辑和测试
---
```

```markdown
# 创建 API 端点

当被要求创建新的 API 端点时，按以下步骤执行：

### 1. 定义路由和验证
在 `src/routes/` 下创建或更新路由文件，包含验证 schema 和路由处理。

### 2. 实现业务逻辑
在 `src/services/` 下创建对应的服务文件，封装数据库操作。

### 3. 编写集成测试
在 `tests/` 下创建测试文件，包含正常流程和错误处理的测试用例。

### 4. 验证
- 运行 `npm test` 确保所有测试通过
- 运行 `npm run lint` 确保代码风格一致
- 手动测试端点（用 curl 或 Postman）
```

#### 14.4.2 使用 Skill

现在你可以直接说：

```
> /create-api 创建一个获取任务列表的端点，支持分页和筛选
```

Claude 会加载这个 Skill，按照你定义的流程执行：创建路由、写验证、实现服务、写测试。你不需要每次都重复解释这些步骤。

#### 14.4.3 为什么这样做

- **封装了可复用的多步骤工作流**
- 每个步骤都有具体的代码示例，Claude 不会偏离
- 包含验证步骤，确保任务真正完成
- 下次创建端点时，直接调用 `/create-api`，效率大幅提升

### 14.5 第三步：代码质量保障——Hooks 自动化

#### 14.5.1 你的需求

你希望 Claude 每次编辑文件后自动格式化，并且阻止它执行危险命令。

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

#### 14.5.2 效果

- Claude 每次编辑文件后，Prettier 自动运行，代码格式始终一致
- 如果 Claude 试图执行 `rm -rf` 或危险的 SQL，Hook 会阻止它
- 这些是确定性操作，不依赖 Claude "记得"去做

> ⚠️ **前提条件**：`jq` 需要提前安装。常见安装方式：
> - macOS: `brew install jq`
> - Linux: `apt install jq` 或 `yum install jq`
> - Windows: `choco install jq`
>
> 建议在实际生产环境中使用更健壮的路径验证脚本。

#### 14.5.3 为什么这样做

- **提供强制执行**，CLAUDE.md 里的"记得跑 lint"只是建议
- `PostToolUse` 在工具执行后触发，适合格式化和检查
- `PreToolUse` 在工具执行前触发，适合阻止危险操作
- **零上下文成本**，除非 Hook 返回输出

### 14.6 第四步：调试和重构——Subagents 隔离分析

#### 14.6.1 场景

你的应用运行一段时间后，内存占用越来越高。你需要定位内存泄漏。

```
> 应用运行一段时间后内存占用持续增长，帮我分析可能的内存泄漏点
```

#### 14.6.2 Claude 的响应

Claude 判断这是一个需要深度分析的任务，会启动一个子代理进行扫描。你看不见中间过程，只收到最终摘要。

#### 14.6.3 子代理返回摘要

```
分析完成。发现以下潜在的内存泄漏点：

1. src/services/cache.ts:15 - 全局 Map 没有大小限制，会无限增长
   建议：使用 LRU 缓存或设置最大条目数

2. src/routes/websocket.ts:23 - WebSocket 连接关闭时没有移除事件监听器
   建议：在 close 事件中调用 removeAllListeners()

3. src/db/connection.ts:8 - 数据库连接池没有配置最大连接数
   建议：设置 max 参数限制连接池大小
```

#### 14.6.4 为什么这样做

- **隔离分析**：内存泄漏需要读取大量文件，中间产物很多
- 如果在主会话中做，这些中间信息会占满上下文窗口
- 子代理在独立上下文中运行，主会话只接收最终摘要
- 子代理可以用 Haiku 模型（更快更便宜），因为这是模式匹配任务

### 14.7 第五步：重大重构——Plan Mode + 模型切换

#### 14.7.1 场景

你决定把数据库从 SQLite 迁移到 PostgreSQL。这是一个重大重构，影响所有数据库操作。

1. 切换到 Plan Mode（输入 `/plan`）
2. 可选：切换模型到 Opus（`/model opus`）以获得更深入的规划能力
3. 让 Claude 分析影响范围
4. 审查 Claude 的迁移计划
5. 确认计划后切回 Sonnet 执行
6. 执行过程中用 `/code-review` 检查变更质量

#### 14.7.2 为什么这样做

- **让你先看到方案再动手**，避免大量不想要的改动
- 重大重构影响范围广，需要全面分析，Plan Mode 不会遗漏
- **模型切换**让你在最需要智力的地方（规划）用最强的模型，在执行阶段省成本
- 手动在 Opus 和 Sonnet 之间切换，灵活控制成本

### 14.8 第六步：代码审查——Code Review + 模型调节

#### 14.8.1 场景

你完成了迁移，准备提交 PR。你想让 Claude 做最后的代码审查。

```
> /code-review
```

Claude 会运行 `git diff` 获取所有变更，逐个文件审查，检查类型安全、错误处理、性能，验证测试覆盖，最后给出分级反馈。

#### 14.8.2 为什么这样做

- `/code-review` 是内置的审查流程，不需要每次解释
- 分级反馈让你快速定位最重要的问题
- 工作量级别可以调节审查深度
- `/effort max` 适合针对特定方面的深度分析

### 14.9 第七步：自动化部署——CI/CD 集成

#### 14.9.1 场景

你希望每次 PR 合并后自动运行测试和部署。

#### 14.9.2 使用 GitHub Actions + Claude Code

Claude Code 可以与 CI/CD 系统集成，实现自动化工作流。推荐的方式是通过 GitHub Actions 调用 Claude Code CLI：

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

> ⚠️ **注意**：上述 `npm install -g @anthropic-ai/claude-code` 安装命令仅为示例，具体安装方式请查阅 [官方文档的安装指南](https://code.claude.com/docs/zh-CN/) 确认。

#### 14.9.3 典型场景

- **PR 审查自动化**：每个 PR 打开时自动审查代码质量
- **部署验证**：CD 管道部署完成后自动触发烟雾测试
- **文档漂移检测**：每周扫描合并的 PR，标记过时的文档

> 💡 **经验**：通过 CI/CD 集成，你可以把 Claude Code 的能力延伸到自动化领域。结合 GitHub Actions 或其他 CI 工具，可以实现复杂的自动化工作流，而不需要编写复杂的脚本。

### 14.10 第八步：集成外部工具——MCP 连接数据库和项目管理

项目基本完成后，你希望 Claude 能直接查询数据库、管理 Jira 任务，而不是每次都切换到其他工具。

#### 14.10.1 配置数据库 MCP

```bash
# 添加 SQLite MCP（因为项目用 SQLite）
claude mcp add sqlite -- npx -y @modelcontextprotocol/server-sqlite

# 配置数据库路径
export SQLITE_DB_PATH=./task-api.db
```

使用示例：

```
> 查询 users 表中注册时间在最近 7 天的用户数量
> 分析 tasks 表中每个用户的任务完成情况，找出 top 5 活跃用户
> 检查是否有任务创建后超过 30 天未更新
```

Claude 会生成 SQL、执行查询、分析结果。你不需要离开终端，也不需要记住表结构——Claude 会自动读取数据库 schema。

#### 14.10.2 配置 Jira MCP

```bash
# 添加 Jira MCP
claude mcp add jira -- npx -y @modelcontextprotocol/server-jira

# 配置 Jira 信息
export JIRA_BASE_URL=https://your-team.atlassian.net
export JIRA_EMAIL=<your_email>
export JIRA_API_TOKEN=<your_api_token>
```

使用示例：

```
> 查看 PROJ-123 这个 issue 的详细信息
> 创建一个新 issue，标题是"修复登录页面的 CORS 错误"，分配给我
> 把 PROJ-456 的状态改为"进行中"
> 列出所有标记为"紧急"的 issue
```

这样你可以在编码的同时管理任务，不需要切换到浏览器。Claude 会根据你的指令自动调用 Jira API。

#### 14.10.3 为什么这样做

- MCP 让 Claude 从"代码助手"变成"全能工作台"，可以操作外部系统
- 数据库 MCP 让你快速查询和分析数据，不需要写 SQL 客户端
- 项目管理 MCP 让你在编码的同时管理任务，提升效率
- 但要注意——MCP 工具越多，上下文成本越高。只添加真正需要的工具

### 14.11 第九步：团队工作流共享——插件打包和分发

项目完成后，你希望把这套工作流（Skills + MCP + Hooks）分享给团队，让新同事也能快速上手。

#### 14.11.1 创建插件

在项目根目录创建 `.claude/plugins/task-api-workflow/` 目录：

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

`plugin.json`：

```json
{
  "name": "task-api-workflow",
  "version": "1.0.0",
  "description": "Task API 项目标准工作流",
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

`mcp/servers.json`：

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

`hooks/hooks.json`：

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

#### 14.11.2 分发插件

把插件推送到团队的 Git 仓库：

```bash
cd .claude/plugins
git init
git add .
git commit -m "Initial plugin release"
git remote add origin https://github.com/your-team/task-api-workflow-plugin.git
git push -u origin main
```

团队成员安装：

```bash
claude plugin install https://github.com/your-team/task-api-workflow-plugin
```

#### 14.11.3 为什么这样做

- 插件把 Skills、MCP、Hooks 打包在一起，一键安装
- 团队成员共享同一套工作流，减少配置差异
- 插件有版本号，可以追踪变更和回滚
- 新同事入职就能直接用上验证过的最佳实践

### 14.12 经验回顾

| 阶段 | 使用的功能 | 为什么用这个 |
|------|-----------|-------------|
| 初始化 | CLAUDE.md + 路径范围规则 | 设定项目基调，分流特定规则 |
| 创建 API | Skills（`/create-api`） | 封装可复用的多步骤工作流 |
| 代码质量 | Hooks（PostToolUse + PreToolUse） | 确定性执行格式化和安全检查 |
| 调试 | Subagents（隔离分析） | 重型任务不污染主上下文 |
| 重构 | Plan Mode + 模型切换 | 先分析后执行，复杂任务用 Opus |
| 审查 | `/code-review` + 工作量调节 | 内置流程，可调深度 |
| 部署 | CI/CD 集成 + GitHub Actions | 自动化 CI/CD，云端运行 |
| 集成工具 | MCP 服务器（数据库、Jira） | 连接外部系统，端到端自动化 |
| 团队共享 | 插件（打包 Skills + MCP + Hooks） | 团队知识沉淀和分发 |

#### 14.12.1 最后的建议

不要试图一次性用上所有功能。从 CLAUDE.md 开始，逐步引入 Skills 和 Hooks，等你熟悉了再尝试 Subagents 和 CI/CD 集成。每个功能都有学习曲线，循序渐进才能掌握。

---

## 第15章 关键经验总结

### 15.1 关于记忆和上下文

- CLAUDE.md 不是写得越多越好。超过 200 行后，Claude 的遵守度会明显下降。把特定场景的规则分流到 `.claude/rules/` 里，让 CLAUDE.md 保持精简和聚焦
- 上下文压缩（`/compact`）后，重要信息可能会丢失。如果你发现压缩后 Claude "忘了"某些指令，可以考虑用 SessionStart hook（matcher 设为 `compact`）重新注入关键上下文 ⚠️ 具体行为请确认当前版本，或者直接把这些指令写进项目根目录的 CLAUDE.md 里
- 自动记忆是 Claude 给自己做的笔记，不总是准确的。定期用 `/memory` 审查一下，清理掉错误或过时的内容

### 15.2 关于 Skills

- Skills 最关键的用法是**自包含的深度指令**。当 Skill 在子代理中运行时，它看不到你的对话历史，所以所有前置信息收集步骤、判断标准、输出格式都要写在 Skill 正文里
- **子代理隔离**是处理"重型"任务的利器。代码库审计、大规模重构分析这类任务，让它们在子代理里跑，主会话只收报告。但记住——子代理看不到你的对话历史，Skill 正文必须自包含
- 用 `/run` 固化复杂项目的启动方式。每个团队都有"只有某个人知道怎么启动"的项目，把这个知识变成 Skill，所有人都能用

### 15.3 关于子代理

- 给子代理**限制工具权限**。一个代码审查子代理只需要 `Read, Glob, Grep`，不给它写权限——这样即使它"想"修改代码也做不到
- 简单任务用 `haiku` 模型跑子代理，比在主会话里用 Opus 做同样的事便宜得多
- 子代理返回的结果是"自报告"——它说自己完成了不代表真的完成了。对于关键操作（部署、数据库变更），自己验证一下子代理的输出

### 15.4 关于权限和工作流

- 重大变更前一定先用 **Plan Mode**。让 Claude 出方案，你确认思路对了再执行。这比让它直接动手、然后花大量时间回滚不想要的改动要高效得多
- 对于"必须做"的操作（lint、格式化、安全检查），用 **Hooks** 而不是在 CLAUDE.md 里写"记得跑 lint"。Hooks 是确定性的，CLAUDE.md 只是建议

### 15.5 关于 MCP 和插件

**MCP 使用经验**：

- **最小权限原则**：MCP 服务器应该只暴露必要的工具。比如文件系统 MCP 应该限制访问目录，不要给整个根目录。数据库 MCP 应该只给读权限，除非确实需要写操作
- **环境变量管理**：敏感信息（API token、密码）用环境变量传递，不要硬编码。可以用 `.env` 文件或系统环境变量。把 `.env` 加入 `.gitignore`，避免泄露
- **上下文成本意识**：MCP 工具的定义在会话开始时加载到上下文中。如果你配置了 15 个 MCP 工具，即使你今天只用了一个，其他 14 个的定义仍然占用上下文。只添加真正需要的工具
- **错误处理**：MCP 服务器应该返回清晰的错误信息。如果工具调用失败，Claude 需要知道原因才能决定下一步。模糊的错误信息（如"操作失败"）会让 Claude 无法有效处理
- **性能考虑**：MCP 工具调用会阻塞 Claude 的执行。如果某个工具执行时间很长（如大规模数据处理），考虑返回任务 ID，让 Claude 轮询状态，而不是一直等待

**插件使用经验**：

- **插件 vs 直接配置**：如果只是个人使用，直接配置 Skills 和 Hooks 更简单。插件适合需要分发给多人的场景——团队协作、开源项目、标准化工作流
- **插件内容审查**：安装插件前，看看它包含什么。如果只是 Skills，成本很低（按需加载）；如果包含大量 MCP 工具，成本较高（常驻上下文）。不要安装你不需要的插件
- **版本管理**：插件有版本号，更新时要谨慎。重大更新可能改变工作流行为，先在测试环境验证，再推广到团队
- **团队知识沉淀**：把验证过的最佳实践（代码审查流程、部署步骤、调试套路）打包成插件，新同事入职就能直接用上。这比让每个人自己摸索效率高得多
- **插件维护**：插件不是一劳永逸的。随着项目演进，插件也需要更新。指定专人负责插件维护，定期审查和更新

### 15.6 关于成本

- 快速模式（`/fast`）在会话开始时开启最划算。中途开启需要为整个已有上下文支付快速模式的溢价
- 大规模任务会消耗较多资源。不要在日常小编改时用，留给代码库审计、大规模迁移这类真正需要编排的场景

---

## 最后的话

Claude Code 的强大不在于某个单一功能，而在于这些扩展层的灵活组合：

- **CLAUDE.md** 设定基调
- **Skills** 封装流程
- **Subagents** 隔离重活
- **Hooks** 兜底确定性操作
- **MCP** 扩展能力边界
- **插件** 打包分发团队知识

理解每层的定位和成本，就能构建出既高效又可控的开发工作流。希望这个实战案例能帮你把这些功能串联起来，形成自己的最佳实践。

---

## 附录：快速参考卡片

### 常用命令速查

| 命令 | 用途 |
|------|------|
| `claude` | 启动 Claude Code |
| `claude --model opus` | 指定模型启动 |
| `claude --dangerously-skip-permissions -p "..."` | CI/CD 无头模式 |
| `/plan` | 切换到计划模式 |
| `/permissions auto` | 切换到自动权限模式 |
| `/model` | 切换模型 |
| `/effort high\|max` | 调节推理深度 |
| `/compact` | 压缩上下文 |
| `/code-review` | 代码审查 |
| `/deep-research` | 深度研究 |
| `/workflows` | 查看工作流进度 |
| `/memory` | 管理自动记忆 |
| `--continue` | 继续上次会话 |
| `--resume` | 从会话列表选择恢复 |

### 扩展机制选择决策树

```
需要每次都用的规则？
├─ 是 → CLAUDE.md
└─ 否 → 多步骤可复用流程？
         ├─ 是 → Skill
         └─ 否 → 产生大量中间信息？
                  ├─ 是 → Subagent
                  └─ 否 → 必须强制执行？
                           ├─ 是 → Hook
                           └─ 否 → 需要调用外部工具？
                                    ├─ 是 → MCP
                                    └─ 否 → 只对特定文件有效？
                                             ├─ 是 → .claude/rules/
                                             └─ 否 → 需要打包分发？
                                                      ├─ 是 → 插件
                                                      └─ 否 → 不需要扩展
```

### 上下文成本对比

| 机制 | 加载时机 | 成本级别 |
|------|----------|----------|
| CLAUDE.md | 每次会话自动加载 | 高 |
| Skills | 调用时按需加载 | 中 |
| Subagents | 独立上下文 | 零（对主会话） |
| Hooks | 事件触发 | 通常零 |
| MCP | 工具定义常驻 | 中 |
| 插件 | 组合成本 | 取决于内容 |
| 路径范围规则 | 匹配时加载 | 低 |
