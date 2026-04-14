---
name: time-svg-creator
description: Creates an SVG time card showing the current time for Dubai. Writes the SVG to agent-teams/output/dubai-time.svg and updates agent-teams/output/output.md.
allowed-tools: Write, Read
---

# 时间 SVG 创建器技能

为阿联酋迪拜创建可视化 SVG 时间卡并写入输出文件。

## 任务

您将从调用上下文接收三个字段：time、timezone 和 formatted。创建 SVG 时间卡并写入 SVG 和 markdown 摘要。

## 说明

1. **创建 SVG** — 使用 reference.md 中的 SVG 模板，用实际值替换占位符
2. **写入 SVG 文件** — 写入 agent-teams/output/dubai-time.svg
3. **写入摘要** — 使用 reference.md 中的 markdown 模板写入 agent-teams/output/output.md

## 规则

- 使用提供的确切时间值 — 永不重新获取或重新计算
- SVG 必须是自包含且有效的
- 两个输出文件都放在 agent-teams/output/ 目录中

## 额外资源

- 关于 SVG 模板、输出模板和设计规范，请参见 reference.md
- 关于示例输入/输出对，请参见 examples.md