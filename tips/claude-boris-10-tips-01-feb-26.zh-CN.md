# 10 个使用 Claude Code 的技巧 — 来自 Claude Code 团队

Boris Cherny（[@bcherny](https://x.com/bcherny)），Claude Code 的创造者，于 2026 年 2 月 1 日分享的团队技巧总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Boris 分享了直接来自 Claude Code 团队的使用技巧。团队使用 Claude 的方式与 Boris 个人使用的方式不同。请记住：使用 Claude Code 没有唯一正确的方式 — 每个人的设置都不同。您应该实验并找出适合您的方式！

<a href="https://x.com/bcherny/status/2017742741636321619"><img src="assets/boris-1-feb-26/0.png" alt="Boris Cherny intro tweet" width="50%" /></a>

---

## 1/ 并行做更多事情

同时启动 3-5 个 git worktrees，每个运行自己的 Claude 会话。这是最大的生产力提升，也是团队的最高技巧。Boris 个人使用 multiple git checkouts，但大多数 Claude Code 团队更喜欢 worktrees — 这就是 @amorisscode 将原生支持构建到 Claude Desktop 应用的原因！

有些人还命名他们的 worktrees 并设置 shell 别名（`2a`、`2b`、`2c`），这样他们可以一键切换。其他人有一个专门的"分析"worktree，仅用于读取日志和运行 BigQuery。

参考：[Worktrees 文档](https://code.claude.com/docs/en/common...)

<a href="https://x.com/bcherny/status/2017742743125299476"><img src="assets/boris-1-feb-26/1.png" alt="Do more in parallel" width="50%" /></a>

---

## 2/ 每个复杂任务从计划模式开始

将精力投入到计划中，这样 Claude 可以一次性完成实施。

一个人让一个 Claude 写计划，然后启动第二个 Claude 以高级工程师的身份审查。

另一个人说一旦事情出问题，就切换回计划模式并重新计划。不要继续推进。他们还明确告诉 Claude 在验证步骤时进入计划模式，而不仅仅是构建。

<a href="https://x.com/bcherny/status/2017742745365057733"><img src="assets/boris-1-feb-26/2.png" alt="Start every complex task in plan mode" width="50%" /></a>

---

## 3/ 投资您的 CLAUDE.md

每次纠正后，以"更新您的 CLAUDE.md 以免再犯同样错误"结束。Claude 非常擅长为自己编写规则。

无情地编辑您的 CLAUDE.md。不断迭代，直到 Claude 的错误率明显下降。

一位工程师告诉 Claude 为每个任务/项目维护一个 notes 目录，在每个 PR 后更新。然后他们指向 CLAUDE.md。

<a href="https://x.com/bcherny/status/2017742747067945390"><img src="assets/boris-1-feb-26/3.png" alt="Invest in your CLAUDE.md" width="50%" /></a>

---

## 4/ 创建您自己的技能并提交到 Git

跨每个项目重用。团队的技巧：

- 如果一天做某事超过一次，将其变成技能或命令
- 构建一个 `/techdebt` 斜杠命令，在每次会话结束时运行，以查找和删除重复代码
- 设置一个斜杠命令，将 7 天的 Slack、GDrive、Asana 和 GitHub 同步到一个上下文转储
- 构建类似-analytics-engineer 的 agent，编写 dbt 模型，审查代码并在开发中测试更改

参考：[使用技能扩展 Claude — Claude Code 文档](https://code.claude.com/docs/en/skills)

<a href="https://x.com/bcherny/status/2017742748984742078"><img src="assets/boris-1-feb-26/4.png" alt="Create your own skills" width="50%" /></a>

---

## 5/ Claude 自己修复大多数错误

团队是这样做的：

启用 Slack MCP，然后将 Slack bug 主题粘贴到 Claude 并说"fix"。无需上下文切换。

或者说"去修复失败的 CI 测试"。不要微观管理如何做。

将 Docker 日志指向 Claude 以排查分布式系统 — 这方面的能力出奇地强。

<a href="https://x.com/bcherny/status/2017742750473720121"><img src="assets/boris-1-feb-26/5.png" alt="Claude fixes most bugs by itself" width="50%" /></a>

---

## 6/ 提升您的提示技巧

a. **挑战 Claude**。说"审查这些更改，在通过测试之前不要创建 PR。"让 Claude 成为您的审查者。或者说"向我证明这有效"并让 Claude 在 main 和功能分支之间对比行为。

b. **在中等的修复后**，说："以你现在的了解，废弃这个并实施优雅的解决方案。"

c. **编写详细的规范**并在交接工作前减少歧义。您越具体，输出越好。

<a href="https://x.com/bcherny/status/2017742752566632544"><img src="assets/boris-1-feb-26/6.png" alt="Level up your prompting" width="50%" /></a>

---

## 7/ 终端和环境设置

团队喜欢 Ghostty！多人喜欢其同步渲染、24 位颜色和正确的 unicode 支持。

为了更轻松地管理多个 Claude，使用 `/statusline` 自定义您的状态栏以始终显示上下文使用量和当前 git 分支。许多人还为他们的终端标签着色和命名，有时使用 tmux — 每个标签一个任务/worktree。

使用语音听写。您说话的速度是输入的 3 倍，因此您的提示会变得更详细。（在 macOS 上按 fn x2）

参考：[终端设置文档](https://code.claude.com/docs/en/termin...)

<a href="https://x.com/bcherny/status/2017742753971769626"><img src="assets/boris-1-feb-26/7.png" alt="Terminal and environment setup" width="50%" /></a>

---

## 8/ 使用 Subagents

a. 在您希望 Claude投入更多计算解决问题的任何请求后添加"use subagents"。

b. 将单独的任务卸载给 subagents 以保持您的主 agent 的上下文窗口干净和专注。

c. 通过钩子将权限请求路由到 Opus 4.5 — 让它扫描攻击并自动批准安全的请求。参考：[Hooks 文档](https://code.claude.com/docs/en/hooks#...)

<a href="https://x.com/bcherny/status/2017742755737555434"><img src="assets/boris-1-feb-26/8.png" alt="Use subagents" width="50%" /></a>

---

## 9/ 使用 Claude 进行数据和Analytics

让 Claude Code 使用"bq"CLI 实时提取和分析指标。团队有一个 BigQuery 技能检查到代码库中，每个人都直接在 Claude Code 中使用它进行 Analytics 查询。就个人而言，Boris 已经有 6+ 个月没写一行 SQL 了。

这适用于任何有 CLI、MCP 或 API 的数据库。

<a href="https://x.com/bcherny/status/2017742757666902374"><img src="assets/boris-1-feb-26/9.png" alt="Use Claude for data and analytics" width="50%" /></a>

---

## 10/ 使用 Claude 学习

团队使用 Claude Code 学习的一些技巧：

a. 在 `/config` 中启用"Explanatory"或"Learning"输出样式，让 Claude 解释其更改的原因。

b. 让 Claude 生成一个解释不熟悉代码的 HTML 演示。它能做出惊人的好幻灯片！

c. 让 Claude 为新协议和代码库绘制 ASCII 图，以帮助您理解。

d. 构建一个间隔重复学习技能：您解释您的理解，Claude 提出后续问题填补空白，存储结果。

<a href="https://x.com/bcherny/status/2017742759218794768"><img src="assets/boris-1-feb-26/10.png" alt="Learning with Claude" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) on X — 2026 年 2 月 1 日](https://x.com/bcherny/status/2017742741636321619)