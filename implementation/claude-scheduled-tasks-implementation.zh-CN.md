# 定时任务实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_10%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#loop-demo"><img src="../!/tags/implemented-hd.svg" alt="已实现"></a>

/loop 技能用于在 cron 间隔上调度重复任务。以下是 /loop 1m "tell current time" 的演示 — 一个每分钟触发一次的简单重复任务。

---

## Loop 演示

### 1. 调度任务

<p align="center">
  <img src="assets/impl-loop-1.png" alt="/loop 1m tell current time — scheduling and cron setup" width="100%">
</p>

/loop 1m "tell current time" 解析间隔（1m → 每分钟），创建 cron 作业，并确认调度。关键说明：

- Cron 的最小粒度是 1 分钟 — 1m 映射到 */1 * * * *
- 重复任务 3 天后自动过期
- 作业是会话作用域 — 仅存在于内存中，Claude 退出时停止
- 随时可以使用 cron cancel <job-id> 取消

---

### 2. Loop 运行中

<p align="center">
  <img src="assets/impl-loop-2.png" alt="Recurring task firing every minute" width="100%">
</p>

任务每分钟触发，运行 date 并报告当前时间。每次迭代触发异步 UserPromptSubmit 和 Stop 钩子 — 与本仓库中用于声音通知的相同钩子系统。

---

## 使用方法

```bash
$ claude
> /loop 1m "tell current time"
> /loop 5m /simplify
> /loop 10m "check deploy status"
```

---

## 实现方法

/loop 是 Claude Code 内置技能 — 无需设置。它在底层使用 cron 工具（CronCreate、CronList、CronDelete）来管理重复调度。