# 技能最佳实践

![Last Updated](https://img.shields.io/badge/Last_Updated-Apr%2013%2C%202026%208%3A02%20PM%20PKT-white?style=flat&labelColor=555) ![Version](https://img.shields.io/badge/Claude_Code-v2.1.101-blue?style=flat&labelColor=555)<br>
[![Implemented](https://img.shields.io/badge/Implemented-2ea44f?style=flat)](../implementation/claude-skills-implementation.md)

Claude Code 技能 — frontmatter 字段和官方捆绑技能。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Frontmatter 字段 (13)

| 字段 | 类型 | 必填 | 描述 |
|-------|------|----------|-------------|
| `name` | string | 否 | 显示名称和 `/斜杠命令` 标识符。如果省略，默认为目录名称 |
| `description` | string | 推荐 | 技能的用途。显示在自动补全中，并用于 Claude 的自动发现 |
| `argument-hint` | string | 否 | 自动补全时显示的提示（例如 `[issue-number]`、`[filename]`） |
| `disable-model-invocation` | boolean | 否 | 设为 `true` 防止 Claude 自动调用此技能 |
| `user-invocable` | boolean | 否 | 设为 `false` 从 `/` 菜单中隐藏 — 技能变为后台知识，仅用于代理预加载 |
| `allowed-tools` | string | 否 | 此技能激活时允许无需权限提示的工具 |
| `model` | string | 否 | 运行此技能时使用的模型（例如 `haiku`、`sonnet`、`opus`） |
| `effort` | string | 否 | 调用时覆盖模型的努力级别（`low`、`medium`、`high`、`max`） |
| `context` | string | 否 | 设为 `fork` 在隔离的子代理上下文中运行技能 |
| `agent` | string | 否 | 当设置 `context: fork` 时的子代理类型（默认：`general-purpose`） |
| `hooks` | object | 否 | 作用于此技能的生命周期钩子 |
| `paths` | string/list | 否 | 限制技能自动激活的 Glob 模式。接受逗号分隔的字符串或 YAML 列表 — Claude 仅在处理匹配的文件时加载该技能 |
| `shell` | string | 否 | `` !`command` `` 块的 Shell — `bash`（默认）或 `powershell`。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1` |

---

## ![Official](../!/tags/official.svg) **(5)**

| # | 技能 | 描述 |
|---|-------|-------------|
| 1 | `simplify` | 审查更改的代码以进行重用、质量和效率 — 重构以消除重复 |
| 2 | `batch` | 批量跨多个文件运行命令 |
| 3 | `debug` | 调试失败的命令或代码问题 |
| 4 | `loop` | 按 recurring interval 运行提示或斜杠命令（最长 3 天） |
| 5 | `claude-api` | 使用 Claude API 或 Anthropic SDK 构建应用 — 在检测到 `anthropic` / `@anthropic-ai/sdk` 导入时触发 |

另请参阅：[官方技能仓库](https://github.com/anthropics/skills/tree/main/skills)，其中包含社区维护的可安装技能。

---

## 来源

- [Claude Code 技能 — 文档](https://code.claude.com/docs/en/skills)
- [Monorepo 中的技能发现](../reports/claude-skills-for-larger-mono-repos.md)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)