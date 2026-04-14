# CLAUDE.md 中文版

本文档为 Claude Code (claude.ai/code) 在本项目中工作时提供指导。

## 项目概述

这是 Claude Code 配置的最佳实践仓库，展示了 skills（技能）、subagents（子代理）、hooks（钩子）和 commands（命令）的模式。作为参考实现而非应用程序代码库。

## 核心组件

### 天气系统（工作流示例）
通过 **Command → Agent → Skill** 架构展示两种不同的技能模式：
- `/weather-orchestrator` command（命令）(`.claude/commands/weather-orchestrator.md`）：入口点 — 询问用户摄氏/华氏温度单位，调用 agent，然后调用 SVG 技能
- `weather-agent` agent（代理）(`.claude/agents/weather-agent.md`）：使用预加载的 `weather-fetcher` skill（技能）获取温度（agent skills 模式）
- `weather-fetcher` skill（技能）(`.claude/skills/weather-fetcher/SKILL.md`）：预加载到 agent — 从 Open-Meteo 获取温度的说明
- `weather-svg-creator` skill（技能）(`.claude/skills/weather-svg-creator/SKILL.md`）：创建 SVG 天气卡片，写入 `orchestration-workflow/weather.svg` 和 `orchestration-workflow/output.md`

两种技能模式：agent skills（通过 `skills:` 字段预加载）vs skills（通过 Skill 工具调用）。详见 `orchestration-workflow/orchestration-workflow.md` 的完整流程图。

### 技能定义结构
`.claude/skills/<name>/SKILL.md` 中的技能使用 YAML frontmatter：
- `name`：显示名称和 `/斜杠命令`（默认取目录名）
- `description`：何时调用（推荐用于自动发现）
- `argument-hint`：自动补全提示（例如 `[issue-number]`）
- `disable-model-invocation`：设为 `true` 阻止自动调用
- `user-invocable`：设为 `false` 从 `/` 菜单隐藏（仅后台知识）
- `allowed-tools`：技能激活时无需权限提示即可使用的工具
- `model`：技能激活时使用的模型
- `context`：设为 `fork` 在隔离的 subagent 环境中运行
- `agent`：`context: fork` 的 subagent 类型（默认 `general-purpose`）
- `hooks`：作用于此技能的生命周期钩子

### 演示系统
详见 `.claude/rules/presentation.md` — 所有演示工作委托给 `presentation-curator` agent。

### 钩子系统
`.claude/hooks/` 中的跨平台声音通知系统：
- `scripts/hooks.py`：Claude Code 钩子事件的主处理器
- `config/hooks-config.json`：团队共享配置
- `config/hooks-config.local.json`：个人覆盖（git 忽略）
- `sounds/`：按钩子事件组织的音频文件（通过 ElevenLabs TTS 生成）

`.claude/settings.json` 中配置的钩子事件：PreToolUse、PostToolUse、UserPromptSubmit、Notification、Stop、SubagentStart、SubagentStop、PreCompact、SessionStart、SessionEnd、Setup、PermissionRequest、TeammateIdle、TaskCompleted、ConfigChange。

特殊处理：git 提交触发 `pretooluse-git-committing` 声音。

## 关键模式

### Subagent 编排
Subagents **不能** 通过 bash 命令调用其他 subagents。使用 Agent 工具（v2.1.63 中从 Task 重命名；`Task(...)` 仍作为别名可用）：
```
Agent(subagent_type="agent-name", description="...", prompt="...", model="haiku")
```

在 subagent 定义中明确工具使用。避免使用"launch"等可能被误解为 bash 命令的模糊术语。

### Subagent 定义结构
`.claude/agents/*.md` 中的 subagents 使用 YAML frontmatter：
- `name`：Subagent 标识符
- `description`：何时调用（使用 "PROACTIVELY" 用于自动调用）
- `tools`：逗号分隔的工具允许列表（如省略则继承全部）。支持 `Agent(agent_type)` 语法
- `disallowedTools`：要拒绝的工具，从继承或指定的列表中移除
- `model`：模型别名：`haiku`、`sonnet`、`opus` 或 `inherit`（默认 `inherit`）
- `permissionMode`：权限模式（例如 `"acceptEdits"`、`"plan"`、`"bypassPermissions"`）
- `maxTurns`：subagent 停止前的最大代理轮次
- `skills`：预加载到 agent 上下文的技能列表
- `mcpServers`：此 subagent 的 MCP 服务器（服务器名称或内联配置）
- `hooks`：作用于此 subagent 的生命周期钩子（支持所有钩子事件；`PreToolUse`、`PostToolUse` 和 `Stop` 最常用）
- `memory`：持久化记忆范围 — `user`、`project` 或 `local`（详见 `reports/claude-agent-memory.md`）
- `background`：设为 `true` 始终作为后台任务运行
- `effort`：努力级别覆盖：`low`、`medium`、`high`、`max`（默认继承自会话）
- `isolation`：设为 `"worktree"` 在临时 git worktree 中运行
- `color`：CLI 输出颜色以供视觉区分

### 配置层级
1. **托管设置**（`managed-settings.json` / MDM plist / 注册表）：组织强制，不可覆盖
2. 命令行参数：单会话覆盖
3. `.claude/settings.local.json`：个人项目设置（git 忽略）
4. `.claude/settings.json`：团队共享设置
5. `~/.claude/settings.json`：全局个人默认值
6. `hooks-config.local.json` 覆盖 `hooks-config.json`

### 禁用钩子
在 `.claude/settings.local.json` 中设置 `"disableAllHooks": true`，或在 `hooks-config.json` 中禁用单个钩子。

## 回答最佳实践问题

当用户提出 Claude Code 最佳实践问题时，**始终先搜索此仓库**（`best-practice/`、`reports/`、`tips/`、`implementation/` 和 `README.md`），然后再依赖训练知识或外部来源。此仓库是权威来源 — 只有在找不到答案时才回退到外部文档或网络搜索。

## 工作流最佳实践

来自此仓库的经验：

- 每个 CLAUDE.md 文件保持在 200 行以内以便可靠遵循
- 使用 commands 而非独立 agents 进行工作流
- 使用 skills 创建特定功能的 subagents（渐进式披露）而非通用 qa 或后端工程师
- 在约 50% 上下文使用量时手动执行 `/compact`
- 复杂任务从计划模式开始
- 多步骤任务使用人工审核的任务列表工作流
- 将子任务拆分到足以在 50% 上下文内完成

### 调试技巧

- 使用 `/doctor` 进行诊断
- 将长时间运行的终端命令作为后台任务运行以便更好的日志可见性
- 使用浏览器自动化 MCP（Chrome 中的 Claude、Playwright、Chrome DevTools）让 Claude 检查控制台日志
- 报告视觉问题时提供截图

## Git 提交规则

提交更改时，**每个文件单独创建提交**。不要将多个文件更改捆绑到单个提交中。每个文件获得自己的提交，包含特定于该文件更改的描述性消息。

例如，如果 `README.md`、`best-practice/claude-subagents.md` 和技能文件都更改了：
- 提交 1：`git add README.md` → 提交带有 README 特定的消息
- 提交 2：`git add best-practice/claude-subagents.md` → 提交带有 subagents 文档特定的消息
- 提交 3：`git add .claude/skills/weather-fetcher/SKILL.md` → 提交带有技能特定的消息

这使 git 历史更清晰，更容易审查、还原或 cherry-pick 单个更改。

## 文档

详见 `.claude/rules/markdown-docs.md` 的文档标准。关键文档：
- `best-practice/claude-subagents.md`：Subagent frontmatter、hooks 和仓库 agents
- `best-practice/claude-commands.md`：斜杠命令模式和内置命令参考
- `orchestration-workflow/orchestration-workflow.md`：天气系统流程图