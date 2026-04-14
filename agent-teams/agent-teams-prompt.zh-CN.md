创建一个 agent 团队来构建时间编排工作流，将当前迪拜时间显示为可视化 SVG 卡。工作流遵循命令 → Agent → 技能架构模式：

- 命令编排流程并处理用户交互
- Agent 使用预加载技能获取迪拜的实时当前时间
- 技能从获取的数据创建可视化 SVG 时间卡

重要：所有文件必须创建在 agent-teams/.claude/ 中 — 不是在仓库根目录的 .claude/ 中。这使 agent 团队的输出自包含并可通过 cd agent-teams && claude 运行。
不要引用或复制现有的天气工作流 — 从头开始构建一切。

分配这些团队成员：

1. 命令架构师 — 设计和实施 /time-orchestrator 命令在 agent-teams/.claude/commands/time-orchestrator.md。命令应该：
   - 通过 Agent 工具（非 bash）调用 time-agent 获取迪拜、阿联酋的当前时间（Asia/Dubai 时区，UTC+4）
   - 通过 Skill 工具调用 time-svg-creator 技能以从获取的时间数据渲染 SVG 卡
   - 在头中使用 model: haiku
   - 包含关键要求：顺序流、正确的工具使用（Agent 工具用于 agent，Skill 工具用于技能）和输出摘要
   通过共享任务列表与其他团队成员协调，同意数据契约（{time, timezone, formatted}）在组件之间传递。

2. Agent 工程师 — 设计和实施 time-agent 在 agent-teams/.claude/agents/time-agent.md 及其预加载的 time-fetcher 技能在 agent-teams/.claude/skills/time-fetcher/SKILL.md。agent 应该：
   - 使用 Bash 获取迪拜当前时间（Asia/Dubai，UTC+4），命令为 TZ='Asia/Dubai' date '+%Y-%m-%d %H:%M:%S %Z'
   - 将时间值、时区名称和格式化字符串返回给命令
   - 使用头：tools（Bash）、model: haiku、color: blue、maxTurns: 3
   - 通过 skills 字段预加载 time-fetcher 技能
   time-fetcher 技能（agent-teams/.claude/skills/time-fetcher/SKILL.md）应包含迪拜时间的 bash 命令、预期输出格式，并设置 user-invocable: false，因为它是 agent 专用的领域知识。
   将约定的数据契约发布到共享任务列表，以便命令架构师和技能设计师可以对齐接口。

3. 技能设计师 — 设计和实施 time-svg-creator 技能在 agent-teams/.claude/skills/time-svg-creator/SKILL.md，以及支持文件 reference.md（SVG 模板 + 输出模板）和 examples.md（示例输入/输出对）。技能应该：
   - 从调用上下文接收时间值、时区和格式化字符串
   - 为迪拜创建显示当前时间的自包含 SVG 时间卡
   - 将 SVG 写入 agent-teams/output/dubai-time.svg
   - 将 markdown 摘要写入 agent-teams/output/output.md
   - 使用提供的确切时间 — 永不重新获取
   - 将模板保留在 reference.md（带占位符的 SVG 标记、markdown 输出模板）和 examples.md 中的示例对中
   还要创建 agent-teams/output/ 目录用于输出文件。

所有三个团队成员都应在共享任务列表中创建任务来协调数据契约：agent 返回 {time, timezone, formatted}，命令通过上下文传递它，技能消费它。
同时启动所有三个，因为组件是独立的 — 它们只需要同意数据接口，无需等待彼此的实现。