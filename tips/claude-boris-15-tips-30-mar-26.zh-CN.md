# Claude Code 中 15 个隐藏和未被充分利用的功能 — 来自 Boris Cherny

Boris Cherny（[@bcherny](https://x.com/bcherny)），Claude Code 的创造者，于 2026 年 3 月 30 日分享的技巧总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Boris 分享了他在 Claude Code 中最喜欢的一些隐藏和未被充分利用的功能，专注于他最常使用的功能。

<a href="https://x.com/bcherny/status/2038454336355999749"><img src="assets/boris-30-mar-26/0.png" alt="Boris Cherny intro tweet" width="50%" /></a>

---

## 1/ Claude Code 有移动应用

您知道 Claude Code 有移动应用吗？Boris 在 iOS 应用中编写了很多代码 — 这是一种无需打开笔记本电脑即可进行更改的便捷方式。

- 为 iOS/Android 下载 Claude 应用
- 导航到左侧的 **Code** 选项卡
- 您可以直接从手机审查更改、批准 PR 和编写代码

<a href="https://x.com/bcherny/status/2038454337811386436"><img src="assets/boris-30-mar-26/1.png" alt="Claude Code mobile app" width="50%" /></a>

---

## 2/ 在移动应用/Web/桌面和终端之间移动会话

运行 `claude --teleport` 或 `/teleport` 在您的机器上继续云会话。或者运行 `/remote-control` 以从手机或 Web 控制本地运行的会话。

- **Teleport**：将云会话拉回本地终端
- **Remote Control**：让您从任何设备控制本地会话
- Boris 在他的 `/config` 中设置了**"为所有会话启用远程控制"**

<a href="https://x.com/bcherny/status/2038454339933548804"><img src="assets/boris-30-mar-26/2.png" alt="Teleport and Remote Control" width="50%" /></a>

---

## 3/ /loop 和 /schedule — 最强大的两个功能

使用这些来安排 Claude 按设定的时间间隔自动运行，最长达一周。Boris 本地运行了很多循环：

- `/loop 5m /babysit` — 自动处理代码审查、自动变基并将 PR 引向生产
- `/loop 30m /slack-feedback` — 每 30 分钟自动将 PR 提交到 Slack 反馈
- `/loop /post-merge-sweeper` — 提交 PR 以处理他错过的代码审查评论
- `/loop 1h /pr-pruner` — 关闭过时和不再需要的 PR
- ……还有很多！

尝试将工作流转化为技能 + 循环。这很强大。

<a href="https://x.com/bcherny/status/2038454341884154269"><img src="assets/boris-30-mar-26/3.png" alt="/loop and /schedule" width="50%" /></a>

---

## 4/ 使用 Hooks 确定性地运行逻辑

使用 hooks 作为代理生命周期的一部分运行逻辑。例如：

- 每次启动 Claude 时**动态加载**上下文（`SessionStart`）
- **记录模型运行的每个 bash 命令**（`PreToolUse`）
- 将权限提示**路由**到 WhatsApp 由您批准/拒绝（`PermissionRequest`）
- **每当 Claude 停止时推动**它继续（`Stop`）

<a href="https://x.com/bcherny/status/2038454343519932844"><img src="assets/boris-30-mar-26/4.png" alt="Use hooks" width="50%" /></a>

---

## 5/ Cowork Dispatch

Boris 每天使用 Dispatch 来跟进 Slack 和电子邮件、管理文件，以及不在电脑前时在笔记本电脑上做事情。当他不编码时，他在分发任务。

- Dispatch 是 Claude 桌面应用的**安全远程控制**
- 它可以在您的许可下使用您的 MCPs、浏览器和电脑
- 把它想象成一种从任何地方将非编码任务委托给 Claude 的方式

<a href="https://x.com/bcherny/status/2038454345419936040"><img src="assets/boris-30-mar-26/5.png" alt="Cowork Dispatch" width="50%" /></a>

---

## 6/ 使用 Chrome 扩展进行前端工作

使用 Claude Code 最重要的技巧：**给 Claude 一种验证其输出的方式。**一旦这样做，Claude 会迭代直到结果变得很棒。

- 想想就像让 someone 建网站但不允许使用浏览器 — 结果可能看起来不好
- 给 Claude 一个浏览器，它会编写代码并迭代直到看起来很好
- Boris 每次做 web 代码工作时都使用 Chrome 扩展 — 它往往比其他类似的 MCP 更可靠

<a href="https://x.com/bcherny/status/2038454347156398333"><img src="assets/boris-30-mar-26/6.png" alt="Chrome extension for frontend" width="50%" /></a>

---

## 7/ 使用 Claude 桌面应用自动启动和测试 Web 服务器

同样，桌面应用内置了让 Claude **自动运行您的 Web 服务器甚至在内置浏览器中测试它**的能力。

- 您可以在 CLI 或 VSCode 中使用 Chrome 扩展设置类似的东西
- 或者只为集成体验使用桌面应用

<a href="https://x.com/bcherny/status/2038454348804714642"><img src="assets/boris-30-mar-26/7.png" alt="Desktop app web server testing" width="50%" /></a>

---

## 8/ 分支您的会话

人们经常问如何分支现有会话。有两种方式：

1. 从您的会话运行 `/branch`
2. 从 CLI 运行 `claude --resume <session-id> --fork-session`

`/branch` 创建一个分支的对话 — 您现在在分支中。要恢复原始的，使用 `claude -r <original-session-id>`。

<a href="https://x.com/bcherny/status/2038454350214041740"><img src="assets/boris-30-mar-26/8.png" alt="Fork your session" width="50%" /></a>

---

## 9/ 使用 /btw 进行边查询

Boris 经常用这个在 agent 工作时回答快速问题。`/btw` 让您无需中断 agent 的当前任务即可提出边问题。

示例：
```
/btw 怎么拼写 dachshund？
> dachshund — 德语"獾狗"（dachs + badger, hund + dog）。
↑/↓ 滚动 · 空格、Enter 或 Escape 关闭
```

<a href="https://x.com/bcherny/status/2038454351849787485"><img src="assets/boris-30-mar-26/9.png" alt="/btw for side queries" width="50%" /></a>

---

## 10/ 使用 Git Worktrees

Claude Code 原生支持 git worktrees。Worktrees 对于在同一仓库中并行做大量工作至关重要。Boris 有**数十个 Claude 同时运行**，这就是他做事的方式。

- 使用 `claude -w` 在 worktree 中启动新会话
- 或者在 Claude 桌面应用中点击 **"worktree" 复选框**
- 对于非 git VCS 用户，使用 `WorktreeCreate` hook 添加您自己的 worktree 创建逻辑

<a href="https://x.com/bcherny/status/2038454353787519164"><img src="assets/boris-30-mar-26/10.png" alt="Git worktrees" width="50%" /></a>

---

## 11/ 使用 /batch 大规模分发更改集

`/batch` 询问您，然后让 Claude 将工作分发到尽可能多的 **worktree agents**（数十、数百甚至数千）来完成。

- 用于大型代码迁移和其他可并行化的工作
- 每个 worktree agent 独立地在自己的代码库副本上工作

<a href="https://x.com/bcherny/status/2038454355469484142"><img src="assets/boris-30-mar-26/11.png" alt="/batch for massive changesets" width="50%" /></a>

---

## 12/ 使用 --bare 将 SDK 启动速度提升高达 10 倍

默认情况下，当您运行 `claude -p`（或 TypeScript 或 Python SDK）时，Claude 搜索本地 CLAUDE.md、设置和 MCP。但对于非交互式使用，您通常想通过 `--system-prompt`、`--mcp-config`、`--settings` 等明确指定要加载的内容。

- 这是 SDK 最初构建时的设计疏忽
- 在未来版本中，他们将默认切换到 `--bare`
- 现���，��用标志选择加入以获得高达 **10 倍更快的启动**

```bash
claude -p "summarize this codebase" \
    --output-format=stream-json \
    --verbose \
    --bare
```

<a href="https://x.com/bcherny/status/2038454357088457168"><img src="assets/boris-30-mar-26/12.png" alt="--bare flag for SDK startup" width="50%" /></a>

---

## 13/ 使用 --add-dir 让 Claude 访问更多文件夹

跨多个仓库工作时，Boris 通常在一个仓库中启动 Claude 并使用 `--add-dir`（或 `/add-dir`）让 Claude 看到其他仓库。

- 这不仅告诉 Claude 关于仓库，还**给它在该仓库中工作的权限**
- 或者，将 `"additionalDirectories"` 添加到您团队的 `settings.json` 以在启动 Claude Code 时始终加载额外文件夹

<a href="https://x.com/bcherny/status/2038454359047156203"><img src="assets/boris-30-mar-26/13.png" alt="--add-dir for multiple repos" width="50%" /></a>

---

## 14/ 使用 --agent 给 Claude Code 自定义系统提示和工具

自定义 agents 是一个经常被忽视的强大原语。要使用它，只需在 `.claude/agents/` 中定义一个新 agent，然后运行：

```bash
claude --agent=<您的 agent 名称>
```

- Agents 可以有受限的工具、自定义描述和特定模型
- 它们非常适合创建只读 agents、专业审查 agents 或特定领域的工具

<a href="https://x.com/bcherny/status/2038454360418787764"><img src="assets/boris-30-mar-26/14.png" alt="--agent for custom system prompts" width="50%" /></a>

---

## 15/ 使用 /voice 启用语音输入

有趣的事实：Boris 大部分编码是通过对 Claude 说话而不是输入来完成的。

- 在 CLI 中运行 `/voice` 然后按住空格键说话
- 在桌面上按语音按钮
- 或者在 iOS 设置中启用听写

<a href="https://x.com/bcherny/status/2038454362226467112"><img src="assets/boris-30-mar-26/15.png" alt="/voice for voice input" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) on X — 2026 年 3 月 30 日](https://x.com/bcherny/status/2038454336355999749)