# 代码审查和时间计算 — 来自 Boris Cherny 的技巧

Boris Cherny（[@bcherny](https://x.com/bcherny)），Claude Code 的创造者，于 2026 年 3 月 10 日分享的见解总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 1/ 介绍代码审查

Claude Code 中的新功能：**Code Review**。一个 agent 团队对每个 PR 进行深入审查。

- 首先为 Anthropic 自己的团队构建 — 每个工程师的代码输出今年**增加了 200%**，而审查是瓶颈
- Boris 已经使用了几周，发现它能捕获许多他否则不会注意到的真正错误
- 当 PR 打开时，Claude 派遣一个 agent 团队来寻找错误

<a href="https://x.com/bcherny/status/2031089411820228645"><img src="assets/boris-10-mar-26/0.png" alt="Boris Cherny announcing Code Review" width="50%" /></a>

---

## 2/ 时间计算和多个上下文窗口

粗略地说，您投入编码问题的 tokens 越多，结果越好。Boris 称之为**时间计算**。

- 使用**单独的上下文窗口**使结果更好 — 这正是使 subagents 工作的原因，以及为什么一个 agent 可以造成错误而另一个（使用相同的模型）可以找到它们
- 类似于工程团队：如果 Boris 造成一个错误，审查他的代码的同事可能比他更可靠地找到它
- 在极限情况下，agents 可能会编写完美的无错误代码 — 在那之前，**多个不相关的上下文窗口**往往是一个好的方法

<a href="https://x.com/bcherny/status/2031151689219321886"><img src="assets/boris-10-mar-26/1.png" alt="Boris Cherny on test time compute" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) on X — 2026 年 3 月 10 日](https://x.com/bcherny/status/2031089411820228645)