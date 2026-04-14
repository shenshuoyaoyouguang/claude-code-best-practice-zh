# 设置报告 — 变更日志历史

## 状态说明

| 状态 | 含义 |
|--------|---------|
| ✅ `COMPLETE (原因)` | 操作已执行并成功解决 |
| ❌ `INVALID (原因)` | 发现有误、不适用或为故意为之 |
| ✋ `ON HOLD (原因)` | 操作已推迟 — 等待外部依赖或用户决定 |

---

## [2026-03-05 06:18 上午 PKT] Claude Code v2.1.69

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失设置项 | 添加 13 个非钩子缺失设置项（`$schema`、`availableModels`、`fastModePerSessionOptIn`、`teammateMode`、`prefersReducedMotion`、`sandbox.filesystem.*`、`sandbox.network.allowManagedDomainsOnly`、`sandbox.enableWeakerNetworkIsolation`、`allowManagedMcpServersOnly`、`blockedMarketplaces`、`includeGitInstructions`、`pluginTrustMessage`、`fileSuggestion`）| ✅ COMPLETE（已添加到报告） |
| 2 | HIGH | 缺失环境变量 | 添加缺失的环境变量，包括 `CLAUDE_CODE_DISABLE_ADAPTIVE_THINKING`、`CLAUDE_CODE_DISABLE_1M_CONTEXT`、`CLAUDE_CODE_ACCOUNT_UUID`、`CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS`、`ENABLE_CLAUDEAI_MCP_SERVERS` 等 | ✅ COMPLETE（已添加 13 个缺失环境变量到报告） |
| 3 | HIGH | 努力程度默认值 | 将努力程度默认值从"High"更新为"Medium"（Max/Team 订阅者）；添加 Sonnet 4.6 支持 | ✅ COMPLETE |
| 4 | MED | 设置层级 | 通过 macOS plist/Windows 注册表添加托管设置 | ✅ COMPLETE |
| 5 | MED | 沙箱文件系统 | 添加 `sandbox.filesystem.allowWrite`、`denyWrite`、`denyRead` | ✅ COMPLETE |
| 6 | MED | 权限语法 | 添加 `Agent(name)` 权限模式 | ✅ COMPLETE |
| 7 | MED | 插件缺口 | 添加 `blockedMarketplaces`、`pluginTrustMessage` | ✅ COMPLETE |
| 8 | MED | 模型配置 | 添加 `availableModels` 设置项 | ✅ COMPLETE |
| 9 | MED | 可疑键 | 验证可疑键 | ✋ ON HOLD |
| 10 | LOW | 标题计数 | 更新标题计数 | ✅ COMPLETE |
| 11 | LOW | CLAUDE.md 同步 | 更新 CLAUDE.md 配置层级 | ✋ ON HOLD |
| 12 | LOW | 示例更新 | 更新快速参考示例 | ✅ COMPLETE |
| 13 | MED | 钩子重定向 | 将钩子部分重定向到专用仓库 | ✅ COMPLETE |

---

## [2026-03-07 02:17 下午 PKT] Claude Code v2.1.71

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 行为变更 | 修复 `teammateMode`：类型和默认值 | ✅ COMPLETE |
| 2 | HIGH | 新设置项 | 添加 `allowManagedPermissionRulesOnly` | ✅ COMPLETE |
| 3 | HIGH | 缺失环境变量 | 添加约 31 个缺失环境变量 | ✅ COMPLETE |
| 4 | MED | 默认值变更 | 修复 `plansDirectory` 默认值 | ✅ COMPLETE |
| 5 | MED | 描述变更 | 修复 `sandbox.enableWeakerNetworkIsolation` | ✅ COMPLETE |
| 6 | MED | 作用域修复 | 修复 `extraKnownMarketplaces` 作用域 | ✅ COMPLETE |
| 7 | MED | 越界违规 | 替换为交叉引用 | ✅ COMPLETE |
| 8 | MED | 版本徽章 | 更新到 v2.1.71 | ✅ COMPLETE |
| 9 | LOW | 可疑键 | 验证可疑键 | ✋ ON HOLD |
| 10 | LOW | CLAUDE.md 同步 | 更新到 5 级层级 | ✅ COMPLETE |

---

## [2026-03-12 12:23 下午 PKT] Claude Code v2.1.74

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 行为变更 | 修复 `dontAsk` 权限模式描述 | ✅ COMPLETE |
| 2 | HIGH | 新设置项 | 添加 `modelOverrides` | ✅ COMPLETE |
| 3 | HIGH | 新设置项 | 添加 `allow_remote_sessions` | ✅ COMPLETE |
| 4 | HIGH | 默认值变更 | 修复 `$schema` URL | ✅ COMPLETE |
| 5 | MED | 描述变更 | 修复 `ANTHROPIC_CUSTOM_HEADERS` 格式 | ✅ COMPLETE |
| 6 | MED | 未验证模式 | 标记未验证模式 | ✅ COMPLETE |
| 7 | MED | 缺失环境变量 | 添加 5 个环境变量 | ✅ COMPLETE |
| 8 | MED | 新设置项 | 添加 `autoMemoryDirectory` | ✅ COMPLETE |
| 9 | LOW | 可疑键 | 验证可疑键 | ✋ ON HOLD |
| 10 | LOW | 缺失环境变量 | 添加 SUBAGENT_MODEL | ✅ COMPLETE |
| 11 | LOW | 示例更新 | 更新示例 | ✅ COMPLETE |

---

## [2026-03-14 01:35 上午 PKT] Claude Code v2.1.75

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 设置层级 | 重构为 5 级层级 | ✅ COMPLETE |
| 2 | HIGH | 行为变更 | 修复 `availableModels` 描述 | ✅ COMPLETE |
| 3 | HIGH | 行为变更 | 添加 cleanupPeriodDays 0 值行为 | ✅ COMPLETE |
| 4 | HIGH | 权限语法 | 添加评估顺序说明 | ✅ COMPLETE |
| 5 | MED | 描述变更 | 添加作用域限制 | ✅ COMPLETE |
| 6 | MED | 描述变更 | 添加远程环境说明 | ✅ COMPLETE |
| 7 | MED | 模型配置 | 添加 Opus 4.6 默认说明 | ✅ COMPLETE |
| 8 | MED | 设置层级 | 添加 Windows 路径说明 | ✅ COMPLETE |
| 9 | MED | 显示与用户体验 | 添加 stdin 格式 | ✅ COMPLETE |
| 10 | MED | 设置层级 | 更新合并说明 | ✅ COMPLETE |
| 11 | LOW | 可疑键 | 可疑键待验证 | ✋ ON HOLD |
| 12 | LOW | 可疑键 | Schema 确认键 | ✋ ON HOLD |

---

## [2026-03-15 12:52 下午 PKT] Claude Code v2.1.76

所有条目保持 ON HOLD，等待用户批准

---

## [2026-03-15 01:10 下午 PKT] Claude Code v2.1.76

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1-13 | 多项 | 用户批准后执行 | ✅ COMPLETE |

---

## [2026-03-17 12:54 下午 PKT] Claude Code v2.1.77

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 添加 allowRead | ✅ COMPLETE |
| 2 | HIGH | 描述变更 | 更新 MAX_OUTPUT_TOKENS | ✅ COMPLETE |
| 3 | HIGH | 缺失环境变量 | 添加 CLAUDECODE | ✅ COMPLETE |
| 4 | HIGH | 缺失环境变量 | 添加 SKIP_FAST_MODE | ✅ COMPLETE |
| 5 | MED | 环境变量表 | 移动 ANTHROPIC 变量 | ✅ COMPLETE |
| 6-12 | 多项 | 更新完成 | ✅ COMPLETE |

---

## [2026-03-18 11:53 下午 PKT] Claude Code v2.1.78

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失设置项 | 添加 voiceEnabled | ✅ COMPLETE |
| 2 | HIGH | 缺失设置项 | 添加 allowManagedReadPathsOnly | ✅ COMPLETE |
| 3 | HIGH | 显示位置 | 移动到全局配置 | ✅ COMPLETE |
| 4 | HIGH | 默认值变更 | 修复 MAX_MCP_OUTPUT | ✅ COMPLETE |
| 5-12 | 多项 | 各项更新 | ✅ COMPLETE |

---

## [2026-03-19 12:38 下午 PKT] Claude Code v2.1.79

所有条目 COMPLETE

---

## [2026-03-20 08:41 上午 PKT] Claude Code v2.1.80

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 添加 channelsEnabled | ✅ COMPLETE |
| 2 | MED | 版本徽章 | 更新到 v2.1.80 | ✅ COMPLETE |

---

## [2026-03-21 09:17 下午 PKT] Claude Code v2.1.81

所有条目 COMPLETE

---

## [2026-03-23 10:02 下午 PKT] Claude Code v2.1.81

所有条目 COMPLETE

---

## [2026-03-25 08:16 下午 PKT] Claude Code v2.1.83

所有条目 COMPLETE

---

## [2026-03-26 01:04 下午 PKT] Claude Code v2.1.84

所有条目 COMPLETE

---

## [2026-03-27 06:32 下午 PKT] Claude Code v2.1.85

所有条目 COMPLETE

---

## [2026-03-28 06:10 下午 PKT] Claude Code v2.1.86

所有条目 COMPLETE

---

## [2026-03-31 07:02 下午 PKT] Claude Code v2.1.88

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失环境变量 | 添加 NO_FLICKER | ✅ COMPLETE |
| 2 | HIGH | 缺失环���变�� | 添加滚动/鼠标 | ✅ COMPLETE |
| 3-6 | 多项 | 各项更新 | ✅ COMPLETE |
| 7 | LOW | 标题计数 | 计数准确 | ❌ INVALID |

---

## [2026-04-01 12:32 下午 PKT] Claude Code v2.1.89

所有条目 COMPLETE

---

## [2026-04-02 09:24 下午 PKT] Claude Code v2.1.90

所有条目 COMPLETE

---

## [2026-04-03 08:44 下午 PKT] Claude Code v2.1.91

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 添加 disableSkillShellExecution | ✅ COMPLETE |
| 2 | HIGH | 版本徽章 | 更新到 v2.1.91 | ✅ COMPLETE |

---

## [2026-04-04 10:48 下午 PKT] Claude Code v2.1.92

所有条目 COMPLETE

---

## [2026-04-08 09:51 下午 PKT] Claude Code v2.1.96

所有条目 COMPLETE

---

## [2026-04-09 11:39 下午 PKT] Claude Code v2.1.97

所有条目 COMPLETE

---

## [2026-04-13 08:10 下午 PKT] Claude Code v2.1.101

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失环境变量 | 添加 CERT_STORE | ✅ COMPLETE |
| 2 | HIGH | 缺失环境变量 | 添加 PERFORCE_MODE | ✅ COMPLETE |
| 3 | HIGH | 缺失环境变量 | 添加 SCRIPT_CAPS | ✅ COMPLETE |
| 4 | HIGH | 版本徽章 | 更新到 v2.1.101 | ✅ COMPLETE |
| 5-7 | MED | 各项更新 | ✅ COMPLETE |
