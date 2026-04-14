# Agents vs Commands vs Skills — 何时使用什么

Claude Code 中三种扩展机制的比较：subagents、commands 和 skills。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

![Slash menu showing time-skill, time-command, and time-agent](assets/agent-command-skill-1.jpg)

---

## 一览

| | Agent | Command | Skill |
|---|---|---|---|
| **位置** | `.claude/agents/<name>.md` | `.claude/commands/<name>.md` | `.claude/skills/<name>/SKILL.md` |
| **上下文** | 独立的 subagent 进程 | 内联（主对话） | 内联（主对话） |
| **用户可调用** | 否 `/` 菜单 — 由 Claude 或 Agent 工具调用 | 是 — `/command-name` | 是 — `/skill-name`（除非 `user-invocable: false`） |
| **由 Claude 自动调用** | 是 — 通过 `description` 字段 | 否 | 是 — 通过 `description` 字段（除非 `disable-model-invocation: true`） |
| **接受参数** | 通过 `prompt` 参数 | `$ARGUMENTS`、`$0`、`$1` | `$ARGUMENTS`、`$0`、`$1` |
| **动态上下文注入** | 否 | 是 — `` !`command` `` | 是 — `` !`command` `` |
| **自己的上下文窗口** | 是 — 隔离的 | 否 — 共享主上下文 | 否 — 共享主上下文（除非 `context: fork`） |
| **模型覆盖** | `model:` frontmatter | `model:` frontmatter | `model:` frontmatter |
| **工具限制** | `tools:` / `disallowedTools:` | `allowed-tools:` | `allowed-tools:` |
| **Hooks** | `hooks:` frontmatter | — | `hooks:` frontmatter |
| **Memory** | `memory:` frontmatter（user/project/local） | — | — |
| **可以预加载技能** | 是 — `skills:` frontmatter | — | — |
| **MCP 服务器** | `mcpServers:` frontmatter | — | — |

---

## 何时使用各

### 在以下情况使用 Agent：

- 任务**自主且多步骤** — agent 需要探索、决定并在没有持续指导的情况下行动
- 您需要**上下文隔离** — 工作不应污染主对话窗口
- agent 需要**跨会话的持久记忆**（例如，学习模式的代码审查者）
- 您想通过技能**预加载领域知识**而不杂乱主上下文
- 任务受益于**在后台运行**或在 **git worktree** 中运行
- 您需要**工具限制**或不同的**权限模式**（例如 `acceptEdits`、`plan`）

**示例**：`weather-agent` — 使用其预加载的 `weather-fetcher` 技能自主获取天气数据，在具有受限工具的独立上下文中运行。

### 在以下情况使用 Command：

- 您需要**用户启动的入口点** — 用户明确触发的工作流
- 工作流涉及**编排**其他 agents 或技能
- 您想**保持上下文精简** — 命令内容在用户触发之前不注入到会话上下文中

**示例**：`weather-orchestrator` — 用户触发它，询问摄氏/华氏偏好，调用 agent，然后调用 SVG 技能。

### 在以下情况使用 Skill：

- 您希望**Claude 根据用户意图自动调用** — 技能描述被注入到会话上下文中进行语义匹配
- 任务是可以从多个地方调用的**可重用过程**（命令、agents 或 Claude 本身）
- 您需要**agent 预加载** — 在启动时将领域知识烘焙到特定 agent 中

**示例**：`weather-svg-creator` — 当用户请求天气卡片时 Claude 自动调用它；也可以从命令调用。

---

## Command → Agent → Skill 架构

此仓库展示了一种分层编排模式：

```
用户触发 /command
    ↓
命令编排工作流
    ↓
命令调用 Agent（独立上下文，自主）
    ↓
Agent 使用预加载的技能（领域知识）
    ↓
命令调用 Skill（内联，用于输出生成）
```

**具体示例** — 天气系统：

```
/weather-orchestrator（命令 — 入口点，询问℃/℉）
    ↓
weather-agent（agent — 自主获取温度）
    ├── weather-fetcher（agent 技能 — 预加载 API 说明）
    ↓
weather-svg-creator（技能 — 内联创建 SVG）
```

---

## Frontmatter 比较

### Agent Frontmatter

```yaml
---
name: my-agent
description: Use this agent PROACTIVELY when...
tools: Read, Write, Edit, Bash
model: sonnet
maxTurns: 10
permissionMode: acceptEdits
memory: user
skills:
  - my-skill
---
```

### Command Frontmatter

```yaml
---
description: Do something useful
argument-hint: [issue-number]
allowed-tools: Read, Edit, Bash(gh *)
model: sonnet
---
```

### Skill Frontmatter

```yaml
---
name: my-skill
description: Do something when the user asks for...
argument-hint: [file-path]
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, Glob
model: sonnet
context: fork
agent: general-purpose
---
```

---

## 关键区别

### 自动调用

| 机制 | Claude 可以自动调用？ | 如何阻止 |
|-----------|------------------------|----------------|
| Agent | 是 — 通过 `description`（使用 "PROACTIVELY" 鼓励它） | 删除或软化描述 |
| Command | 否 — 始终通过 `/` 由用户启动 | 不适用 |
| Skill | 是 — 通过 `description` | 设置 `disable-model-invocation: true` |

### 在 `/` 菜单中的可见性

| 机制 | 出现在 `/` 菜单中？ | 如何隐藏 |
|-----------|---------------------|-------------|
| Agent | 否 | 不适用 |
| Command | 是 — 始终 | 无法隐藏 |
| Skill | 是 — 默认 | 设置 `user-invocable: false` |

### 上下文隔离

| 机制 | 在自己的上下文中运行？ | 如何配置 |
|-----------|---------------------|-----------------|
| Agent | 始终 | 内置行为 |
| Command | 从不 | 不适用 |
| Skill | 可选 | 设置 `context: fork` |

---

## 工作示例："现在几点？"

此仓库为同一任务定义了三种机制 — 显示巴基斯坦标准时间的当前时间。以下是当用户输入"现在几点？"而不显式调用任何 `/` 命令时会发生什么：

| 机制 | 会触发？ | 为什么/为什么不 |
|-----------|--------------|---------------|
| `time-command` | 否 | 命令**永远不会自动触发**。用户需要显式输入 `/time-command` 才能运行。命令没有自动发现路径 — 它们严格由用户启动。 |
| `time-agent` | **是**（可能） | agent 的 `description` 说*"使用此 agent 显示巴基斯坦标准时间的当前时间"*。Claude 将此与用户意图匹配并可能通过 Agent 工具生成它。然而，agents 在**独立的上下文窗口**中运行，使其对于这个简单任务比必要的更重。 |
| `time-skill` | **是**（最可能） | skill 的 `description` 说*"显示巴基斯坦标准时间（PKT，UTC+5）。当用户询问当前时间、巴基斯坦时间或 PKT 时使用"*。Claude 匹配并通过 Skill 工具调用它。由于它在**内联**运行且没有上下文开销，这是最有效的匹配。 |

### 解决顺序

当多个机制匹配同一意图时，Claude 首选满足请求的**最轻量级选项**：

```
1. Skill（内联，无上下文开销）     ← 首选
2. Agent（独立上下文，自主）    ← 如果技能不可用或任务复杂则使用
3. Command（从不 — 需要显式 /）   ← 仅当用户输入 /time-command
```

如果该技能设置了 `disable-model-invocation: true` 会怎样？

那么 Claude **无法**自动调用该技能。agent 成为唯一可自动调用的选项，因此 Claude 将生成 `time-agent` — 代价是为一个单行 bash 命令使用���立��上下文窗口。

如果技能和 agent 都禁用了自动调用会怎样？

那么**没有任何东西自动触发**。Claude 将回退到其自己的一般知识并可能直接运行 `TZ='Asia/Karachi' date` — 不涉及扩展机制。用户需要显式输入 `/time-command` 或 `/time-skill` 来使用其中一个。

![Claude auto-invoking time-skill when user asks "What is the current time?"](assets/agent-command-skill-2.png)

---

## 来源

- [Claude Code Skills — 文档](https://code.claude.com/docs/en/skills)
- [Claude Code Sub-agents — 文档](https://code.claude.com/docs/en/sub-agents)
- [Claude Code Slash Commands — 文档](https://code.claude.com/docs/en/slash-commands)
- [Skills 最佳实践](../best-practice/claude-skills.md)
- [Commands 最佳实践](../best-practice/claude-commands.md)
- [Sub-agents 最佳实践](../best-practice/claude-subagents.md)