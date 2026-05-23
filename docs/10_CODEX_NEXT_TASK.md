# docs/10_CODEX_NEXT_TASK.md

# M1：项目文档包、接口契约与评估协议落库

## 0. 任务背景

本项目按 OAR-M 机制协作。

Owner 已接受：

- Project Brief v0.2；
- Algorithm Decision Cards D1–D13 默认建议。

当前进入 M1：项目包与 Codex 任务文件。

M1 的目标是把已确认的项目目标、接口边界、数据契约、评估协议、里程碑计划和 PR 审查规则落入仓库文档。

M1 不实现 PPG-IBI 算法，不写 C 算法代码，不写 Python 评估代码。

## 1. 你必须先阅读并遵守

如果仓库存在以下文件，必须先阅读：

- `docs/00_OAR_M_PROTOCOL.md`
- `docs/01_DEEP_RESEARCH_PROTOCOL.md`
- `docs/02_CODEX_GITHUB_RULES.md`

如果缺少上述文件，报告缺失，不要自行编造协议内容。

## 2. 分支要求

不得在 `main` / `master` 上编码。

请创建独立工作分支：

```text
docs/m1-project-docs
```

如果仓库已有同名分支，可使用：

```text
docs/m1-project-docs-v2
```

Commit message 建议：

```text
M1: add project brief and evaluation docs
```

## 3. 本任务允许修改 / 新增的文件

允许新增或更新：

```text
README.md

docs/03_PROJECT_BRIEF_V0_2.md
docs/04_IO_CONTRACT.md
docs/05_DATA_CONTRACT.md
docs/06_EVALUATION_PROTOCOL.md
docs/07_ALGORITHM_DECISION_CARDS.md
docs/08_MILESTONE_PLAN.md
docs/09_RISK_REGISTER.md
docs/10_CODEX_NEXT_TASK.md
docs/11_PR_REVIEW_CHECKLIST.md
```

如果仓库已有 README.md，只能做最小必要补充，不得大改无关内容。

如果仓库已有同名文档，优先保留已有重要内容，并将本任务内容合并为 v0.2 / v0.1 版本。

## 4. 本任务明确禁止

禁止：

1. 实现 PPG-IBI 算法；
2. 新增 C 源码算法实现；
3. 新增 Python 解析或评估代码；
4. 引入第三方生产依赖；
5. 复制或改写第三方源码；
6. 提交当前上传的真实 CSV 或任何原始生理数据；
7. 修改已确认的 Project Brief v0.2 决策；
8. 修改已确认的 D1–D13 Decision Cards 默认建议；
9. 修改验收指标定义；
10. 声称算法已实现或测试已达标；
11. 伪造测试、benchmark 或 PR 链接；
12. merge PR。

## 5. 文档内容要求

### 5.1 Project Brief

`docs/03_PROJECT_BRIEF_V0_2.md` 必须包含：

- 项目目标；
- IBI 定义；
- 输入条件；
- 输出字段；
- 输出节奏与延迟；
- 支持/不支持场景；
- MCU 与资源约束；
- 推荐 baseline；
- 推荐主算法路线；
- 第三方依赖策略；
- fixture 使用边界；
- 交付路径。

### 5.2 IO Contract

`docs/04_IO_CONTRACT.md` 必须包含：

- 输入字段；
- 输出字段；
- `beat_timestamp_ms` 语义；
- `IBI_ms` 语义；
- `confidence` 0～100 初版范围；
- `valid_flag` 语义；
- internal/debug `invalid_reason` 枚举；
- 状态机建议；
- 回填策略边界。

### 5.3 Data Contract

`docs/05_DATA_CONTRACT.md` 必须包含：

- CSV 字段说明；
- `time_s_50hz` 转 ms；
- 缺失 `allow_measure` 时 fallback 全允许；
- `quality_flag` 不等同于 `allow_measure`；
- fixture 只做 smoke test；
- 默认不提交真实生理数据。

### 5.4 Evaluation Protocol

`docs/06_EVALUATION_PROTOCOL.md` 必须包含：

- beat-level 与 interval-level 分开评估；
- lag compensation；
- ±150 ms beat matching；
- sensitivity、PPV、F1；
- IBI MAE、RMSE、bias、median AE；
- ±300 ms 命中率；
- coverage；
- invalid ratio；
- confidence threshold sweep；
- fixture 不代表正式性能。

### 5.5 Decision Cards

`docs/07_ALGORITHM_DECISION_CARDS.md` 必须包含 D1–D13。

### 5.6 Milestone Plan

`docs/08_MILESTONE_PLAN.md` 必须包含 M1–M8，每个 milestone 包含目标、允许事项、禁止事项和验收点。

### 5.7 Risk Register

`docs/09_RISK_REGISTER.md` 必须列出至少：

- fixture 数据不足；
- 评估门槛未冻结；
- ECG/PPG 生理延迟；
- 无 ACC/佩戴检测；
- 资源预算；
- FPU/SDK 未确认；
- 第三方许可证；
- confidence 语义；
- 数据提交风险；
- Codex 越权风险。

### 5.8 PR Review Checklist

`docs/11_PR_REVIEW_CHECKLIST.md` 必须覆盖：

- 范围检查；
- 分支检查；
- 依赖/许可证检查；
- 数据检查；
- 测试检查；
- 算法逻辑检查；
- Reviewer 重点关注项。

## 6. README.md 更新要求

若需要更新 README.md，只添加简短项目概览和当前阶段说明。

不得把 README.md 写成完整设计文档。

建议 README.md 至少链接：

- `docs/03_PROJECT_BRIEF_V0_2.md`
- `docs/04_IO_CONTRACT.md`
- `docs/06_EVALUATION_PROTOCOL.md`
- `docs/10_CODEX_NEXT_TASK.md`

## 7. 测试要求

M1 以文档为主。

如果仓库有 markdown lint，可运行：

```text
markdownlint docs README.md
```

如果没有 markdownlint，应至少运行：

```text
find docs -maxdepth 1 -type f -name "*.md" -print
```

并报告实际执行的命令。

不得声称测试通过，除非实际执行。

## 8. PR 描述必须包含

```markdown
## Summary

## Changed Files

## Algorithm Logic Change
- No
- Explanation: M1 only adds/updates documentation and task boundaries.

## Interface / IO Contract Change
- Yes
- Explanation: Adds initial IO Contract v0.1 based on accepted Project Brief v0.2.

## Dependency Change
- No
- Explanation: No production dependencies added.

## Data Format Change
- Yes
- Explanation: Adds Data Contract v0.1, no data files committed.

## Test Protocol Change
- Yes
- Explanation: Adds Evaluation Protocol v0.1.

## Test Commands and Results

## Tests Not Run and Reasons

## Known Risks

## Suggested Reviewer Focus

## Out-of-scope Items
```

## 9. 完成标准

M1 PR 只有在以下条件全部满足时才算完成：

- 文档文件存在；
- 未实现算法；
- 未提交真实数据；
- 未引入生产依赖；
- PR 描述完整；
- 测试命令如实报告；
- 未 merge PR。

## 10. M1 完成后的下一步

M1 合并后，Architect / Reviewer 将审查 PR 并准备 M2 任务。

M2 预计主题：PC 离线数据解析与评估框架。
