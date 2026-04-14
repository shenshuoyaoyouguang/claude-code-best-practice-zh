# 技能实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_02%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#weather-svg-creator"><img src="../!/tags/implemented-hd.svg" alt="已实现"></a>

本仓库中实现了两个技能作为命令 → Agent → 技能架构模式的一部分，演示了两种不同的技能调用模式：agent 技能（预加载）和技能（直接调用）。

---

## 天气 SVG 创建器（技能）

文件：.claude/skills/weather-svg-creator/SKILL.md

```yaml
---
name: weather-svg-creator
description: Creates an SVG weather card showing the current temperature for
  Dubai. Writes the SVG to orchestration-workflow/weather.svg and updates
  orchestration-workflow/output.md.
---

# Weather SVG Creator Skill

This skill creates a visual SVG weather card and writes the output files.

## Task
Create an SVG weather card displaying the temperature for Dubai, UAE,
and write it along with a summary to output files.

## Instructions
You will receive the temperature value and unit (Celsius or Fahrenheit)
from the calling context.

### 1. Create SVG Weather Card
Generate a clean SVG weather card...

### 2. Write SVG File
Write the SVG content to orchestration-workflow/weather.svg.

### 3. Write Output Summary
Write to orchestration-workflow/output.md...

...
```

这是一个技能 — 通过 Skill 工具由命令直接调用。它从对话上下文接收温度数据，并创建 SVG 天气卡和输出摘要。

---

## 天气获取器（Agent 技能）

文件：.claude/skills/weather-fetcher/SKILL.md

```yaml
---
name: weather-fetcher
description: Instructions for fetching current weather temperature data
  for Dubai, UAE from Open-Meteo API
user-invocable: false
---

# Weather Fetcher Skill

This skill provides instructions for fetching current weather data.

## Task
Fetch the current temperature for Dubai, UAE in the requested unit
(Celsius or Fahrenheit).

## Instructions
1. Fetch Weather Data: Use the WebFetch tool to get current weather data
   - Celsius URL: https://api.open-meteo.com/v1/forecast?latitude=25.2048&longitude=55.2708&current=temperature_2m&temperature_unit=celsius
   - Fahrenheit URL: https://api.open-meteo.com/v1/forecast?latitude=25.2048&longitude=55.2708&current=temperature_2m&temperature_unit=fahrenheit
2. Extract Temperature: From the JSON response, extract current.temperature_2m
3. Return Result: Return the temperature value and unit clearly.

...
```

这是一个 agent 技能 — 在启动时通过 skills 前缀字段预加载到 weather-agent 中。它不是直接调用的，而是作为领域知识注入到 agent 的上下文中。注意 user-invocable: false，这使其从 / 命令菜单中隐藏。

---

## 两种技能模式

| 模式 | 调用方式 | 示例 | 关键区别 |
|---------|-----------|---------|----------------|
| 技能 | 通过 Skill(skill: "name") 调用 | weather-svg-creator | 通过 Skill 工具直接调用 |
| Agent 技能 | 通过 skills 字段预加载 | weather-fetcher | 在启动时注入到 agent 上下文 |

---

## 使用方法

技能 — 通过斜杠命令直接调用：
```bash
$ claude
> /weather-svg-creator
```

---

## 实现方法

让 Claude 为您创建一个 — 它将在 .claude/skills/my-skill/SKILL.md 中生成带有 YAML 头和正文的 markdown 文件

# My Skill

Instructions for what the skill does.