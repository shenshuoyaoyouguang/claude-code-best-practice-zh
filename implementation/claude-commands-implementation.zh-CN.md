# 命令实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_02%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#weather-orchestrator"><img src="../!/tags/implemented-hd.svg" alt="已实现"></a>

天气编排命令在本仓库中实现，作为命令 → Agent → 技能架构模式的入口点，演示了命令如何编排多步骤工作流。

---

## 天气编排器

文件：.claude/commands/weather-orchestrator.md

```yaml
---
description: Fetch weather data for Dubai and create an SVG weather card
model: haiku
---

# Weather Orchestrator Command

Fetch the current temperature for Dubai, UAE and create a visual SVG weather card.

## Workflow

### Step 1: Ask User Preference
Use the AskUserQuestion tool to ask the user whether they want the temperature
in Celsius or Fahrenheit.

### Step 2: Fetch Weather Data
Use the Agent tool to invoke the weather agent:
- subagent_type: weather-agent
- prompt: Fetch the current temperature for Dubai, UAE in [unit]...

### Step 3: Create SVG Weather Card
Use the Skill tool to invoke the weather-svg-creator skill:
- skill: weather-svg-creator

...
```

命令编排整个工作流：询问用户温度单位偏好，通过 Agent 工具调用 weather-agent，然后通过 Skill 工具调用 weather-svg-creator 技能。

---

## 使用方法

```bash
$ claude
> /weather-orchestrator
```

---

## 实现方法

让 Claude 为您创建一个 — 它将在 .claude/commands/<name>.md 中生成带有 YAML 头和正文的 markdown 文件。

---

<a href="https://github.com/shanraisshan/claude-code-best-practice#orchestration-workflow"><img src="../!/tags/orchestration-workflow-hd.svg" alt="Orchestration Workflow"></a>

天气编排器是命令 → Agent → 技能编排模式中的命令。它作为入口点 — 处理用户交互（温度单位偏好），将数据获取委托给 weather-agent，并调用 weather-svg-creator 技能进行可视化输出。

<p align="center">
  <img src="../orchestration-workflow/orchestration-workflow.svg" alt="Command Skill Agent Architecture Flow" width="100%">
</p>

| 组件 | 角色 | 本仓库 |
|-----------|------|-----------|
| 命令 | 入口点，用户交互 | /weather-orchestrator |
| Agent | 使用预加载技能获取数据 | weather-agent 配合 weather-fetcher |
| 技能 | 独立创建输出 | weather-svg-creator |