# Algorithm Decision Cards v0.2

状态：Owner 已接受 D1–D13 默认建议。

## D1. Production Baseline

| 项目 | 内容 |
|---|---|
| 决策主题 | 生产 baseline |
| 推荐方案 | Elgendi/Shin 风格低复杂度自适应阈值 PPG peak detector |
| 证据强度 | Moderate-Strong |
| 风险 | 低灌注、轻度形变、复杂噪声下稳健性有限 |
| 工程决策 | Accept |

## D2. Recommended Main Route

| 项目 | 内容 |
|---|---|
| 决策主题 | 推荐主算法路线 |
| 推荐方案 | qppgfast-inspired slope-sum + peak refine + SQI + interval sanitizer + calibrated confidence |
| 证据强度 | Moderate |
| 风险 | 自研 C99/MISRA 迁移后需真实数据验证 |
| 工程决策 | Accept |

## D3. Offline Ceiling / Reference

| 项目 | 内容 |
|---|---|
| 决策主题 | 离线 ceiling / reference |
| 推荐方案 | MSPTDfast / NeuroKit2 / pyPPG 仅用于离线 benchmark / 对拍 / 协议验证 |
| 证据强度 | Strong for benchmark；Weak for production |
| 风险 | 许可证、生态、复杂度不适合首版生产依赖 |
| 工程决策 | Accept |

## D4. Beat Timestamp

| 项目 | 内容 |
|---|---|
| 决策主题 | `beat_timestamp_ms` 语义 |
| 推荐方案 | PPG systolic apex 时间戳 |
| 证据强度 | Moderate |
| 风险 | 与 ECG-RRI 最优 surrogate fiducial 可能不同 |
| 工程决策 | Accept |

## D5. IBI Definition

| 项目 | 内容 |
|---|---|
| 决策主题 | `IBI_ms` 定义 |
| 推荐方案 | current valid apex - previous valid apex |
| 证据强度 | Strong as project definition |
| 风险 | invalid beat 断点必须严格处理 |
| 工程决策 | Accept |

## D6. Confidence

| 项目 | 内容 |
|---|---|
| 决策主题 | `confidence` 语义 |
| 推荐方案 | 0～100 的 calibrated reliability；表示当前 IBI 落入目标误差带的经验可靠度 |
| 证据强度 | Moderate / Assumption |
| 风险 | 目标误差带与发布阈值未冻结 |
| 工程决策 | Accept with later calibration |

## D7. Valid / Invalid Contract

| 项目 | 内容 |
|---|---|
| 决策主题 | `valid_flag / invalid_reason` |
| 推荐方案 | `valid_flag` 公开输出；`invalid_reason` 至少作为 internal/debug 字段 |
| 证据强度 | Moderate |
| 风险 | 若不记录 invalid_reason，后续错误分析困难 |
| 工程决策 | Accept |

## D8. Evaluation Protocol

| 项目 | 内容 |
|---|---|
| 决策主题 | ECG RRI vs PPG-IBI 评估协议 |
| 推荐方案 | beat-level 与 interval-level 分开；±150 ms beat matching；连续 matched valid beats 计算 IBI error |
| 证据强度 | Strong |
| 风险 | 需要足够数据与可靠 ECG reference |
| 工程决策 | Accept |

## D9. Sampling Rate

| 项目 | 内容 |
|---|---|
| 决策主题 | 采样率范围 |
| 推荐方案 | MVP 固定 50 Hz；未来更高采样率或双速率路径作为扩展 |
| 证据强度 | Strong as project constraint |
| 风险 | 多硬件兼容时需扩展 |
| 工程决策 | Accept |

## D10. Numeric Strategy

| 项目 | 内容 |
|---|---|
| 决策主题 | 数值策略 |
| 推荐方案 | PC float；MCU fixed/integer-friendly 优先，float 可选 |
| 证据强度 | Moderate |
| 风险 | 与实际 SDK / ABI / FPU 配置有关 |
| 工程决策 | Accept |

## D11. Dependency Policy

| 项目 | 内容 |
|---|---|
| 决策主题 | 第三方依赖策略 |
| 推荐方案 | 生产零第三方依赖；第三方仅离线 benchmark / 对拍 / 协议验证 |
| 证据强度 | Strong for risk control |
| 风险 | 自研工作量增加 |
| 工程决策 | Accept |

## D12. Fixture Use

| 项目 | 内容 |
|---|---|
| 决策主题 | 当前上传 CSV 使用边界 |
| 推荐方案 | 仅作为 fixture / smoke test，不做正式性能结论 |
| 证据强度 | Strong |
| 风险 | 无法验证真实性能达标 |
| 工程决策 | Accept |

## D13. M1 Scope

| 项目 | 内容 |
|---|---|
| 决策主题 | M1 范围 |
| 推荐方案 | 先生成项目包、接口、评估协议、里程碑计划和 Codex 任务边界，不直接写完整算法 |
| 证据强度 | Strong by protocol |
| 风险 | 进展看似慢，但降低返工 |
| 工程决策 | Accept |
