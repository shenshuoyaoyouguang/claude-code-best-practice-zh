# 子代理实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_02%2C_2026_07%3A59_PM_PKT-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#weather-agent"><img src="../!/tags/implemented-hd.svg" alt="已实现"></a>

天气代理在本仓库中实现，作为命令 → Agent → 技能架构模式的示例，演示了两种不同的技能模式。

---

## 天气代理

文件：.claude/agents/weather-agent.md

```yaml
---
name: weather-agent
description: Use this agent PROACTIVELY when you need to fetch weather data for
  Dubai, UAE. This agent fetches real-time temperature from Open-Meteo
  using its preloaded weather-fetcher skill.
tools: WebFetch, Read, Write, Edit
model: sonnet
color: green
maxTurns: 5
permissionMode: acceptEdits
memory: project
skills:
  - weather-fetcher
---

# Weather Agent

You are a specialized weather agent that fetches weather data for Dubai,
UAE.

## Your Task

Execute the weather workflow by following the instructions from your preloaded
skill:

1. **Fetch**: Follow the weather-fetcher skill instructions to fetch the
   current temperature
2. **Report**: Return the temperature value and unit to the caller
3. **Memory**: Update your agent memory with the reading details for
   historical tracking

...
```

代理有一个预加载技能（weather-fetcher），提供从 Open-Meteo 获取数据的说明。它将温度值和单位返回给调用命令。

---

## 使用方法

```bash
$ claude
> what is the weather in dubai?
```

---

## 实现方法

您可以使用 /agents 命令创建代理，
```bash
$ claude
> /agents
```

或让 Claude 为您创建一个 — 它将在 .claude/agents/<name>.md 中生成带有 YAML 头和正文的 markdown 文件

---

<a href="https://github.com/shanraisshan/claude-code-best-practice#orchestration-workflow"><img src="../!/tags/orchestration-workflow-hd.svg" alt="Orchestration Workflow"></a>

天气代理是命令 → Agent → 技能编排模式中的 Agent。它从 /weather-orchestrator 命令接收工作流，并使用其预加载技能（weather-fetcher）获取温度。然后命令调用独立的 weather-svg-creator 技能来创建可视化输出。

<p align="center">
  <img src="../orchestration-workflow/orchestration-workflow.svg" alt="Command Skill Agent Architecture Flow" width="100%">
</p>

| 组件 | 角色 | 本仓库 |
|-----------|------|-----------|
| 命令 | 入口点，用户交互 | /weather-orchestrator |
| Agent | 使用预加载技能获取数据 | weather-agent 配合 weather-fetcher |
| 技能 | 独立创建输出 | weather-svg-creator |