# 理解大型 Monorepo 中的 Claude Skills 发现

在 monorepo 中使用 Claude Code 时，了解如何发现技能并将其加载到上下文中，对于有效组织项目特定功能至关重要。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 与 CLAUDE.md 的重要区别

**技能不具备与 CLAUDE.md 文件相同的加载行为。**虽然 CLAUDE.md 文件向上遍历目录树（祖先加载），但技能使用不同的发现机制，专注于项目内的嵌套目录。

## 如何发现技能

### 1. 标准技能位置

技能根据范围从这些固定位置加载：

| 位置 | 路径 | 适用于 |
|----------|------|------------|
| 企业 | 托管设置 | 组织中的所有用户 |
| 个人 | `~/.claude/skills/<技能名称>/SKILL.md` | 你的所有项目 |
| 项目 | `.claude/skills/<技能名称>/SKILL.md` | 仅此项目 |
| 插件 | `<插件>/skills/<技能名称>/SKILL.md` | 插件启用的地方 |

### 2. 从嵌套目录自动发现

当你在子目录中处理文件时，Claude Code 会自动从嵌套的 `.claude/skills/` 目录发现技能。例如，如果你在 `packages/frontend/` 中编辑文件，Claude Code 也会在 `packages/frontend/.claude/skills/` 中查找技能。

这支持包拥有自己的技能的 monorepo 设置。

## 典型 Monorepo 结构

考虑一个带有独立包的典型 monorepo：

```
/mymonorepo/
├── .claude/
│   └── skills/
│       └── shared-conventions/SKILL.md    # 项目级技能
├── packages/
│   ├── frontend/
│   │   ├── .claude/
│   │   │   └── skills/
│   │   │       └── react-patterns/SKILL.md  # 前端专用技能
│   │   └── src/
│   │       └── App.tsx
│   ├── backend/
│   │   ├── .claude/
│   │   │   └── skills/
│   │   │       └── api-design/SKILL.md      # 后端专用技能
│   │   └── src/
│   └── shared/
│       ├── .claude/
│       │   └── skills/
│       │       └── utils-patterns/SKILL.md  # 共享工具技能
│       └── src/
```

## 场景 1：仅在根目录启动 Claude（尚未编辑文件）

当你从 `/mymonorepo` 运行 Claude Code 且尚未编辑任何文件时：

```bash
cd /mymonorepo
claude
# 仅启动 - 尚未编辑文件
```

| 技能 | 在上下文中？ | 原因 |
|-------|-------------|--------|
| `shared-conventions` | **是** | 项目级技能在根 `.claude/skills/` |
| `react-patterns` | **否** | 未发现 - 尚未在 `packages/frontend/` 中处理文件 |
| `api-design` | **否** | 未发现 - 尚未在 `packages/backend/` 中处理文件 |
| `utils-patterns` | **否** | 未发现 - 尚未在 `packages/shared/` 中处理文件 |

## 场景 2：在包中编辑文件后

当你要求 Claude 编辑 `packages/frontend/src/App.tsx` 后：

| 技能 | 在上下文中？ | 原因 |
|-------|-------------|--------|
| `shared-conventions` | **是** | 项目级技能在根 `.claude/skills/` |
| `react-patterns` | **是** | 在 `packages/frontend/` 中编辑文件时发现 |
| `api-design` | **否** | 仍未发现 - 尚未在 `packages/backend/` 中处理文件 |
| `utils-patterns` | **否** | 仍未发现 - 尚未在 `packages/shared/` 中处理文件 |

**关键洞察：** 嵌套技能在你在这些目录中处理文件时**按需发现**。它们不会在会话开始时预加载。

## 关键行为：描述 vs 完整内容

技能描述会被加载到上下文中以便 Claude 知道有什���可用，但**完整技能内容仅在调用时加载**。这是一个重要的优化：

- **描述：** 始终在上下文中（在字符预算内）
- **完整内容：** 在技能被调用时按需加载

> 注意：带有预加载技能的子代理工作方式不同 — 完整技能内容在启动时注入。

## 优先级顺序（当技能共享名称时）

当技能在多个级别共享相同名称时，较高优先级的位置获胜：

| 优先级 | 位置 | 范围 |
|----------|----------|-------|
| 1（最高）| 企业 | 组织范围 |
| 2 | 个人（`~/.claude/skills/`）| 你的所有项目 |
| 3（最低）| 项目（`.claude/skills/`）| 仅此项目 |

插件技能使用 `plugin-name:skill-name` 命名空间，因此与其他级别不会冲突。

## 为什么这种设计适合 Monorepo

- **包特定技能保持隔离** — 在 `packages/frontend/` 中工作的前端开发人员获得前端特定技能，而不会让后端技能混乱上下文。

- **自动发现减少配置** — 无需显式注册包级技能；当你这些目录中工作时它们会被发现。

- **上下文已优化** — 最初只加载技能描述，嵌套技能按需发现。

- **团队可以维护自己的技能** — 每个包团队可以定义特定于其领域的技能，无需与其他团队协调。

## 字符预算注意事项

技能描述被加载到上下文中，直到字符预算（默认 15,000 字符）。在具有许多包和技能的大型 monorepo 中，你可能会达到此限制。

- 运行 `/context` 检查关于排除技能的警告
- 设置 `SLASH_COMMAND_TOOL_CHAR_BUDGET` 环境变量来增加限制

## 最佳实践

1. **将共享工作流放在根 `.claude/skills/`** — 仓库范围的约定、提交工作流和共享模式。

2. **将包特定技能放在包的 `.claude/skills/`** — 框架特定模式、组件约定、该包独有的测试工具。

3. **对危险技能使用 `disable-model-invocation: true`** — 部署或破坏性技能需要显式用户调用。

4. **保持技能描述简洁** — 描述始终在上下文中（直到字符预算），因此冗长的描述会浪费上下文空间。

5. **在技能名称中使用命名空间** — 考虑添加包名称前缀（例如 `frontend-review`、`backend-deploy`）以避免混淆。

## 对比：技能 vs CLAUDE.md 加载

| 行为 | CLAUDE.md | 技能 |
|----------|-----------|--------|
| 祖先加载（向上遍历目录树）| 是 | 否 |
| 嵌套/后代发现（向下遍历目录树）| 是（延迟）| 是（自动发现）|
| 全局位置 | `~/.claude/CLAUDE.md` | `~/.claude/skills/` |
| 项目位置 | `.claude/` 或仓库根目录 | `.claude/skills/` |
| 内容加载 | 完整内容 | 仅描述（调用时加载完整）|

---

## 来源

- [Claude Code 文档 - 使用技能扩展 Claude](https://code.claude.com/docs/en/skills)
- [Claude Code 文档 - 从嵌套目录自动发现](https://code.claude.com/docs/en/skills#automatic-discovery-from-nested-directories)