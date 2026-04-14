# Agent 团队实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_12%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#time-orchestration"><img src="../!/tags/implemented-hd.svg" alt="已实现"></a>

<p align="center">
  <img src="assets/impl-agent-teams.png" alt="Agent Teams in action — split pane mode with tmux" width="100%">
</p>

Agent 团队生成多个独立的 Claude Code 会话，通过共享任务列表进行协调。与子代理（一个会话内的隔离上下文分支）不同，每个团队成员都有自己完整的上下文窗口，自动加载 CLAUDE.md、MCP 服务器和技能。

---

## 使用方法

时间编排工作流完全由 agent 团队构建。要运行完成的产品：

```bash
cd agent-teams
claude
/time-orchestrator
```

这会调用命令 → Agent → 技能管道：agent 获取迪拜的当前时间，技能将 SVG 时间卡渲染到 agent-teams/output/dubai-time.svg。

---

## 实现方法

您可以使用 agent 团队创建天气编排工作流的副本 — 在本例中，时间编排工作流完全由 agent 团队构建。

### 1. 安装 iTerm2 和 tmux

```bash
brew install --cask iterm2
brew install tmux
```

### 2. 启动 iTerm2 → tmux → Claude

```bash
tmux new -s dev
CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1 claude
```

### 3. 使用团队结构提示

<a id="time-orchestration"></a>

将此提示粘贴到 Claude 中以使用 agent 团队引导完整的时间编排工作流：

主提示：agent-teams-prompt.md

### 团队协调流程

```
┌──────────────────────────────────────────────────────────────┐
│                         LEAD (你)                            │
│       "创建一个 agent 团队来构建时间编排"                      │
└──────────────────────────┬───────────────────────────────────┘
                           │ 生成团队（全部并行）
              ┌────────────┼────────────┐
              ▼            ▼            ▼
   ┌────────────────┐ ┌──────────┐ ┌──────────────┐
   │ 命令            │ │ Agent    │ │ 技能          │
   │ 架构师          │ │ 工程师   │ │ 设计师        │
   │                │ │          │ │              │
   │ agent-teams/   │ │ agent-   │ │ agent-teams/ │
   │ .claude/       │ │ teams/   │ │ .claude/     │
   │ commands/      │ │ .claude/ │ │ skills/      │
   │ time-          │ │ agents/  │ │ time-svg-    │
   │ orchestrator.md│ │ time-    │ │ creator/     │
   │                │ │ agent.md │ │              │
   └───────┬────────┘ └────┬─────┘ └──────┬───────┘
           │               │              │
           ▼               ▼              ▼
   ┌──────────────────────────────────────────────────┐
   │            共享任务列表                           │
   │  ☐ 同意数据契约: {time, tz, formatted}          │
   │  ☐ 命令使用 Agent 工具（非 bash）               │
   │  ☐ Agent 预加载 time-fetcher 技能                │
   │  ☐ 技能从上下文读取时间（不再重新获取）           │
   │  ☐ 所有文件位于 agent-teams/.claude/            │
   └──────────────────────────────────────────────────┘
                       │
                       ▼
          ┌──────────────────────────────┐
          │  cd agent-teams && claude   │
          │    /time-orchestrator       │
          │   命令 → Agent → 技能       │
          └──────────────────────────────┘
```