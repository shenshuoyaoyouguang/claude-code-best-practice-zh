# Glob: **/*.md

## 文档标准

- 保持文件聚焦简洁 — 每个文件一个主题
- 使用相对链接（如 ../best-practice/claude-memory.md），而非 GitHub 绝对链接
- 在 best-practice 和 report 文档顶部包含返回导航链接（参见现有文件的模式）
- 添加新概念或报告时，更新 README.md 中对应的表格（CONCEPTS 或 REPORTS）

## 结构规范

- 最佳实践文档放在 est-practice/
- 实现文档放在 implementation/
- 报告放在 eports/
- 技巧放在 	ips/
- 变更日志放在 changelog/<category>/

## 格式

- 使用表格进行结构化对比（参见 README CONCEPTS 表格作为参考）
- 使用 !/tags/ 中的徽章图片以保持视觉一致性来链接 best-practice 或实现文档
- 保持标题层级 — 不要跳过级别（例如不要从 ## 跳到 ####）
