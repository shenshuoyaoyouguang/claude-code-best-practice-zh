# 编排工作流

本文档描述了命令 → Agent（带技能）→ 技能编排工作流，通过天气数据获取和 SVG 渲染系统进行演示。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 系统概述

天气系统演示了单个编排工作流中的两种不同技能模式：
- Agent 技能（预加载）：weather-fetcher 作为领域知识在启动时注入到 weather-agent
- 技能（独立）：weather-svg-creator 通过 Skill 工具由命令直接调用

这展示了命令 → Agent → 技能架构模式，其中：
- 命令编排工作流并处理用户交互
- Agent 使用其预加载技能获取数据
- 技能独立创建可视化输出

## 组件摘要

| 组件 | 角色 | 示例 |
|-----------|------|---------|
| 命令 | 入口点，用户交互 | /weather-orchestrator |
| Agent | 使用预加载技能获取数据 | weather-agent 配合 weather-fetcher |
| 技能 | 独立创建输出 | weather-svg-creator |

## 流程图

```
╔══════════════════════════════════════════════════════════════════╗
║              ORCHESTRATION WORKFLOW                              ║
║           Command  →  Agent  →  Skill                            ║
╚══════════════════════════════════════════════════════════════════╝

                         ┌───────────────────┐
                         │  User Interaction │
                         └─────────┬─────────┘
                                   │
                                   ▼
         ┌─────────────────────────────────────────────────────┐
         │  /weather-orchestrator — Command (Entry Point)      │
         └─────────────────────────┬───────────────────────────┘
                                   │
                              Step 1
                                   │
                                   ▼
                      ┌────────────────────────┐
                      │  AskUser — C° or F°?   │
                      └────────────┬───────────┘
                                   │
                         Step 2 — Agent tool
                                   │
                                   ▼
         ┌─────────────────────────────────────────────────────┐
         │  weather-agent — Agent ● skill: weather-fetcher     │
         └─────────────────────────┬───────────────────────────┘
                                   │
                          Returns: temp + unit
                                   │
                         Step 3 — Skill tool
                                   │
                                   ▼
         ┌─────────────────────────────────────────────────────┐
         │  weather-svg-creator — Skill ● SVG card + output    │
         └─────────────────────────┬───────────────────────────┘
                                   │
                          ┌────────┴────────┐
                          │                 │
                          ▼                 ▼
                   ┌────────────┐    ┌────────────┐
                   │weather.svg │    │ output.md  │
                   └────────────┘    └────────────┘
```

## 组件详情

### 1. 命令

#### /weather-orchestrator（命令）
- 位置：.claude/commands/weather-orchestrator.md
- 目的：入口点 — 编排工作流并处理用户交互
- 操作：
  1. 询问用户温度单位偏好（摄氏度/华氏度）
  2. 通过 Agent 工具调用 weather-agent
  3. 通过 Skill 工具调用 weather-svg-creator
- 模型：haiku

### 2. 带预加载技能的 Agent（Agent 技能）

#### weather-agent（Agent）
- 位置：.claude/agents/weather-agent.md
- 目的：使用其预加载技能获取天气数据
- 技能：weather-fetcher（作为领域知识预加载）
- 可用工具：WebFetch, Read
- 模型：sonnet
- 颜色：green
- 内存：project

代理在启动时将 weather-fetcher 预加载到其上下文中。它遵循技能的说明获取温度并将值返回给命令。

### 3. 技能

#### weather-svg-creator（技能）
- 位置：.claude/skills/weather-svg-creator/SKILL.md
- 目的：创建可视化 SVG 天气卡并写入输出文件
- 调用方式：通过命令的 Skill 工具（不预加载到任何 agent）
- 输出：
  - orchestration-workflow/weather.svg — SVG 天气卡
  - orchestration-workflow/output.md — 天气摘要

### 4. 预加载技能

#### weather-fetcher（技能）
- 位置：.claude/skills/weather-fetcher/SKILL.md
- 目的：获取实时温度数据的说明
- 数据来源：Open-Meteo API（迪拜，阿联酋）
- 输出：温度值和单位（摄氏度或华氏度）
- 注意：这是一个 agent 技能 — 预加载到 weather-agent，不直接调用

## 执行流程

1. 用户调用：用户运行 /weather-orchestrator 命令
2. 用户提示：命令询问用户首选温度单位（摄氏度/华氏度）
3. Agent 调用：命令通过 Agent 工具调用 weather-agent
4. 技能执行（在 agent 上下文中）：
   - agent 遵循 weather-fetcher 技能说明从 Open-Meteo 获取温度
   - agent 将温度值和单位返回给命令
5. SVG 创建：命令通过 Skill 工具调用 weather-svg-creator
   - 技能在 orchestration-workflow/weather.svg 创建 SVG 天气卡
   - 技能在 orchestration-workflow/output.md 写入摘要
6. 结果显示：向用户显示摘要，包括：
   - 请求的温度单位
   - 获取的温度
   - SVG 卡位置
   - 输出文件位置

## 示例执行

```
输入: /weather-orchestrator
├─ Step 1: 询问：摄氏度还是华氏度？
│  └─ 用户：摄氏度
├─ Step 2: Agent 工具 → weather-agent
│  ├─ 预加载技能：
│  │  └─ weather-fetcher（领域知识）
│  ├─ 从 Open-Meteo 获取 → 26°C
│  └─ 返回：temperature=26, unit=Celsius
├─ Step 3: Skill 工具 → /weather-svg-creator
│  ├─ 创建：orchestration-workflow/weather.svg
│  └─ 写入：orchestration-workflow/output.md
└─ 输出：
   ├─ 单位：Celsius
   ├─ 温度：26°C
   ├─ SVG：orchestration-workflow/weather.svg
   └─ 摘要：orchestration-workflow/output.md
```

## 关键设计原则

1. 两种技能模式：演示了 agent 技能（预加载）和技能（直接调用）
2. 命令作为编排器：命令处理用户交互并协调工作流
3. Agent 用于数据获取：agent 使用其预loaded技能获取数据，然后返回
4. 技能用于输出：SVG 创建器独立运行，从命令上下文接收数据
5. 清晰分离：获取（agent）→ 渲染（skill）— 每个组件都有单一职责

## 架构模式

### Agent 技能（预加载）

```yaml
# 在 agent 定义中（.claude/agents/weather-agent.md）
---
name: weather-agent
skills:
  - weather-fetcher    # 在启动时预加载到 agent 上下文
---
```

- 技能预加载：完整技能内容在启动时注入到 agent 的上下文中
- Agent 使用技能知识：agent 遵循预加载技能的说明
- 非动态调用：技能是参考资料，不是单独调用

### 技能（直接调用）

```yaml
# 在技能定义中（.claude/skills/weather-svg-creator/SKILL.md）
---
name: weather-svg-creator
description: Creates an SVG weather card...
---
```

- 通过 Skill 工具调用：命令调用 Skill(skill: "weather-svg-creator")
- 独立执行：在命令的上下文中运行，不在 agent 内部
- 从上下文接收数据：使用对话中已可用的温度数据