# 自定义 Claude Code 的 12 种方式 — 来自 Boris Cherny 的技巧

Boris Cherny（[@bcherny](https://x.com/bcherny)），Claude Code 的创造者，于 2026 年 2 月 12 日分享的自定义技巧总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Boris Cherny 强调可定制性是工程师们最喜欢 Claude Code 的地方之一 — hooks、插件、LSPs、MCPs、技能、努力、自定义 agents、状态行、输出样式等等。他分享了开发者和团队自定义其设置的 12 种实用方式。

<a href="https://x.com/bcherny/status/2021699851499798911"><img src="assets/boris-12-feb-26/0.webp" alt="Boris Cherny intro tweet" width="50%" /></a>

---

## 1/ 配置您的终端

为最佳 Claude Code 体验设置您的终端：

- **主题**：运行 `/config` 设置浅色/深色模式
- **通知**：为 iTerm2 启用通知，或使用自定义通知钩子
- **换行符**：如果在 IDE 终端、Apple Terminal、Warp 或 Alacritty 中使用 Claude Code，运行 `/terminal-setup` 启用 shift+enter 换行（这样您不需要输入 `\`）
- **Vim 模式**：运行 `/vim`

<a href="https://x.com/bcherny/status/2021699859359883608"><img src="assets/boris-12-feb-26/1.webp" alt="Configure your terminal" width="50%" /></a>

---

## 2/ 调整努力级别

运行 `/model` 选择您的首选努力级别：

- **Low** — 更少的 tokens，更快的响应
- **Medium** — 平衡的行为
- **High** — 更多的 tokens，更高的智能

Boris 的偏好：一切都是 High。

<a href="https://x.com/bcherny/status/2021699860869902424"><img src="assets/boris-12-feb-26/2.webp" alt="Adjust effort level" width="50%" /></a>

---

## 3/ 安装插件、MCPs 和技能

插件让您安装 LSP（每种主要语言都有）、MCPs、技能、agents 和自定义 hooks。

从官方的 Anthropic 插件市场安装，或为您公司创建自己的市场。将 `settings.json` 检查到您的代码库中以为您的团队自动添加市场。

运行 `/plugin` 开始。

<a href="https://x.com/bcherny/status/2021699862522364149"><img src="assets/boris-12-feb-26/3.webp" alt="Install Plugins, MCPs, and Skills" width="50%" /></a>

---

## 4/ 创建自定义 Agents

在 `.claude/agents` 中放置 `.md` 文件以创建自定义 agents。每个 agent 可以有自定义名称、颜色、工具集、预先允许和预先禁止的工具、权限模式和模型。

您还可以使用 `settings.json` 或 `--agent` 标志为主会话设置默认 agent。

运行 `/agents` 开始。

<a href="https://x.com/bcherny/status/2021700144039903699"><img src="assets/boris-12-feb-26/4.webp" alt="Create custom agents" width="50%" /></a>

---

## 5/ 预先批准常见权限

Claude Code 使用结合了提示注入检测、静态分析、沙箱和人工监督的权限系统。

开箱即用，一小部分安全命令被预先批准。要预先批准更多，运行 `/permissions` 并添加到允许和阻止列表中。将这些检查到您团队的 `settings.json` 中。

支持完整的通配符语法 — 例如 `Bash(bun run *)` 或 `Edit(/docs/**)`。

<a href="https://x.com/bcherny/status/2021700332292911228"><img src="assets/boris-12-feb-26/5.webp" alt="Pre-approve common permissions" width="50%" /></a>

---

## 6/ 启用��箱

选择加入 Claude Code 的开源沙箱运行时，以提高安全性同时减少权限提示。

运行 `/sandbox` 启用它。沙箱在您的机器上运行，并支持文件和网络隔离。

<a href="https://x.com/bcherny/status/2021700506465579443"><img src="assets/boris-12-feb-26/6.webp" alt="Enable sandboxing" width="50%" /></a>

---

## 7/ 添加状态行

自定义状态行显示在作曲器正下方，显示模型、目录、剩余上下文、成本以及您工作中想看到的任何其他内容。

每个团队成员可以有不同的状态行。使用 `/statusline` 让 Claude 根据您的 `.bashrc`/`.zshrc` 生成一个。

<a href="https://x.com/bcherny/status/2021700784019452195"><img src="assets/boris-12-feb-26/7.webp" alt="Add a status line" width="50%" /></a>

---

## 8/ 自定义您的键盘绑定

Claude Code 中的每个键绑定都是可定制的。运行 `/keybindings` 重新映射任何键。设置会实时重新加载，因此您可以立即感受它。

<a href="https://x.com/bcherny/status/2021700883873165435"><img src="assets/boris-12-feb-26/8.webp" alt="Customize your keybindings" width="50%" /></a>

---

## 9/ 设置 Hooks

Hooks 让您确定性地接入 Claude 的生命周期：

- 自动将权限请求路由到 Slack 或 Opus
- 当 Claude 到达一轮的末尾时推动它继续（您甚至可以启动一个 agent 或使用提示来决定 Claude 是否应该继续）
- 预处理或后处理工具调用，例如，添加您自己的日志

让 Claude 添加一个 hook 开始。

<a href="https://x.com/bcherny/status/2021701059253874861"><img src="assets/boris-12-feb-26/9.webp" alt="Set up hooks" width="50%" /></a>

---

## 10/ 自定义您的微调动词

自定义您的微调动词以添加或用您自己的动词替换默认列表。将 `settings.json` 检查到源代码控制中以与您的团队共享动词。

<a href="https://x.com/bcherny/status/2021701145023197516"><img src="assets/boris-12-feb-26/10.webp" alt="Customize your spinner verbs" width="50%" /></a>

---

## 11/ 使用输出样式

运行 `/config` 并设置输出样式以让 Claude 以不同的语气或格式响应。

- **Explanatory** — 推荐在熟悉新代码库时使用，让 Claude 解释框架和代码模式
- **Learning** — 让 Claude 引导您进行代码更改
- **Custom** — 创建自定义输出样式以调整 Claude 的声音

<a href="https://x.com/bcherny/status/2021701379409273093"><img src="assets/boris-12-feb-26/11.webp" alt="Use output styles" width="50%" /></a>

---

## 12/ 自定义所有内容！

Claude Code 开箱即用效果很好，但当您自定义时，将 `settings.json` 检查到 git 中以便您的团队也能受益。支持多级配置：

- 用于您的代码库
- 用于子文件夹
- 仅用于您自己
- 通过企业范围策略

有 37 个设置和 84 个环境变量（在您的 `settings.json` 中使用 `"env"` 字段以避免包装脚本），您想要的大多数行为都是可配置的。

<a href="https://x.com/bcherny/status/2021701636075458648"><img src="assets/boris-12-feb-26/12.webp" alt="Customize all the things" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) on X — 2026 年 2 月 12 日](https://x.com/bcherny)
- [Claude Code 终端设置文档](https://code.claude.com/docs/en/terminal)
- [Claude Code 插件和发现文档](https://code.claude.com/docs/en/discover-plugins)
- [Claude Code 子代理文档](https://code.claude.com/docs/en/sub-agents)
- [Claude Code 权限文档](https://code.claude.com/docs/en/permissions)
- [Claude Code 沙箱文档](https://code.claude.com/docs/en/sandbox)
- [Claude Code 状态行文档](https://code.claude.com/docs/en/statusline)
- [Claude Code 键盘快捷键文档](https://code.claude.com/docs/en/keybindings)
- [Claude Code Hooks 参考](https://code.claude.com/docs/en/hooks)
- [Claude Code 输出样式文档](https://code.claude.com/docs/en/output-styles)
- [Claude Code 设置文档](https://code.claude.com/docs/en/settings)