# 压缩合并和 PR 大小分布 — 来自 Boris Cherny 的技巧

Boris Cherny（[@bcherny](https://x.com/bcherny)），Claude Code 的创造者，于 2026 年 3 月 25 日分享的见解总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 1/ 266 次贡献在一天中 — 始终压缩合并

Boris 分享了他的 GitHub 贡献图，显示 **3 月 24 日 266 次贡献** — 来自 **141 个 PR，始终压缩合并**，每个 PR 的**中位数 118 行**。

- 压缩合并将所有分支提交合并到目标分支上的单个提交 — 保持历史简洁线性
- 每个 PR = 一次提交使撤销整个功能变得容易，并简化了 `git bisect`
- 在高velocity AI 辅助工作流（141 PR/天）中，压缩合并是务实的选择 — 分支内的个别"修复 lint"、"尝试这个"提交是噪音

<a href="https://x.com/bcherny/status/2038552880018538749"><img src="assets/boris-25-mar-26/1.png" alt="Boris Cherny — 266 contributions, always squashed" width="50%" /></a>

---

## 2/ PR 大小分布 — 保持 PR 小

Boris 分享了这 141 个 PR 的大小分布，总共 **45,032 行更改**（添加 + 删除）：

| 指标 | 行数 (添加+��除) | 含义 |
|--------|---------------:|---------|
| **p50** | **118** | 中位数 PR 大小 — 一半的 PR 为 118 行或更少 |
| p90 | 498 | 90% 的 PR 低于 500 行 |
| **p99** | **2,978** | 只有一个 PR 超过约 3K 行 |
| min | 2 | 最小的 PR — 快速的 2 行修复 |
| max | 10,459 | 最大的单个 PR — 可能是迁移或生成的代码 |

- **中位数 118 行**意味着大多数 PR 是集中的和可审查的，即使在 141 PR/天
- 分布严重右偏 — 偶尔的大型 PR 是不可避免的（大面积重命名、迁移），但规范是紧凑的
- 小 PR 减少合并冲突风险，更容易审查，与压缩合并完美配对以进行干净的撤销

<a href="https://x.com/bcherny/status/2038552880018538749"><img src="assets/boris-25-mar-26/2.png" alt="Boris Cherny — PR size distribution table" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) on X — 2026 年 3 月 25 日](https://x.com/bcherny/status/2038552880018538749)