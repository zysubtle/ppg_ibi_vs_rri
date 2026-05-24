# Milestone Plan v0.1

## M1：项目包、文档与任务边界

目标：

- 建立仓库文档；
- 建立接口契约、数据契约和评估协议；
- 建立风险登记、PR 审查清单和 `docs/10_CODEX_NEXT_TASK.md`。

允许：文档、目录规划、任务边界。

禁止：算法实现、第三方生产依赖、提交真实原始数据。

验收：

- `docs/03_PROJECT_BRIEF_V0_2.md` 存在；
- `docs/04_IO_CONTRACT.md` 存在；
- `docs/05_DATA_CONTRACT.md` 存在；
- `docs/06_EVALUATION_PROTOCOL.md` 存在；
- `docs/07_ALGORITHM_DECISION_CARDS.md` 存在；
- `docs/08_MILESTONE_PLAN.md` 存在；
- `docs/09_RISK_REGISTER.md` 存在；
- `docs/10_CODEX_NEXT_TASK.md` 存在；
- `docs/11_PR_REVIEW_CHECKLIST.md` 存在。

## M2：PC 离线解析与评估框架

目标：建立 PC 离线数据解析、输出格式、fixture smoke test 和评估指标框架。

允许：解析 CSV、生成中间输出、评估协议代码化、smoke test。

禁止：把 fixture 结果描述为正式性能。

验收：

- 能读取 `time_s_50hz`、`ppg_50hz`、`ecg_50hz`、`r_peak_time_s`、`rr_ms`；
- 能将秒转换为 ms；
- 能处理缺失 `allow_measure` 的 fallback；
- 能输出空算法或 mock 算法的评估表结构；
- fixture smoke test 可运行。

## M3：Baseline PPG Peak / IBI 算法

目标：实现 Elgendi/Shin 风格 baseline。

允许：低复杂度因果滤波、自适应阈值、局部极大值、refractory、基础 IBI 计算。

禁止：复杂主算法、第三方源码复制、调参过拟合 fixture。

验收：

- baseline 可在 PC 离线工具运行；
- 输出 `IBI_ms`、`beat_timestamp_ms`、`confidence`、`valid_flag`；
- 评估框架能生成 sensitivity、PPV、F1、IBI MAE、hit_rate_300ms、coverage、invalid ratio；
- 明确 fixture 只是 smoke test。

## M4：SQI / Invalid / Confidence 初版

目标：加入质量控制、invalid_reason、confidence 初版。

允许：flatline、clipping、low amplitude、timestamp anomaly、shape fail、interval fail、warmup 等规则。

禁止：

- 把 confidence 当作已校准医学可靠性结论；
- 修改已确认的公开输出字段语义；
- 跳过 invalid_reason 记录。

验收：

- invalid_reason 枚举完整；
- confidence 0～100；
- 可以做 threshold sweep；
- 输出错误案例摘要。

## M5：主算法与状态机

目标：实现 qppgfast-inspired slope-sum + peak refine + interval sanitizer + 状态机 + 回填。

允许：positive derivative / slope-sum、自适应阈值、peak refine、state machine、backfill。

禁止：FFT 主路径、深度学习、MSPTDfast 直接生产移植。

验收：

- 状态机可跟踪 INIT / ACQUIRE / TRACK / INVALID / REACQUIRE；
- 回填延迟 ≤2 s；
- 与 baseline 进行对比；
- 输出风险与参数表。

## M6：MCU C99 移植

目标：将确认的主算法迁移到 C99 MCU 库。

允许：静态内存、固定大小 ring buffer、fixed/integer-friendly 实现、必要的编译开关。

禁止：malloc/free、递归、可变长数组、第三方生产依赖。

验收：

- RAM 估算 / 实测 ≤15 KB；
- Flash 估算 / 实测 ≤30 KB；
- C99 编译通过；
- PC / MCU golden vector 一致性检查通过。

## M7：回归测试与参数冻结

目标：建立回归测试矩阵、参数冻结、benchmark 对拍。

允许：回归测试矩阵、参数变更记录、baseline 与主算法对比、第三方离线 benchmark 对拍。

禁止：

- 引入第三方生产依赖；
- 把离线 benchmark 工具作为 MCU 生产实现；
- 未由 Owner 确认就冻结验收门槛。

验收：

- 每个数据集版本有结果记录；
- 参数变更有日志；
- 回归测试可以比较 baseline 与主算法；
- 第三方 benchmark 结果只作为参考。

## M8：验收报告与风险复盘

目标：整理最终验收报告、风险复盘、文档收敛和后续版本计划。

允许：汇总验收结果、关闭或转移风险、更新交付文档、提出后续版本计划。

禁止：伪造或夸大达标结果；隐藏未达标项；merge PR 或发布版本绕过 Owner 确认。

验收：

- 验收指标结果明确；
- 未达标项明确；
- 风险登记关闭或转入后续；
- 文档与代码版本一致。
