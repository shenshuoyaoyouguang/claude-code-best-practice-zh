# Claude Code：全局 vs 项目级功能

Claude Code 中哪些功能是全局独有（`~/.claude/`）vs 哪些同时有全局和项目级（`.claude/`）等价物的全面比较。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 目录

1. [概述](#概述)
2. [全局独有功能](#全局独有功能)
3. [双范围功能](#双范围功能)
4. [设置优先级](#设置优先级)
5. [目录结构比较](#目录结构比较)
6. [任务系统](#任务系统)
7. [Agent 团队](#agent-团队)
8. [设计原则](#设计原则)
9. [来源](#来源)

---

## 概述

Claude Code 使用**范围层级**，其中一些功能同时存在于全局（`~/.claude/`）和项目（`.claude/`）级别，而其他则是纯全局的。设计原则：*个人状态*或*跨项目协调*住在全局；*团队可共享的项目配置*可以住在项目级别。

- `~/.claude/` 是您的**用户级主目录**（全局，所有项目）
- `.claude/` 在仓库内是您的**项目级主目录**（仅限于该项目）

---

## 全局独有功能

这些**仅**住在 `~/.claude/` 下，不能限定到项目：

| 功能 | 位置 | 目的 |
|---------|----------|---------|
| **任务** | `~/.claude/tasks/` | 跨会话和 agents 的持久任务列表 |
| **Agent 团队** | `~/.claude/teams/` | 多 agent 协调配置（实验性，2026 年 2 月） |
| **Auto Memory** | `~/.claude/projects/<hash>/memory/` | 每个项目的 Claude 自我学习（个人，永不共享） |
| **凭证和 OAuth** | 系统 keychain + `~/.claude.json` | API key、OAuth token（永不在项目文件中） |
| **键盘绑定** | `~/.claude/keybindings.json` | 自定义键盘快捷键 |
| **MCP 用户服务器** | `~/.claude.json`（`mcpServers` 键） | 跨所有项目的个人 MCP 服务器 |
| **偏好/缓存** | `~/.claude.json` | 主题、模型、输出样式、会话状态 |

---

## 双范围功能

这些同时存在于两个级别，**项目级优先**于全局：

| 功能 | 全局（`~/.claude/`） | 项目（`.claude/`） | 优先级 |
|---------|----------------------|---------------------|------------|
| **CLAUDE.md** | `~/.claude/CLAUDE.md` | `./CLAUDE.md` 或 `.claude/CLAUDE.md` | 项目覆盖全局 |
| **设置** | `~/.claude/settings.json` | `.claude/settings.json` + `.claude/settings.local.json` | 项目 > 全局 |
| **规则** | `~/.claude/rules/*.md` | `.claude/rules/*.md` | 项目覆盖 |
| **Agents/Subagents** | `~/.claude/agents/*.md` | `.claude/agents/*.md` | 项目覆盖 |
| **Commands** | `~/.claude/commands/*.md` | `.claude/commands/*.md` | 两者可用 |
| **Skills** | `~/.claude/skills/` | `.claude/skills/` | 两者可用 |
| **Hooks** | `~/.claude/hooks/` | `.claude/hooks/` | 两者执行 |
| **MCP 服务器** | `~/.claude.json`（用户范围） | `.mcp.json`（项目范围） | 三范围：本地 > 项目 > 用户 |

---

## 设置优先级

用户可写设置按此覆盖顺序应用（从高到低）：

| 优先级 | 位置 | 范围 | 版本控制 | 目的 |
|----------|----------|-------|-----------------|---------|
| 1 | 命令行标志 | 会话 | 不适用 | 单会话覆盖 |
| 2 | `.claude/settings.local.json` | 项目 | 否（git 忽略） | 个人项目特定 |
| 3 | `.claude/settings.json` | 项目 | 是（已提交） | 团队共享设置 |
| 4 | `~/.claude/settings.local.json` | 用户 | 不适用 | 个人全局覆盖 |
| 5 | `~/.claude/settings.json` | 用户 | 不适用 | 全局个人设置 |

策略层：`managed-settings.json` 是组织强制的，不能被本地文件覆盖。

**重要**：`deny` 规则具有最高安全优先级，不能被较低优先级的 allow/ask 规则覆盖。

---

## 目录结构比较

### 全局范围（`~/.claude/`）

```
~/.claude/
├── settings.json              # 用户级设置（所有项目）
├── settings.local.json        # 个人覆盖
├── CLAUDE.md                  # 用户记忆（所有项目）
├── agents/                    # 用户 subagents（所有项目可用）
│   └── *.md
├── rules/                     # 用户级模块化规则
│   └── *.md
├── commands/                  # 用户级命令
│   └── *.md
├── skills/                   # 用户级技能
│   └── */
├── hooks/                    # 用户级 hooks
├── teams/                    # 用户级 agent 团队
├── tasks/                    # 持久任务列表
├── keybindings.json         # 键盘快捷键
└── projects/
    └── <project-hash>/
        └── memory/          # auto-memory
```

### 项目范围（`.claude/`）

```
.claude/
├── settings.json            # 项目设置（团队共享）
├── settings.local.json      # 个人覆盖（git 忽略）
├── hooks/                   # 项目 hooks
│   └── *
├── agents/                  # 项目 subagents
│   └── *.md
├── commands/                # 项目命令
│   └── *.md
├── skills/                  # 项目技能
│   └── */
├── rules/                    # 项目规则
│   └── *.md
├── agent-memory/            # 项目范围 agent memory
├── agent-memory-local/     # 本地范围 agent memory（git 忽略）
└── MEMORY.md                # 项目规则（可选）
```

---

## 任务系统

任务存储在 `~/.claude/tasks/` 下，结构为：

```
~/.claude/tasks/
├── my-todo-list/
│   ├── TODO.md
│   └── completed.md
└── another-list/
    └── TODO.md
```

任务与 agent 团队配合使用。详见 [Claude Code 任务文档](https://code.claude.com/docs/en/tasks)。

---

## Agent 团队

Agent 团队存储在 `~/.claude/teams/` 下：

```
~/.claude/teams/
└── my-team/
    ├── team.md
    └── agents/
        ├── researcher.md
        writer.md
        reviewer.md
```

详见 [Claude Code Agent 团队文档](https://code.claude.com/docs/en/agent-teams)。

---

## 设计原则

**全局物品**：
- 个人状态（例如主题、输出样式偏好）
- 跨项目协调（例如常用 subagents、技能）
- 敏感凭证（API key、OAuth token）
- 跨会话持久数据（任务、团队）

**项目级物品**：
- 团队可共享配置（设置、agents、技能、hooks）
- 项目特定规则（CLAUDE.md、代码约定）
- 团队约定（归属消息格式、权限规则）

**分层原则**：
- 设置：本地 > 项目 > 用户 > 全局
- 记忆：agent 特定 > 项目 > 用户
- Hooks：所有范围级联执行

---

## 来源

- [Claude Code 设置文档](https://code.claude.com/docs/en/settings)
- [Claude Code 记忆文档](https://code.claude.com/docs/en/memory)
- [Claude Code Agent 团队文档](https://code.claude.com/docs/en/agent-teams)
- [Claude Code 任务文档](https://code.claude.com/docs/en/tasks)