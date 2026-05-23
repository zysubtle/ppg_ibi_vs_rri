# Risk Register v0.1

| ID | 风险 | 严重性 | 当前状态 | 缓解措施 |
|---|---|---:|---|---|
| R1 | 当前 fixture 过短，只能 smoke test | 高 | 已识别 | 不用 fixture 声称性能；M2 后要求更多数据 |
| R2 | 验收门槛未冻结 | 高 | 未关闭 | M2/M3 用 baseline 结果提出建议，Owner 确认 |
| R3 | ECG RRI 与 PPG-IBI 存在生理延迟与 PAT 波动 | 高 | 已识别 | beat-level 与 interval-level 分开评估 |
| R4 | 无 ACC/IMU 和佩戴检测，异常识别受限 | 高 | 已识别 | 依赖 `allow_measure` 与 PPG 自身 SQI；不支持运动/低灌注/松佩戴 |
| R5 | 15 KB RAM / 30 KB Flash 预算紧 | 高 | 已识别 | 静态内存、低阶 IIR、无生产依赖、避免 FFT / ML / DTW |
| R6 | FPU / SDK / 编译器细节未确认 | 中 | 未关闭 | MCU 端 fixed/integer-friendly 优先，float 可选 |
| R7 | 第三方库许可证风险 | 高 | 已识别 | 生产零依赖；第三方仅离线；复制源码需 Owner 单独确认 |
| R8 | `confidence` 目标误差带未定 | 中 | 未关闭 | M2/M3 做 threshold sweep 后由 Owner 确认 |
| R9 | `invalid_reason` 若不进入日志会影响调试 | 中 | 已缓解 | 作为 internal/debug 必须字段 |
| R10 | MSPTDfast/pyPPG 等研究路线被误用为生产依赖 | 中 | 已缓解 | 明确只作为离线 benchmark/reference |
| R11 | 过早编码导致接口和评估返工 | 高 | 已缓解 | M1 先冻结文档和协议，不实现算法 |
| R12 | Codex 越权修改目标/接口/验收标准 | 高 | 已缓解 | 使用 `docs/10_CODEX_NEXT_TASK.md` 和 PR 审查清单约束 |
| R13 | 提交人体生理原始数据到 GitHub | 高 | 未关闭 | 默认不提交；需要 Owner 明确确认脱敏与用途 |
| R14 | fixture 质量字段与 `allow_measure` 混淆 | 中 | 已识别 | Data Contract 明确二者不同 |
