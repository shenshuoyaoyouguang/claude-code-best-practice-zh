---
model: haiku
---

# 时间编排器命令

获取迪拜的当前时间（Asia/Dubai，UTC+4）并创建可视化 SVG 时间卡。

## 工作流

### 步骤 1：获取当前迪拜时间

使用 Agent 工具调用时间 agent：
- subagent_type: time-agent
- description: 获取当前迪拜时间
- prompt: 获取迪拜的当前时间（Asia/Dubai，UTC+4）。精确返回三个字段：time（时间部分，例如 "14:30:45"）、timezone（"GST (UTC+4)"）和 formatted（完整格式化字符串，例如 "2026-03-12 14:30:45 +04"）。agent 有预加载技能（time-fetcher）提供详细说明。
- model: haiku

等待 agent 完成并捕获返回的时间数据。

### 数据契约

time-agent 必须返回这三个字段：
- **time**：时间部分（例如，"14:30:45"）
- **timezone**："GST (UTC+4)"
- **formatted**：完整格式化字符串（例如，"2026-03-12 14:30:45 +04"）

### 步骤 2：创建 SVG 时间卡

使用 Skill 工具调用 time-svg-creator 技能：
- skill: time-svg-creator
- args: 从步骤 1 传递时间数据 — 包含 time、timezone 和 formatted 值

技能将使用步骤 1 的时间数据（当前上下文中可用）创建 SVG 卡并写入输出文件。

## 关键要求

1. **对 time-agent 使用 Agent 工具**：不要使用 bash 命令调用 agent。您必须使用 Agent 工具，subagent_type: "time-agent"。
2. **对 SVG 创建器使用 Skill 工具**：通过 Skill 工具调用 SVG 创建器，skill: "time-svg-creator"，而不是 Agent 工具。
3. **顺序流**：agent 必须完成并返回时间数据，然后才能调用技能。不要并行运行它们。
4. **数据传递**：确保从 agent 响应获取的所有三个字段（time、timezone、formatted）在调用技能时在上下文中可用。

## 输出摘要

两个步骤完成后，向用户提供清晰摘要，显示：
- 获取的当前迪拜时间
- 时区：GST (UTC+4)
- 完整格式化时间戳
- SVG 卡创建于 agent-teams/output/dubai-time.svg
- 摘要写入 agent-teams/output/output.md