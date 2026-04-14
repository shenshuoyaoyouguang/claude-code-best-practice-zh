# Claude Agent SDK 与 Claude CLI：系统提示和输出一致性

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

![SDK 与 CLI 系统提示图表](assets/sdk-vs-cli-diagram.svg)

---

## 执行摘要

当通过 **Claude Agent SDK** 和 **Claude CLI（Claude Code）** 发送相同的消息（例如"挪威的首都是什么？"）时，随附的系统提示本质上是不同的。CLI 使用**模块化系统提示架构**（约 269 个基础令牌，根据功能按条件加载额外上下文），而 SDK 默认使用最小提示。**无法保证两者产生相同的输出**，即使配置相同也无法保证，因为缺少种子参数且 Claude 架构本身具有非确定性。

---

## 1. 系统提示比较

### Claude CLI（Claude Code）

Claude CLI 使用**模块化系统提示架构**，具有约 269 个令牌的基础提示，根据功能按条件加载额外上下文：

| 组件 | 描述 | 加载方式 |
|-----------|-------------|---------|
| **基础系统提示** | 核心指令和行为 | 始终（约 269 令牌）|
| **工具指令** | 18+ 个内置工具（Write、Read、Edit、Bash、TodoWrite 等）| 始终 |
| **编码指南** | 代码风格、格式规则、安全实践 | 始终 |
| **安全规则** | 拒绝规则、注入防御、伤害预防 | 始终 |
| **响应风格** | 语气、详细程度、解释深度、表情符号使用 | 始终 |
| **环境上下文** | 工作目录、git 状态、平台信息 | 始终 |
| **项目上下文** | CLAUDE.md 内容、设置、hooks 配置 | 按条件 |
| **子代理提示** | Plan 模式、Explore 代理、Task 代理 | 按条件 |
| **安全审查** | 扩展安全指令（约 2,610 令牌）| 按条件 |

**关键特征：**
- **模块化架构**，110+ 个系统提示字符串按条件加载
- 基础提示不大（约 269 令牌），总计因激活的功能而异
- 包含广泛的安全和注入防御层
- 自动加载工作目录中的 CLAUDE.md 文件
- 交互模式下会话持久上下文

### Claude Agent SDK

Agent SDK 默认使用**包含以下内容的最小系统提示**：

| 组件 | 描述 | 令牌影响 |
|-----------|-------------|--------------|
| **必要工具指令** | 仅显式提供的工具 | 最小 |
| **基本安全** | 最小安全指令 | 最小 |

**关键特征：**
- 默认无编码指南或风格偏好
- 默认无项目上下文（除非显式配置）
- 无广泛的工具描述
- 需要显式配置才能匹配 CLI 行为

---

## 2. 每个接口发送的内容

### 示例："挪威的首都是什么？"

#### 通过 Claude CLI

```
系统提示：[模块化，约 269+ 基础令牌]
├── 基础系统提示（约 269 令牌）
├── 工具指令（Write、Read、Edit、Bash、Grep、Glob 等）
├── Git 安全协议
├── 代码参考指南
├── 专业客观性指令
├── 安全和注入防御规则
├── 环境上下文（操作系统、目录、日期）
├── CLAUDE.md 内容（如存在）[按条件]
├── MCP 工具描述（如配置）[按条件]
├── Plan/Explore 模式提示 [按条件]
└── 会话/对话上下文

用户消息："挪威的首都是什么？"
```

#### 通过 Claude Agent SDK（默认）

```
系统提示：[最小]
├── 必要工具指令（如提供任何工具）
└── 基本操作上下文

用户消息："挪威的首都是什么？"
```

#### 通过 Agent SDK（使用 `claude_code` 预设）

```typescript
const response = await query({
  prompt: "挪��的首都是什么？",
  options: {
    systemPrompt: {
      type: "preset",
      preset: "claude_code"
    }
  }
});
```

```
系统提示：[模块化，匹配 CLI]
├── 完整 Claude Code 系统提示
├── 工具指令
├── 编码指南
└── 安全规则

// 注意：仍不包含 CLAUDE.md，除非配置了 settingSources
```

---

## 3. 自定义方法

### Claude CLI 自定义

| 方法 | 命令 | 效果 |
|--------|---------|--------|
| **追加到提示** | `claude -p "..." --append-system-prompt "..."` | 添加指令同时保留默认值 |
| **替换提示** | `claude -p "..." --system-prompt "..."` | 完全替换系统提示 |
| **项目上下文** | CLAUDE.md 文件 | 自动加载，持久 |
| **输出风格** | `/output-style [name]` | 应用预定义的响应风格 |

### Agent SDK 自定义

| 方法 | 配置 | 效果 |
|--------|---------------|--------|
| **自定义提示** | `systemPrompt: "..."` | 完全替换默认值（失去工具）|
| **预设加追加** | `systemPrompt: { type: "preset", preset: "claude_code", append: "..." }` | 保留 CLI 功能 + 自定义指令 |
| **CLAUDE.md 加载** | `settingSources: ["project"]` | 加载项目级指令 |
| **输出风格** | `settingSources: ["user"]` 或 `settingSources: ["project"]` | 加载保存的输出风格 |

### 配置比较表

| 功能 | CLI 默认 | SDK 默认 | SDK 预设 |
|---------|-------------|-------------|-----------------|
| 工具指令 | ✅ 完整 | ❌ 最小 | ✅ 完整 |
| 编码指南 | ✅ 是 | ❌ 否 | ✅ 是 |
| 安全规则 | ✅ 是 | ❌ 基本 | ✅ 是 |
| CLAUDE.md 自动加载 | ✅ 是 | ❌ 否 | ❌ 否* |
| 项目上下文 | ✅ 自动 | ❌ 否 | ❌ 否* |

*需要显式配置 `settingSources: ["project"]`

---

## 4. 输出一致性保证

### 关键发现：无法保证确定性

**Claude Messages API 不提供种子参数以实现可重现性。**这是一个基本的架构限制。

### 阻止相同输出的因素

| 因素 | 描述 | 可控？ |
|-----------|-------------|---------------|
| **不同的系统提示** | CLI 与 SDK 有不同的默认值 | ✅ 是（通过配置）|
| **浮点运算** | 并行硬件差异 | ❌ 否 |
| **MoE 路由** | 专家混合架构变体 | ❌ 否 |
| **批处理/调度** | 云基础设施差异 | ❌ 否 |
| **数值精度** | 推理引擎变体 | ❌ 否 |
| **模型快照** | 版本更新/变更 | ❌ 否 |

### 温度和采样

即使使用 `temperature=0.0`（贪心解码）：
- 完整确定性**无法保证**
- 由于基础设施因素仍可能发生微小变化
- 已知问题：[Claude CLI 对于相同输入产生非确定性输出](https://github.com/anthropics/claude-code/issues/3370)

---

## 5. 实现最大一致性

要获得 SDK 和 CLI 之间**尽可能相同**的输出：

### Agent SDK 配置

```typescript
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic();

// 选项 1：使用 claude_code 预设
const response = await client.messages.create({
  model: "claude-sonnet-4-20250514",
  max_tokens: 1024,
  // 尽可能匹配 CLI 系统提示
  system: "你的确切系统提示匹配 CLI",
  messages: [
    { role: "user", content: "挪威的首都是什么？" }
  ],
  // 使用贪心解码以实现最大一致性
  temperature: 0
});

// 选项 2：使用 Agent SDK query 函数
import { query } from "@anthropic-ai/agent-sdk";

for await (const message of query({
  prompt: "挪威的首都是什么？",
  options: {
    systemPrompt: {
      type: "preset",
      preset: "claude_code"
    },
    temperature: 0,
    model: "claude-sonnet-4-20250514",
    // 像 CLI 一样加载项目上下文
    settingSources: ["project"]
  }
})) {
  // 处理响应
}
```

### CLI 配置

```bash
# 尽可能匹配 SDK 配置
claude -p "挪威的首都是什么？" \
  --model claude-sonnet-4-20250514 \
  --temperature 0
```

### 仍然无法保证

即使配置完全匹配：
- 输出可能在不同运行之间不同
- 输出可能在 SDK 和 CLI 之间不同
- 没有种子参数来强制实现可重现性

---

## 6. 实际影响

### 何时使用每个接口

| 使用场景 | 推荐接口 | 原因 |
|----------|----------------------|--------|
| 交互式开发 | Claude CLI | 完整工具套件、项目上下文 |
| 程序化集成 | Agent SDK | 细粒度控制、嵌入 |
| 一致的 API 响应 | Agent SDK + 自定义提示 | 更好地控制系统提示 |
| 批处理 | Agent SDK | 更适合自动化流水线 |
| 一次性任务 | Claude CLI | 更快的设置、即時上下文 |

### 设计建议

1. **不要依赖逐位可重现性**
   - 构建能耐受微小输出变化的应用
   - 使用结构化输出和验证

2. **对于需要一致性的生产流水线：**
   - 尽可能缓存结果
   - 使用带 JSON 模式验证的结构化输出
   - 结合确定 性逻辑和验证
   - 考虑多代共识

3. **对于在 SDK 中匹配 CLI 行为：**
   ```typescript
   systemPrompt: {
     type: "preset",
     preset: "claude_code",
     append: "你的额外指令"
   },
   settingSources: ["project", "user"]
   ```

---

## 7. 系统提示令牌影响

| 配置 | 架构 | 备注 |
|---------------|-------------|-------|
| SDK（最小）| 默认最小 | 仅必要工具指令 |
| SDK（claude_code 预设）| 模块化（约 269+ 基础）| 匹配 CLI，因功能而异 |
| CLI（默认）| 模块化（约 269+ 基础）| 额外上下文按条件加载 |
| CLI（带 MCP 工具）| 模块化 + MCP | MCP 工具描述添加大量令牌 |

**注意：** Claude Code 使用模块化架构，110+ 个系统提示字符串。基础提示约 269 令牌，个别组件从 18 到 2,610 令牌不等，取决于激活的功能。

**含义：** SDK 的最小默认值为你的实际任务提供更多上下文，但以牺牲 Claude Code 的完整功能为代价。

---

## 8. 总结表

| 方面 | Claude CLI | Agent SDK（默认）| Agent SDK（预设）|
|---------|------------|--------------------|--------------------|
| **系统提示** | 模块化（约 269+ 基础）| 最小 | 模块化（匹配 CLI）|
| **包含的工具** | 18+ 内置 | 仅如果提供 | 18+ 内置 |
| **CLAUDE.md 自动加载** | 是 | 否 | 否（需要配置）|
| **编码指南** | 是 | 否 | 是 |
| **安全规则** | 完整 | 基本 | 完整 |
| **温度控制** | 是 | 是 | 是 |
| **确定性保证** | 否 | 否 | 否 |
| **相同输出？** | 不适用 | 否（vs CLI）| 更接近，但不是 |

---

## 9. 结论

**问：SDK 和 CLI 中相同消息附带什么系统提示？**

CLI 使用**模块化系统提示架构**，具有约 269 令牌的基础提示和 110+ 个按条件加载的组件（工具指令、编码指南、安全规则、项目上下文）。SDK 使用**默认最小值**，仅包含必要工具指令，尽管可以使用 `claude_code` 预设配置为匹配 CLI 行为。

**问：有相同输出的保证吗？**

**没有。**即使系统提示相同、输入相同且 `temperature=0`，也无法保证相同输出，原因是：
- Claude API 缺少种子参数
- 浮点运算变体
- 基础设施级非确定性
- 模型架构（专家混合）路由变体

**建议：** 设计系统以能够耐受输出变化，而非依赖确定性行为。对于需要一致性的应用，使用结构化输出、缓存和验证层。

---

## 来源

- [修改系统提示 - Agent SDK](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/sdk#modifying-system-prompts)
- [Claude Code CLI 参考](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/cli)
- [Claude Code 无头模式](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/headless)
- [Claude Code 最佳实践 - Anthropic Engineering](https://www.anthropic.com/engineering/claude-code-best-practices)
- [Claude Messages API 参考](https://docs.anthropic.com/en/api/messages)
- [GitHub 问题 #3370：非确定性输出](https://github.com/anthropics/claude-code/issues/3370)
- [Claude Code 系统提示仓库](https://github.com/Piebald-AI/claude-code-system-prompts) - 模块化提示架构分析
- [为什么 LLM 的确定性输出几乎不可能](https://unstract.com/blog/understanding-why-deterministic-output-from-llms-is-nearly-impossible/)

---

*本报告由 Claude Code 使用 Opus 4.5 模型于 2026 年 2 月 3 日生成。*