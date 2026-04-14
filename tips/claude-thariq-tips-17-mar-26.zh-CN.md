# 构建 Claude Code 的经验：如何使用技能 — Thariq

Thariq（[@trq212](https://x.com/trq212)）于 2026 年 3 月 17 日分享的关于 Anthropic 内部如何使用技能的全面指南。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

技能已成为 Claude Code 中最常用的扩展点之一。它们灵活、易于制作且易于分发。但这种灵活性也使得很难知道什么最好。Thariq 分享了在 Anthropic 广泛使用技能的经验教训，目前有数百个技能正在积极使用。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/1.png" alt="Thariq intro tweet" width="50%" /></a>

---

## 什么是技能？

一个常见的误解是技能"Just markdown files"，但最有趣的部分是它们是**文件夹**，可以包含脚本、资产、数据等 — agent 可以发现、探索和操作的东西。技能还具有广泛的配置选项，包括注册动态 hooks。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/2.png" alt="What are Skills?" width="50%" /></a>

---

## 技能类型

在对所有技能进行分类后，团队注意到它们聚集在 9 个重复出现的类别中。最好的技能清晰地适合一个；更混乱的技能跨越多个。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/3.png" alt="Types of Skills grid" width="50%" /></a>

---

### 1/ 库和 API 参考

解释如何正确使用库、CLI 或 SDK 的技能。这些可能是 Claude Code 有时遇到困难的内部库或常见库。它们通常包含参考代码片段文件夹和在编写脚本时要避免的陷阱列表。

**示例：**billing-lib、internal-platform-cli、frontend-design

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/4.png" alt="Library & API Reference" width="50%" /></a>

---

### 2/ 产品验证

描述如何测试或验证您的代码工作的技能。这些通常与 Playwright、tmux 等外部工具配对。验证技能对于确保 Claude 的输出正确非常有价值。让工程师花一周时间使您的验证技能变得出色是值得的。

**示例：**signup-flow-driver、checkout-verifier、tmux-cli-driver

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/5.png" alt="Product Verification" width="50%" /></a>

---

### 3/ 数据获取和分析

连接到您的数据监控堆栈的技能。这些可能包括带凭证获取数据的库、特定仪表板 ID 等，以及常见工作流或获取数据方式的说明。

**示例：**funnel-query、cohort-compare、grafana

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/6.png" alt="Data Fetching & Analysis" width="50%" /></a>

---

### 4/ 业务流程和团队自动化

将重复工作流自动化为一个命令的技能。这些通常是相当简单的指令，但可能对其他技能或 MCP 有更复杂的依赖关系。在日志文件中保存先前可以帮助模型保持一致并反思工作流的先前执行。

**示例：**standup-post、create-<ticket-system>-ticket、weekly-recap

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/7.png" alt="Business Process & Team Automation" width="50%" /></a>

---

### 5/ 代码脚手架和模板

为代码库中的特定功能生成框架样板文件的技能。您可以将这些技能与可组合的脚本结合使用。当您的脚手架有不能纯粹用代码覆盖的自然语言需求时，它们特别有用。

**示例：**new-<framework>-workflow、new-migration、create-app

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/8.png" alt="Code Scaffolding & Templates" width="50%" /></a>

---

### 6/ 代码质量和审查

在组织内执行代码质量并帮助审查代码的技能。这些可以包括确定性脚本或工具以获得最大健壮性。您可能希望将这些技能自动作为 hooks 的一部分或 GitHub Action 内运行。

**示例：**adversarial-review、code-style、testing-practices

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/10.png" alt="Code Quality & Review" width="50%" /></a>

---

### 7/ CI/CD 和部署

帮助您在代码库中获取、推送和部署代码的技能。这些技能可能引用其他技能来收集数据。

**示例：**babysit-pr、deploy-<service>、cherry-pick-prod

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/11.png" alt="CI/CD & Deployment" width="50%" /></a>

---

### 8/ 运行手册

接受症状（如 Slack 主题、警报或错误签名）并进行多工具调查并生成结构化报告的技能。

**示例：**<service>-debugging、oncall-runner、log-correlator

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/12.png" alt="Runbooks" width="50%" /></a>

---

### 9/ 基础设施运营

执行常规维护和运营程序（其中一些涉及受益于guardrails的破坏性操作）的技能。这些使工程师更容易在关键操作中遵循最佳实践。

**示例：**<resource>-orphans、dependency-management、cost-investigation

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/13.png" alt="Infrastructure Operations" width="50%" /></a>

---

## 制作技能的技巧

编写有效技能的 9 个最佳实践，加上分发和测量指南。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/14.png" alt="Tips for Making Skills grid" width="50%" /></a>

---

### 技巧 1：不要陈述显而易见的事情

Claude Code 了解很多关于您的代码库的知识，Claude 也了解很多关于编码的知识，包括许多默认意见。如果您发布的技能主要是关于知识的，请尝试专注于将 Claude 从其正常思维方式中push出去的信息。frontend design 技能是一个很好的例子 — 它是通过与客户迭代以改善 Claude 的设计品味而构建的，避免经典模式如 Inter 字体和紫色渐变。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/15.png" alt="Don't State the Obvious" width="50%" /></a>

---

### 技巧 2：构建陷阱部分

任何技能中最高信号的内容是 Gotchas 部分。这些部分应该从 Claude 使用您的技能时遇到的常见失败点构建而成。理想情况下，您会随着时间的推移更新您的技能以捕获这些陷阱。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/16.png" alt="Build a Gotchas Section" width="50%" /></a>

---

### 技巧 3：使用文件系统和渐进式披露

一个技能是一个文件夹，而不仅仅是一个 markdown 文件。您应该将整个文件系统视为一种上下文工程和渐进式披露形式。告诉 Claude 您的技能中有哪些文件，它会在适当的时候读取它们。最简单的形式是指向其他 markdown 文件 — 例如，将详细的函数签名和使用示例拆分到 `references/api.md`。您可以拥有 references、scripts、examples 等的文件夹。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/17.png" alt="Progressive Disclosure" width="50%" /></a>

---

### 技巧 4：避免railroad Claude

Claude 通常会尝试遵守您的指令，并且因为技能如此可重用，您需要小心过于具体。给 Claude 它需要的信息，但给它适应情况的灵活性。与其规定性的分步说明，不如给出目标和约束。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/18.png" alt="Avoid Railroading Claude" width="50%" /></a>

---

### 技巧 5：考虑设置

一些技能可能需要从用户设置上下文。一个好的模式是在技能目录中存储此设置信息到 `config.json` 文件。如果未设置配置，agent 可以向用户询问信息。您可以指示 Claude 使用 AskUserQuestion 工具进行结构化的多项选择题。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/19.png" alt="Think through the Setup" width="50%" /></a>

---

### 技巧 6：描述字段是为模型编写的

当 Claude Code 开始会话时，它会构建每个可用技能及其描述的列表。这个列表是 Claude 扫描以决定"这个请求有技能吗？"的内容。这意味着描述字段不是总结 — 而是**何时触发**这个技能的描述。为模型编写它。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/20.png" alt="Description = Trigger" width="50%" /></a>

---

### 技巧 7：Memory 和存储数据

一些技能可以通过在技能中存储数据来包含某种形式的 memory。您可以将数据存储在简单的追加日志文件或 JSON 文件中，或复杂的 SQLite 数据库中。存储在技能目录中的数据在升级技能时可能会被删除，因此请使用 `${CLAUDE_PLUGIN_DATA}` 作为每个插件的稳定文件夹来存储数据。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/21.png" alt="Memory & Storing Data" width="50%" /></a>

---

### 技巧 8：存储脚本和生成代码

您可以给 Claude 的最强大的工具之一是代码。给 Claude 脚本和库让 Claude 将其轮次花在组合上，决定下一步做什么，而不是重构样板代码。然后 Claude 可以动态生成脚本以组合此功能进行更高级的分析。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/22.png" alt="Store Scripts & Generate Code" width="50%" /></a>

---

### 技巧 9：按需 Hooks

技能可以包含仅在调用技能时激活的 hooks，并持续到会话结束。使用这对于您不想一直运行但有时非常有用的更固执的 hooks。

**示例：**
- `/careful` — 通过 PreToolUse matcher 阻止 rm -rf、DROP TABLE、force-push、kubectl delete
- `/freeze` — 阻止任何不在特定目录中的 Edit/Write

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/23.png" alt="On Demand Hooks" width="50%" /></a>

---

## 分发技能

与团队分享技能的两种方式：
- **检查到您的仓库**（在 `.claude/skills` 下）— 适用于在相对较少的仓库中工作的小型团队
- **制作一个插件**并让用户可以上传和安装插件的 Claude Code Plugin 市场

每个检查到的技能也会为模型的上下文增加一点。随着您扩展，内部插件市场允许您分发技能并让您的团队决定安装哪些。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/24.png" alt="Distributing Skills" width="50%" /></a>

---

## 管理市场

没有集中式团队决定哪些技能进入市场。相反，尝试有机地找到最有用的技能。上传到 GitHub 中的沙箱文件夹并在 Slack 或其他论坛中指向人们。一旦技能获得了牵引力（由技能所有者决定），他们可以提交 PR 将其移入市场。发布前的策展对于避免冗余技能很重要。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/25.png" alt="Managing a Marketplace" width="50%" /></a>

---

## 组合技能

您可能希望有相互依赖的技能。例如，一个文件上传技能上传文件，以及一个生成 CSV 并上传的 CSV 生成技能。这种依赖管理尚未原生内置到市场或技能中，但您可以按名称引用其他技能，如果已安装，模型将调用它们。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/26.png" alt="Composing Skills" width="50%" /></a>

---

## 测量技能

要了解技能的表现，使用 PreToolUse hook 记录公司内的技能使用情况。这意味着您可以找到比预期更受欢迎或触发不足的技能。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/27.png" alt="Measuring Skills" width="50%" /></a>

---

## 结论

技能是代理的非常强大、灵活的工具，但现在还处于早期，我们都在弄清楚如何最好地使用它们。把这更多地视为我们见过的有用的技巧袋，而不是权威指南。了解技能的最佳方式是开始、实验并查看什么适合您。我们的大多数从一个几行和一个单一的陷阱开始，并随着人们在 Claude 遇到新的边缘情况时不断添加而变得更好。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-17-mar-26/28.png" alt="Conclusion" width="50%" /></a>

---

## 来源

- [Thariq (@trq212) on X — 2026 年 3 月 17 日](https://x.com/trq212/status/2033949937936085378)
- [Skilljar — Agent Skills 课程](https://code.claude.com/docs/en/skills)
- [技能创建器](https://code.claude.com/docs/en/skills)