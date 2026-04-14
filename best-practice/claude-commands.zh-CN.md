# 命令最佳实践

![Last Updated](https://img.shields.io/badge/Last_Updated-Apr%2013%2C%202026%208%3A00%20PM%20PKT-white?style=flat&labelColor=555) ![Version](https://img.shields.io/badge/Claude_Code-v2.1.101-blue?style=flat&labelColor=555)<br>
[![Implemented](https://img.shields.io/badge/Implemented-2ea44f?style=flat)](../implementation/claude-commands-implementation.md)

Claude Code 命令 — frontmatter 字段和官方内置斜杠命令。

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
| `description` | string | 推荐 | 命令的功能。显示在自动补全中，并用于 Claude 的自动发现 |
| `argument-hint` | string | 否 | 自动补全时显示的提示（例如 `[issue-number]`、`[filename]`） |
| `disable-model-invocation` | boolean | 否 | 设为 `true` 防止 Claude 自动调用此命令 |
| `user-invocable` | boolean | 否 | 设为 `false` 从 `/` 菜单中隐藏 — 命令变为后台知识 |
| `paths` | string/list | 否 | 限制此技能何时激活的 Glob 模式。接受逗号分隔的字符串或 YAML 列表。设置后，Claude 仅在处理与模式匹配的文件时自动加载该技能 |
| `allowed-tools` | string | 否 | 此命令激活时允许无需权限提示的工具 |
| `model` | string | 否 | 运行此命令时使用的模型（例如 `haiku`、`sonnet`、`opus`） |
| `effort` | string | 否 | 调用时覆盖模型的努力级别（`low`、`medium`、`high`、`max`） |
| `context` | string | 否 | 设为 `fork` 在隔离的子代理上下文中运行命令 |
| `agent` | string | 否 | 当设置 `context: fork` 时的子代理类型（默认：`general-purpose`） |
| `shell` | string | 否 | `` !`command` `` 块的 Shell — 接受 `bash`（默认）或 `powershell`。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1` |
| `hooks` | object | No | 作用于此命令的生命周期钩子 |

---

## ![Official](../!/tags/official.svg) **(69)**

| # | 命令 | 标签 | 描述 |
|---|---------|-----|-------------|
| 1 | `/login` | ![Auth](https://img.shields.io/badge/Auth-2980B9?style=flat) | 登录到您的 Anthropic 账户 |
| 2 | `/logout` | ![Auth](https://img.shields.io/badge/Auth-2980B9?style=flat) | 从 Anthropic 账户登出 |
| 3 | `/setup-bedrock` | ![Auth](https://img.shields.io/badge/Auth-2980B9?style=flat) | 通过交互式向导配置 Amazon Bedrock 认证、区域和模型锁定。仅在设置 `CLAUDE_CODE_USE_BEDROCK=1` 时可见。首次使用 Bedrock 的用户也可以从登录屏幕访问此向导 |
| 4 | `/setup-vertex` | ![Auth](https://img.shields.io/badge/Auth-2980B9?style=flat) | 通过交互式向导配置 Google Vertex AI 认证、项目、区域和模型锁定。仅在设置 `CLAUDE_CODE_USE_VERTEX=1` 时可见。首次使用 Vertex AI 的用户也可以从登录屏幕访问此向导 |
| 5 | `/upgrade` | ![Auth](https://img.shields.io/badge/Auth-2980B9?style=flat) | 打开升级页面以切换到更高级别的订阅计划 |
| 6 | `/color [color\|default]` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 为当前会话设置提示栏颜色。可用颜色：`red`、`blue`、`green`、`yellow`、`purple`、`orange`、`pink`、`cyan`。使用 `default` 重置 |
| 7 | `/config` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 打开设置界面以调整主题、模型、输出样式和其他偏好。别名：`/settings` |
| 8 | `/keybindings` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 打开或创建您的键盘绑定配置文件 |
| 9 | `/permissions` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 管理工具权限的 allow、ask 和 deny 规则。打开一个交互式对话框，您可以在其中按范围查看规则、添加或删除规则、管理工作目录以及查看最近的自动模式拒绝。别名：`/allowed-tools` |
| 10 | `/privacy-settings` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 查看和更新您的隐私设置。仅适用于 Pro 和 Max 计划订阅者 |
| 11 | `/sandbox` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 切换沙盒模式。仅在支持的平台上可用 |
| 12 | `/statusline` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 配置 Claude Code 的状态行。描述您想要的内容，或不带参数运行以从您的 shell 提示符自动配置 |
| 13 | `/stickers` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 订购 Claude Code 贴纸 |
| 14 | `/terminal-setup` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 配置终端的键盘绑定，包括 Shift+Enter 和其他快捷键。仅在需要它的终端中可见，如 VS Code、Alacritty 或 Warp |
| 15 | `/theme` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 更改颜色主题。包括浅色和深色变体、无障碍（色盲友好）主题以及使用终端调色板的 ANSI 主题 |
| 16 | `/voice` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 切换按键说话语音输入。需要 Claude.ai 账户 |
| 17 | `/context` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 可视化当前上下文使用情况为彩色网格。显示针对上下文密集型工具的优化建议、内存膨胀和容量警告 |
| 18 | `/cost` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 显示令牌使用统计。请参阅订阅特定详情的使用成本跟踪指南 |
| 19 | `/extra-usage` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 配置额外使用量，以便在遇到速率限制时继续工作 |
| 20 | `/insights` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 生成分析您 Claude Code 会话的报告，包括项目领域、交互模式和摩擦点 |
| 21 | `/stats` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 可视化每日使用情况、会话历史、连续天数和模型偏好 |
| 22 | `/status` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 打开设置界面（状态标签）显示版本、模型、账户和连接状态。在 Claude 响应时也能工作，无需等待当前响应完成 |
| 23 | `/usage` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 显示计划使用限制和速率限制状态 |
| 24 | `/doctor` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 诊断和验证您的 Claude Code 安装和设置 |
| 25 | `/feedback [report]` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 提交关于 Claude Code 的反馈。别名：`/bug` |
| 26 | `/help` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 显示帮助和可用命令 |
| 27 | `/powerup` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 通过带有动画演示的快速交互式课程发现 Claude Code 功能 |
| 28 | `/release-notes` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 在交互式版本选择器中查看更新日志。选择特定版本以查看其发布说明，或选择显示所有版本 |
| 29 | `/tasks` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 列出和管理后台任务。别名：`/bashes` |
| 30 | `/copy [N]` | ![Export](https://img.shields.io/badge/Export-7F8C8D?style=flat) | 将最后一条助手响应复制到剪贴板。传入数字 `N` 复制第 N 条最新响应：`/copy 2` 复制倒数第二条。当存在代码块时，显示交互式选择器以选择单个块或完整响应。在选择器中按 `w` 将选择内容写入文件而不是剪贴板，这在 SSH 环境下很有用 |
| 31 | `/export [filename]` | ![Export](https://img.shields.io/badge/Export-7F8C8D?style=flat) | 将当前对话导出为纯文本。带文件名时直接写入该文件。不带则打开对话框以复制到剪贴板或保存到文件 |
| 32 | `/agents` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 管理代理配置 |
| 33 | `/chrome` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 在 Chrome 设置中配置 Claude |
| 34 | `/hooks` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 查看工具事件的钩子配置 |
| 35 | `/ide` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 管理 IDE 集成并显示状态 |
| 36 | `/mcp` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 管理 MCP 服务器连接和 OAuth 认证 |
| 37 | `/plugin` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 管理 Claude Code 插件 |
| 38 | `/reload-plugins` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 重新加载所有活动插件以应用待处理的更改，无需重启。报告每个重新加载的组件计数，并标记任何加载错误 |
| 39 | `/skills` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 列出可用技能 |
| 40 | `/memory` | ![Memory](https://img.shields.io/badge/Memory-3498DB?style=flat) | 编辑 `CLAUDE.md` 内存文件，启用或禁用自动记忆，并查看自动记忆条目 |
| 41 | `/effort [low\|medium\|high\|max\|auto]` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 设置模型的努力级别。`low`、`medium` 和 `high` 跨会话保持。`max` 仅适用于当前会话，需要 Opus 4.6。`auto` 重置为模型默认。不带参数时显示当前级别。立即生效，无需等待当前响应完成 |
| 42 | `/fast [on\|off]` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 切换快速模式开启或关闭 |
| 43 | `/model [model]` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 选择或更改 AI 模型。对于支持的模型，使用左/右箭头调整努力级别。更改立即生效，无需等待当前响应完成 |
| 44 | `/passes` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 与朋友分享一周的免费 Claude Code。仅在您的账户符合条件时可见 |
| 45 | `/plan [description]` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 直接从提示符进入计划模式。传递可选描述以进入计划模式并立即开始该任务，例如 `/plan fix the auth bug` |
| 46 | `/ultraplan <prompt>` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 在 ultraplan 会话中起草计划，在浏览器中审查，然后远程执行或发送回您的终端 |
| 47 | `/add-dir <path>` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 添加当前会话期间可访问文件的工作目录。大多数 `.claude/` 配置不会从添加的目录中发现 |
| 48 | `/diff` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 打开交互式差异查看器，显示未提交的更改和每轮差异。使用左/箭头在当前 git 差异和各个 Claude 轮次之间切换，使用上/下浏览文件 |
| 49 | `/init` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 使用 `CLAUDE.md` 指南初始化项目。设置 `CLAUDE_CODE_NEW_INIT=1` 以获得交互式流程，还会引导您了解技能、钩子和个人内存文件 |
| 50 | `/review` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 已弃用。请改为安装 `code-review` 插件：`claude plugin install code-review@claude-plugins-official` |
| 51 | `/security-review` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 分析当前分支上的待处理更改是否存在安全漏洞。检查 git 差异并识别注入、认证问题和数据暴露等风险 |
| 52 | `/autofix-pr [prompt]` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 生成一个在 Web 会话上监视当前分支 PR 的 Claude Code，当 CI 失败或审阅者留下评论时推送修复。通过 `gh pr view` 从您检出的分支检测打开的 PR；要监视不同的 PR，请先检出其分支。需要 `gh` CLI 和 Web 上的 Claude Code 访问权限 |
| 53 | `/desktop` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 在 Claude Code Desktop 应用中继续当前会话。仅限 macOS 和 Windows。别名：`/app` |
| 54 | `/install-github-app` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 为仓库设置 Claude GitHub Actions 应用。引导您选择仓库并配置集成 |
| 55 | `/install-slack-app` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 安装 Claude Slack 应用。打开浏览器完成 OAuth 流程 |
| 56 | `/mobile` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 显示二维码以下载 Claude 移动应用。别名：`/ios`、`/android` |
| 57 | `/remote-control` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 使此会话可从 claude.ai 进行远程控制。别名：`/rc` |
| 58 | `/remote-env` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 配置使用 `--remote` 启动的 Web 会话的默认远程环境 |
| 59 | `/schedule [description]` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 创建、更新、列出或运行 Cloud 计划任务。Claude 会话式地引导您完成设置 |
| 60 | `/teleport` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 将 Web 上的 Claude Code 会话拉入此终端：打开选择器，然后获取分支和对话。也可用作 `/tp`。需要 claude.ai 订阅 |
| 61 | `/web-setup` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 使用本地 `gh` CLI 凭证将您的 GitHub 账户连接到 Web 上的 Claude Code。如果 GitHub 未连接，`/schedule` 会自动提示 |
| 62 | `/branch [name]` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 在此时刻创建当前对话的分支。别名：`/fork` |
| 63 | `/btw <question>` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 提出一个快速的问题而不添加到对话中 |
| 64 | `/clear` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 清除对话历史并释放上下文。别名：`/reset`、`/new` |
| 65 | `/compact [instructions]` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 使用可选的焦点指令压缩对话 |
| 66 | `/exit` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 退出 CLI。别名：`/quit` |
| 67 | `/rename [name]` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 重命名当前会话并在提示栏显示名称。不带名称则从对话历史自动生成 |
| 68 | `/resume [session]` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 按 ID 或名称恢复对话，或打开会话选择器。别名：`/continue` |
| 69 | `/rewind` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 将对话和/或代码倒回先前某一点，或从选定的消息开始总结。请参阅检查点。别名：`/checkpoint` |

捆绑的技能如 `/debug` 也可以出现在斜杠命令菜单中，但它们不是内置命令。

---

## 来源

- [Claude Code 斜杠命令](https://code.claude.com/docs/en/slash-commands)
- [Claude Code 交互模式](https://code.claude.com/docs/en/interactive-mode)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)