# 子代理最佳实践

![Last Updated](https://img.shields.io/badge/Last_Updated-Apr%2013%2C%202026%208%3A02%20PM%20PKT-white?style=flat&labelColor=555) ![Version](https://img.shields.io/badge/Claude_Code-v2.1.101-blue?style=flat&labelColor=555)<br>
[![Implemented](https://img.shields.io/badge/Implemented-2ea44f?style=flat)](../implementation/claude-subagents-implementation.md)

Claude Code 子代理 — frontmatter 字段和官方内置代理类型。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Frontmatter 字段 (16)

| 字段 | 类型 | 必填 | 描述 |
|-------|------|----------|-------------|
| `name` | string | 是 | 使用小写字母和连字符的唯一标识符 |
| `description` | string | 是 | 何时调用。使用 `"PROACTIVELY"` 让 Claude 自动调用 |
| `tools` | string/list | 否 | 逗号分隔的工具允许列表（例如 `Read, Write, Edit, Bash`）。如果省略则继承所有工具。支持 `Agent(agent_type)` 语法来限制可生成的子代理；旧的 `Task(agent_type)` 别名仍然有效 |
| `disallowedTools` | string/list | 否 | 要拒绝的工具，从继承或指定的列表中移除 |
| `model` | string | 否 | 使用的模型：`sonnet`、`opus`、`haiku`、完整模型 ID（例如 `claude-opus-4-6`）或 `inherit`（默认：`inherit`） |
| `permissionMode` | string | 否 | 权限模式：`default`、`acceptEdits`、`auto`、`dontAsk`、`bypassPermissions` 或 `plan` |
| `maxTurns` | integer | 否 | 子代理停止前的最大代理轮次数 |
| `skills` | list | 否 | 启动时预加载到代理上下文的技能名称（注入完整内容，而不仅仅是可用） |
| `mcpServers` | list | 否 | 此子代理的 MCP 服务器 — 服务器名称字符串或内联 `{name: config}` 对象 |
| `hooks` | object | 否 | 作用于此子代理的生命周期钩子。支持所有钩子事件；`PreToolUse`、`PostToolUse` 和 `Stop` 是最常见的 |
| `memory` | string | 否 | 持久化内存范围：`user`、`project` 或 `local` |
| `background` | boolean | 否 | 设为 `true` 始终作为后台任务运行（默认：`false`） |
| `effort` | string | 否 | 此子代理激活时的努力级别覆盖：`low`、`medium`、`high`、`max`（仅 Opus 4.6）。默认：从会话继承 |
| `isolation` | string | 否 | 设为 `"worktree"` 在临时 git 工作树中运行（如果没有更改则自动清理） |
| `initialPrompt` | string | 否 | 当此代理作为主会话代理运行时（通过 `--agent` 或 `agent` 设置），自动作为第一个用户轮次提交。处理命令和技能。附加到任何用户提供的提示之前 |
| `color` | string | 否 | 子代理在任务列表和记录中显示的颜色：`red`、`blue`、`green`、`yellow`、`purple`、`orange`、`pink` 或 `cyan` |

---

## ![Official](../!/tags/official.svg) **(5)**

| # | 代理 | 模型 | 工具 | 描述 |
|---|-------|-------|-------|-------------|
| 1 | `general-purpose` | inherit | 所有 | 复杂的多步骤任务 — 研究、代码搜索和自主工作的默认代理类型 |
| 2 | `Explore` | haiku | 只读（无 Write、Edit） | 快速代码库搜索和探索 — 优化用于查找文件、搜索代码和回答代码库问题 |
| 3 | `Plan` | inherit | 只读（无 Write、Edit） | 计划模式中的预规划研究 — 在编写代码之前探索代码库并设计实现方法 |
| 4 | `statusline-setup` | sonnet | Read, Edit | 配置用户的 Claude Code 状态行设置 |
| 5 | `claude-code-guide` | haiku | Glob, Grep, Read, WebFetch, WebSearch | 回答有关 Claude Code 功能、Agent SDK 和 Claude API 的问题 |

---

## 来源

- [创建自定义子代理 — Claude Code 文档](https://code.claude.com/docs/en/sub-agents)
- [CLI 参考 — Claude Code 文档](https://code.claude.com/docs/en/cli-reference)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)