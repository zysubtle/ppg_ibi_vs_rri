# PR Review Checklist v0.1

用于 Architect / Reviewer 审查 Codex PR。

## 1. 范围检查

- [ ] PR 只修改当前 milestone 授权范围内文件。
- [ ] 未擅自修改项目目标。
- [ ] 未擅自修改核心 IO Contract。
- [ ] 未擅自修改 Evaluation Protocol。
- [ ] 未擅自修改验收标准。
- [ ] 未擅自扩大 milestone 范围。

## 2. 分支与 GitHub 检查

- [ ] 未在 `main` / `master` 上编码。
- [ ] 使用独立工作分支。
- [ ] 未直接 push 到 `main` / `master`。
- [ ] 未 merge PR。
- [ ] 未 force push。
- [ ] PR 描述包含 Summary、Changed Files、Tests、Known Risks。

## 3. 依赖与许可证检查

- [ ] 未引入新的第三方生产依赖。
- [ ] 未复制或改写第三方源码。
- [ ] 如使用第三方工具做离线 benchmark，已说明用途和许可证。
- [ ] 未引入许可证不明代码。

## 4. 数据检查

- [ ] 未提交原始完整数据集。
- [ ] 未提交隐私数据、受限数据或未经确认的人体生理数据。
- [ ] 如提交 fixture，已确认脱敏、足够小、用途明确。
- [ ] 未把 fixture 结果夸大为正式性能。

## 5. 测试检查

- [ ] 实际运行了与任务相关的测试。
- [ ] 未伪造测试结果。
- [ ] 如测试未运行，说明了原因。
- [ ] smoke test 与正式评估结论区分清楚。

## 6. 算法逻辑检查

M1：

- [ ] 不应包含算法实现。
- [ ] 只应包含文档、接口、协议和任务边界。

M2 及以后：

- [ ] 算法修改与当前 milestone 匹配。
- [ ] 参数变更有说明。
- [ ] 评估结果没有夸大。
- [ ] 错误案例与风险有记录。

## 7. 建议 Reviewer 重点关注

- IO Contract 是否稳定；
- Evaluation Protocol 是否严谨；
- `allow_measure`、`quality_flag`、`valid_flag` 是否混淆；
- `confidence` 是否被误用为 hard valid；
- 第三方库是否越界进入生产；
- 数据是否合规；
- 是否误把 Deep Research 结论当作最终已验证结果。
