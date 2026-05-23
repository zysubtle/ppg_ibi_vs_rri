# OAR-M PPG-IBI M1 Project Package

本包用于将已确认的 Project Brief v0.2 与 Algorithm Decision Cards 工程化为仓库文档和 Codex 可执行任务边界。

当前阶段：M1：项目包与 Codex 任务文件。

本包只包含文档、接口契约、评估协议、里程碑计划、风险登记和 Codex 任务说明；不包含算法实现代码，不包含原始生理数据，不包含第三方源码。

## 建议放置方式

将本包中的 `docs/` 目录内容复制到目标仓库的 `docs/` 目录。

建议目标仓库至少保留以下协议文件：

- `docs/00_OAR_M_PROTOCOL.md`
- `docs/01_DEEP_RESEARCH_PROTOCOL.md`
- `docs/02_CODEX_GITHUB_RULES.md`

如仓库中已有这些文件，以仓库版本为准；如没有，应先补齐协议文件，再执行 `docs/10_CODEX_NEXT_TASK.md`。

## 当前 M1 边界

M1 允许：

- 建立或更新项目文档；
- 固化 Project Brief v0.2；
- 固化 IO Contract 初版；
- 固化 Data Contract 初版；
- 固化 Evaluation Protocol 初版；
- 固化 Algorithm Decision Cards；
- 固化 Milestone Plan；
- 固化 Risk Register；
- 固化 Codex PR 流程和审查清单；
- 写入 `docs/10_CODEX_NEXT_TASK.md`。

M1 禁止：

- 实现 PPG-IBI 算法；
- 引入第三方生产依赖；
- 复制或改写第三方源码；
- 提交原始完整数据集或未经确认的生理数据；
- 修改已确认的项目目标、核心接口、评估协议或验收标准；
- 声称 fixture 测试代表真实评估结果。

## 当前关键决策

- 生产 baseline：Elgendi/Shin 风格低复杂度自适应阈值峰检测。
- 推荐主路线：qppgfast-inspired slope-sum + peak refine + SQI + interval sanitizer + calibrated confidence。
- 第三方库：默认仅用于离线 benchmark / 对拍 / 协议验证；生产 C99 MCU 库默认零第三方生产依赖。
- 当前上传 CSV：仅作为 fixture / smoke test；默认不提交到 GitHub，除非 Owner 后续明确确认。
