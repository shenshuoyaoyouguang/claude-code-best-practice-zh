# 我如何使用 Claude Code — 来自 Boris Cherny 的 13 个技巧

Boris Cherny（[@bcherny](https://x.com/bcherny)），Claude Code 的创造者，于 2026 年 1 月 3 日分享的设置技巧总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Boris 分享了他个人的 Claude Code 设置，注意到它"出人意料地 vanilla" — Claude Code 开箱即用效果很好，所以他不太自定义它。使用它没有唯一正确的方式：团队故意构建它让您可以按照自己喜欢的方式使用、自定义和破解。Claude Code 团队的每个人使用它的方式都非常不同。

<a href="https://x.com/bcherny/status/2007179832300581177"><img src="assets/boris-3-jan-26/0.png" alt="Boris Cherny intro tweet" width="50%" /></a>

---

## 1/ 并行运行 5 个 Claude

在终端中并行运行 5 个 Claude。给您的标签编号 1-5，并使用系统通知知道何时需要输入。

参考：[终端设置文档](https://code.claude.com/docs/en/terminal)

<a href="https://x.com/bcherny/status/2007179833990885678"><img src="assets/boris-3-jan-26/1.png" alt="Run 5 Claudes in parallel" width="50%" /></a>

---

## 2/ 使用 claude.ai/code 获得更多并行性

在 claude.ai/code 上并行运行 5-10 个 Claude，并与您的本地 Claude 会话。使用 `claude.ai/code` 将本地会话交接到 Web 会话，在 Chrome 中手动启动会话，并在两者之间切换。

<a href="https://x.com/bcherny/status/2007179836704600237"><img src="assets/boris-3-jan-26/2.png" alt="claude.ai/code parallelism" width="50%" /></a>

---

## 3/ 所有事情都使用 Opus 和 Thinking

所有事情都使用 Opus 4.5 和 thinking。这是 Boris 用过的最好的编码模型 — 尽管它比 Sonnet 更大更慢，由于您需要更少地引导它且它更好地使用工具，最终几乎总是比使用更小的模型更快。

<a href="https://x.com/bcherny/status/2007179838864666847"><img src="assets/boris-3-jan-26/3.png" alt="Opus with thinking" width="50%" /></a>

---

## 4/ 与您的团队共享单个 CLAUDE.md

为仓库共享单个 `CLAUDE.md`。将其检查到 git 中，并让整个团队每周贡献多次。每当 Claude 做错了什么，就添加到 `CLAUDE.md` 中，这样下次它就不会再做同样的事情。

<a href="https://x.com/bcherny/status/2007179840848597422"><img src="assets/boris-3-jan-26/4.png" alt="Shared CLAUDE.md" width="50%" /></a>

---

## 5/ 在 PR 上标记 @claude 以更新 CLAUDE.md

在代码审查期间，在同事的 PR 上标记 `@claude` 以将内容添加到 `CLAUDE.md` 中作为 PR 的一部分。使用 Claude Code GitHub action（[install-@hub-action](https://github.com/apps/claude)）来执行 — 这是 Boris 的复合工程版本。

<a href="https://x.com/bcherny/status/2007179842928947333"><img src="assets/boris-3-jan-26/5.png" alt="Tag @claude on PRs" width="50%" /></a>

---

## 6/ 大多数会话从计划模式开始

大多数会话从计划模式开始（shift+tab 两次）。如果目标是编写 Pull Request，使用计划模式并与 Claude 来 回直到您喜欢它的计划。从那里切换到自动接受编辑模式，Claude 通常可以一次性完成。一个好的计划非常重要。

<a href="https://x.com/bcherny/status/2007179845336527000"><img src="assets/boris-3-jan-26/6.png" alt="Plan mode" width="50%" /></a>

---

## 7/ 使用斜杠命令进行内部循环工作流

为您每天做多次的每个"内部循环"工作流使用斜杠命令。这节省了重复提示，并使 Claude 可以使用这些工作流。命令被检查到 git 中并住在 `.claude/commands/`。

示例：`/commit-push-pr` — 提交、推送并打开一个 PR。

<a href="https://x.com/bcherny/status/2007179847949500714"><img src="assets/boris-3-jan-26/7.png" alt="Slash commands" width="50%" /></a>

---

## 8/ 使用 Subagents 自动执行常见工作流

定期使用一些 subagents：`code-simplifier` 在 Claude 完成工作后简化代码，`verify-app` 有测试 Claude Code 端到端的详细说明，等等。将 subagents 视为自动化最常见的工作流 — 类似于斜杠命令。

Subagents 住在 `.claude/agents/`。

<a href="https://x.com/bcherny/status/2007179850139000872"><img src="assets/boris-3-jan-26/8.png" alt="Subagents" width="50%" /></a>

---

## 9/ 使用 PostToolUse 钩子自动格式化代码

使用 `PostToolUse` 钩子格式化 Claude 的代码。Claude 通常开箱即生成格式良好的代码，钩子处理最后 10% 以避免以后在 CI 中出现格式化错误。

```json
"PostToolUse": [
  {
    "matcher": "Write|Edit",
    "hooks": [
      {
        "type": "command",
        "command": "bun run format || true"
      }
    ]
  }
]
```

<a href="https://x.com/bcherny/status/2007179852047335529"><img src="assets/boris-3-jan-26/9.png" alt="PostToolUse hook for formatting" width="50%" /></a>

---

## 10/ 预先允许权限而不是 --dangerously-skip-permissions

不要使用 `--dangerously-skip-permissions`。相反，使用 `/permissions` 预先允许您知道在环境中安全的常见 bash 命令，以避免不必要的权限提示。大多数这些被检查到 `.claude/settings.json` 中并与团队共享。

<a href="https://x.com/bcherny/status/2007179854077407667"><img src="assets/boris-3-jan-26/10.png" alt="Pre-allow permissions" width="50%" /></a>

---

## 11/ 让 Claude 通过 MCP 使用您的所有工具

Claude Code 使用您所有的工具。它经常通过 MCP 服务器搜索和发布到 Slack，运行 BigQuery 查询以回答 Analytics 问题（使用 `bq` CLI），从 Sentry 获取错误日志，等等。Slack MCP 配置被检查到 `.mcp.json` 中并与团队共享。

<a href="https://x.com/bcherny/status/2007179856266789204"><img src="assets/boris-3-jan-26/11.png" alt="MCP tools" width="50%" /></a>

---

## 12/ 使用后台代理验证长时间运行的任务

对于非常长时间运行的任务，要么 (a) 提示 Claude 在完成后使用后台代理验证其工作，(b) 使用 agent Stop 钩子更确定性地执行此操作，或 (c) 使用 ralph-wiggum 插件（最初由 @GeoffreyHuntley 构思）。

<a href="https://x.com/bcherny/status/2007179858435281082"><img src="assets/boris-3-jan-26/12.png" alt="Long-running tasks verification" width="50%" /></a>

---

## 13/ 给 Claude 一种验证其工作的方式

可能是在 Claude Code 上获得出色结果的最重要的事情 — 给 Claude 一种验证其工作的方式。如果 Claude 有那个反馈循环，它会将最终结果的质量提高 2-3 倍。

Boris 检验他发布的每一个更改。

<a href="https://x.com/bcherny/status/2007179861115511237"><img src="assets/boris-3-jan-26/13.png" alt="Give Claude a way to verify" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) on X — 2026 年 1 月 3 日](https://x.com/bcherny/status/2007179832300581177)