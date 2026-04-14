
## 代理 Frontmatter Hooks

Claude Code 2.1.0 引入了在代理 frontmatter 文件中定义的代理特定 hooks 支持。这些 hooks 仅在代理的生命周期内运行，支持部分 hook 事件。

### 支持的代理 Hooks

代理 frontmatter hooks 支持 6 个 hooks（并非全部 27 个）：
- PreToolUse：在代理使用工具前运行
- PostToolUse：在代理完成工具使用后运行
- PermissionRequest：在工具需要用户权限时运行
- PostToolUseFailure：在工具调用失败后运行
- Stop：在代理完成时运行
- SubagentStop：在子代理完成时运行

### 代理声音文件夹

代理特定的声音存储在单独的文件夹中：
- .claude/hooks/sounds/agent_pretooluse/
- .claude/hooks/sounds/agent_posttooluse/
- .claude/hooks/sounds/agent_permissionrequest/
- .claude/hooks/sounds/agent_posttoolusefailure/
- .claude/hooks/sounds/agent_stop/
- .claude/hooks/sounds/agent_subagentstop/

### 创建带 Hooks 的代理

1. 在 .claude/agents/ 中创建代理定义文件

2. 将声音文件添加到代理声音文件夹

### Hook 选项：once: true

once: true 选项确保 hook 仅在每个会话中运行一次：
`json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "timeout": 5000,
  "once": true
}
`

注意：once 选项仅适用于 skills，不是 agents。

### Hook 选项：async: true

Hooks 可以通过添加 async: true 在后台运行而不阻塞 Claude Code 的执行：
`json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "timeout": 5000,
  "async": true
}
`

何时使用 async hooks：
- 日志记录和分析
- 通知和声音效果
- 不应减慢 Claude Code 的任何副作用

## Hook 类型

Claude Code 支持四种 hook 处理程序类型。此项目使用 command hooks 进行所有声音播放。

### type: command（此项目使用）

运行 shell 命令。通过 stdin 接收 JSON，通过退出码和 stdout 传达结果。

### type: prompt

将 prompt 发送给 Claude 模型进行单轮评估。模型返回是/否决定作为 JSON。

### type: agent

生成具有多轮工具访问权限的子代理来验证条件，然后返回决定。

### type: http（自 v2.1.63 起）

POST JSON 到 URL 并接收 JSON 响应，而不是运行 shell 命令。

## 环境变量

Claude Code 为 hook 脚本提供这些环境变量：

| 变量 | 可用性 | 描述 |
|----------|-------------|-------------|
|  | 所有 hooks | 项目根目录 |
|  | SessionStart, CwdChanged, FileChanged | 用于为后续 Bash 命令保留环境变量的文件路径 |
|  | Plugin hooks | 插件的根目录 |
|  | 所有 hooks | 在远程 Web 环境中设置为 true |
|  | Skill hooks | 技能自己的目录 |
|  | Plugin hooks | 插件的持久数据目录 |

### 常见输入字段（stdin JSON）

每个 hook 在 stdin 上接收一个 JSON 对象，包含这些常见字段：

| 字段 | 类型 | 描述 |
|-----|------|-------------|
| hook_event_name | string | 触发的 hook 事件名称 |
| session_id | string | 当前会话标识符 |
| transcript_path | string | 对话 JSON 文件的路径 |
| cwd | string | 当前工作目录 |
| permission_mode | string | 当前权限模式：default, plan, acceptEdits, dontAsk, 或 bypassPermissions |
| agent_id | string | 唯一子代理标识符 |
| agent_type | string | 代理类型名称 |

## Hooks 管理命令

Claude Code 提供用于管理 hooks 的内置命令：
- /hooks — 交互式 hook 管理 UI
- claude hooks reload — 重新加载 hooks 配置

## MCP 工具匹配器

对于 PreToolUse、PostToolUse 和 PermissionRequest hooks，可以使用模式 mcp__<server>__<tool> 匹配 MCP 工具：
`json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "mcp__memory__.*",
      "hooks": [{ "type": "command", "command": "echo 'MCP memory tool used'" }]
    }]
  }
}
`

## 已知问题和解决方法

### 代理 Stop Hook Bug

在代理的 frontmatter 中定义 Stop hook 时，传递给 hook 脚本的 hook_event_name 是 SubagentStop 而不是 Stop。

### PreToolUse updatedInput for AskUserQuestion

当 PreToolUse hook 匹配 AskUserQuestion 时，它可以返回 updatedInput 来自动回答问题。

## 决策控制模式

不同 hooks 使用不同的输出模式来阻止或控制执行：

| Hooks | 控制方法 | 值 |
|---------|---------------|--------|
| PreToolUse | permissionDecision | allow, deny, ask, defer |
| PreToolUse | autoAllow | true |
| PermissionRequest | decision.behavior | allow, deny |
| Stop, SubagentStop, ConfigChange | decision | block |

### 通用 JSON 输出字段

所有 hooks 可以通过 stdout JSON 返回这些字段：

| 字段 | 类型 | 描述 |
|-----|------|-------------|
| continue | bool | 如果为 false，完全停止 Claude |
| stopReason | string | 当 continue 为 false 时显示的消息 |
| suppressOutput | bool | 隐藏详细模式的 stdout |
| systemMessage | string | 显示给用户的警告消息 |
| additionalContext | string | 添加到 Claude 对话的上下文 |

## Hook 去重和外部更改

- Hook 去重：在多个设置位置定义的相同 hook 处理程序仅并行运行一次，防止重复执行
- 外部更改检测：当 hooks 在活动会话期间被外部修改时，Claude Code 会发出警告