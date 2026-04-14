# 子代理报告 — 变更日志历史

## 状态说明

| 状态 | 含义 |
|--------|---------|
| ✅ `COMPLETE (原因)` | 操作已执行并成功解决 |
| ❌ `INVALID (原因)` | 发现有误、不适用或为故意为之 |
| ✋ `ON HOLD (原因)` | 操作已推迟 — 等待外部依赖或用户决定 |

---

## [2026-02-28 03:22 下午 PKT] Claude Code v2.1.63

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 代理表 | 添加工作流变更日志子代理到本仓库代理表 | ✅ COMPLETE |
| 2 | HIGH | 代理表 | 修复 presentation-curator 技能列 | ✅ COMPLETE |
| 3 | MED | 字段文档 | 添加 color 字段说明 | ✅ COMPLETE |
| 4 | MED | 调用部分 | 扩展调用部分 | ✅ COMPLETE |

---

## [2026-03-07 08:35 上午 PKT] Claude Code v2.1.71

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 失效链接 | 修复代理内存链接 | ✅ COMPLETE |
| 2 | HIGH | 行为变更 | 更新 tools 字段描述：Task → Agent | ✅ COMPLETE |
| 3 | HIGH | 行为变更 | 更新调用部分：Task → Agent | ✅ COMPLETE |
| 4 | HIGH | 示例更新 | 更新完整示例 | ✅ COMPLETE |
| 5 | HIGH | 内置代理 | 添加 Bash 代理 | ✅ COMPLETE |
| 6 | HIGH | 代理表 | 添加概念代理 | ✅ COMPLETE |
| 7 | HIGH | 代理表 | 添加 Claude 设置代理 | ✅ COMPLETE |
| 8 | MED | 内置代理 | 修复 statusline-setup 模型 | ✅ COMPLETE |
| 9 | MED | 内置代理 | 修复 claude-code-guide 模型 | ❌ 不适用（已移除）|
| 10 | MED | 代理表 | 修复 weather-agent 颜色 | ✅ COMPLETE |
| 11 | MED | 调用 | 添加 --agent CLI 标志 | ✅ COMPLETE |
| 12 | MED | 行为变更 | 更新表标题 | ✅ COMPLETE |
| 13 | MED | 跨文件 | 更新 CLAUDE.md | ✅ COMPLETE |

---

## [2026-03-12 12:17 下午 PKT] Claude Code v2.1.74

未检测到漂移 — 报告与官方文档完全同步。所有 13 个前matter 字段和 6 个内置代理匹配。

---

## [2026-03-13 04:21 下午 PKT] Claude Code v2.1.74

未检测到漂移。

---

## [2026-03-15 12:50 下午 PKT] Claude Code v2.1.76

未检测到漂移。

---

## [2026-03-17 12:42 下午 PKT] Claude Code v2.1.77

未检测到漂移。

---

## [2026-03-18 11:41 下午 PKT] Claude Code v2.1.78

未检测到漂移。

---

## [2026-03-19 11:56 上午 PKT] Claude Code v2.1.79

未检测到漂移。

---

## [2026-03-20 08:35 上午 PKT] Claude Code v2.1.80

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新字段 | 添加 effort 字段 | ✅ COMPLETE（已添加，计数更新 14→15）|

---

## [2026-03-21 09:07 下午 PKT] Claude Code v2.1.81

未检测到漂移。

---

## [2026-03-23 09:49 下午 PKT] Claude Code v2.1.81

未检测到漂移 — 所有 15 个前matter 字段（14 个官方 + 1 个非官方 color）和 6 个内置代理匹配。

---

## [2026-03-25 08:07 下午 PKT] Claude Code v2.1.83

**观察项：** initialPrompt 在 v2.1.83 变更日志中添加但尚未出现在官方文档支持的前matter 字段表中。出现时报告需要更新。

---

## [2026-03-26 01:01 下午 PKT] Claude Code v2.1.84

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新字段 | 添加 initialPrompt | ✅ COMPLETE（已添加，计数更新 15→16）|

---

## [2026-03-27 06:28 下午 PKT] Claude Code v2.1.85

未检测到漂移。

---

## [2026-03-28 06:00 下午 PKT] Claude Code v2.1.86

未检测到漂移。

---

## [2026-04-01 12:26 下午 PKT] Claude Code v2.1.89

未检测到漂移。

---

## [2026-04-02 09:11 下午 PKT] Claude Code v2.1.90

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 移除代理 | 从内置代理表移除 Bash | ✅ COMPLETE |
| 2 | LOW | 字段文档 | 更新 color 字段描述 | ✅ COMPLETE |

---

## [2026-04-03 08:30 下午 PKT] Claude Code v2.1.91

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | LOW | 字段文档 | 添加 auto 到 permissionMode | ✅ COMPLETE |

---

## [2026-04-04 10:43 下午 PKT] Claude Code v2.1.92

未检测到漂移。

---

## [2026-04-08 09:34 下午 PKT] Claude Code v2.1.96

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | LOW | 字段文档 | 更新 model 字段 | ✅ COMPLETE |
| 2 | LOW | 字段文档 | 更新 effort 字段 | ✅ COMPLETE |
| 3 | LOW | 字段文档 | 更新 color 字段 | ✅ COMPLETE |

---

## [2026-04-09 11:34 下午 PKT] Claude Code v2.1.97

未检测到漂移。

---

## [2026-04-11 06:10 下午 PKT] Claude Code v2.1.101

未检测到漂移。

---

## [2026-04-13 08:02 下午 PKT] Claude Code v2.1.101

未检测到漂移 — 所有 16 个前matter 字段和 5 个内置代理与官方文档完全同步。
