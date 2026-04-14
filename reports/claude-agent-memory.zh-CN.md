# Claude Code：Agent Memory Frontmatter

Subagent 的持久化记忆 — 使 agent 能够跨会话学习、记忆和构建知识。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 概述

在 **Claude Code v2.1.33**（2026 年 2 月）引入，`memory` frontmatter 字段赋予每个 subagent 其自己的持久化基于 markdown 的知识库。在此之前，每次 agent 调用都是从零开始。

```yaml
---
name: code-reviewer
description: Reviews code for quality and best practices
tools: Read, Write, Edit, Bash
model: sonnet
memory: user
---

You are a code reviewer. As you review code, update your agent memory with
patterns, conventions, and recurring issues you discover.
```

---

## 记忆范围

| 范围 | 存储位置 | 版本控制 | 共享 | 适用于 |
|-------|-----------------|-------------------|--------|----------|
| `user` | `~/.claude/agent-memory/<agent-name>/` | 否 | 否 | 跨项目知识（推荐默认） |
| `project` | `.claude/agent-memory/<agent-name>/` | 是 | 是 | 团队应共享的特定项目知识 |
| `local` | `.claude/agent-memory-local/<agent-name>/` | 否（git 忽略） | 否 | 特定于项目的个人知识 |

这些范围镜像设置层级（`~/.claude/settings.json` → `.claude/settings.json` → `.claude/settings.local.json`）。

---

## 工作原理

1. **启动时**：`MEMORY.md` 的前 200 行被注入到 agent 的系统提示中
2. **工具访问**：`Read`、`Write`、`Edit` 自动启用以便 agent 可以管理其记忆
3. **执行期间**：agent 可以自由读写其记忆目录
4. **策展**：如果 `MEMORY.md` 超过 200 行，agent 将细节移入主题特定的文件

```
~/.claude/agent-memory/code-reviewer/     # user 范围示例
├── MEMORY.md                              # 主文件（前 200 行加载）
├── react-patterns.md                      # 主题特定文件
└── security-checklist.md                  # 主题特定文件
```

---

## Agent Memory 与其他记忆系统

| 系统 | 谁写入 | 谁读取 | 范围 |
|--------|--------|-----------|-------|
| **CLAUDE.md** | 您（手动） | 主 Claude + 所有 agents | 项目 |
| **Auto-memory** | 主 Claude（自动） | 主 Claude 仅 | 每个项目每个用户 |
| **`/memory` 命令** | 您（通过编辑器） | 主 Claude 仅 | 每个项目每个用户 |
| **Agent memory** | agent 本身 | 该特定 agent | 可配置（user/project/local） |

这些系统是**互补的** — agent 读取 CLAUDE.md（项目上下文）和它自己的记忆（agent 特定知识）。

---

## 实用示例

```yaml
---
name: api-developer
description: Implement API endpoints following team conventions
tools: Read, Write, Edit, Bash
model: sonnet
memory: project
skills:
  - api-conventions
  - error-handling-patterns
---

Implement API endpoints. Follow the conventions from your preloaded skills.
As you work, save architectural decisions and patterns to your memory.
```

这将**技能**（启动时的静态知识）与**记忆**（随时间构建的动态知识）结合。

---

## 技巧

- **提示记忆使用** — 包含明确指令："启动前，查看您的记忆。完成后，用您学到的更新您的记忆。"
- **在调用 agents 时请求记忆检查**："审查此 PR，并检查您之前见过的模式您的记忆。"
- **选择正确的范围** — `user` 用于跨项目，`project` 用于团队共享，`local` 用于个人

---

## 来源

- [创建自定义 subagents — Claude Code 文档](https://code.claude.com/docs/en/sub-agents)
- [管理 Claude 的记忆 — Claude Code 文档](https://code.claude.com/docs/en/memory)
- [Claude Code v2.1.33 发布说明](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)