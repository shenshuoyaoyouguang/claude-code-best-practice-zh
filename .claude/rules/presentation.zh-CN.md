# Glob: presentation/**

## 委托规则

任何更新、修改或修复演示文稿（presentation/index.html）的请求都必须由 presentation-curator 代理处理。始终通过 Task 工具将演示工作委托给此代理 — 不要直接编辑演示文稿。

`
Task(subagent_type="presentation-curator", description="...", prompt="...")
`

## 原因

presentation-curator 代理具有三个预加载的技能，使其与演示文稿的结构、样式和概念框架保持同步。它还会在每次执行后自我进化，更新自己的技能以防止知识偏差。绕过此代理可能会破坏幻灯片编号、级别转换或样式一致性。
