# Risk Register v0.1

## R1. Fixture 数据不足

- 严重性：高
- 当前状态：已识别
- 缓解措施：不用 fixture 声称性能；M2 后要求更多数据。

## R2. 评估门槛未冻结

- 严重性：高
- 当前状态：未关闭
- 缓解措施：M2/M3 用 baseline 结果提出建议，由 Owner 确认。

## R3. ECG/PPG 生理延迟

- 严重性：高
- 当前状态：已识别
- 缓解措施：beat-level 与 interval-level 分开评估。

## R4. 无 ACC/佩戴检测

- 严重性：高
- 当前状态：已识别
- 缓解措施：
  依赖 `allow_measure` 与 PPG 自身 SQI；
  不支持运动、低灌注、松佩戴。

## R5. 资源预算

- 严重性：高
- 当前状态：已识别
- 缓解措施：
  静态内存、低阶 IIR、无生产依赖；
  避免 FFT、ML、DTW。

## R6. FPU / SDK 未确认

- 严重性：中
- 当前状态：未关闭
- 缓解措施：MCU 端 fixed/integer-friendly 优先，float 可选。

## R7. 第三方许可证

- 严重性：高
- 当前状态：已识别
- 缓解措施：
  生产零依赖；
  第三方仅离线；
  复制源码需 Owner 单独确认。

## R8. Confidence 语义

- 严重性：中
- 当前状态：未关闭
- 缓解措施：M2/M3 做 threshold sweep 后由 Owner 确认。

## R9. `invalid_reason` 日志缺失

- 严重性：中
- 当前状态：已缓解
- 缓解措施：作为 internal/debug 必须字段。

## R10. 第三方研究路线误用

- 严重性：中
- 当前状态：已缓解
- 缓解措施：
  MSPTDfast/pyPPG 等路线只作为离线 benchmark/reference。

## R11. 过早编码

- 严重性：高
- 当前状态：已缓解
- 缓解措施：M1 先冻结文档和协议，不实现算法。

## R12. Codex 越权风险

- 严重性：高
- 当前状态：已缓解
- 缓解措施：
  使用 `docs/10_CODEX_NEXT_TASK.md` 和 PR 审查清单约束。

## R13. 数据提交风险

- 严重性：高
- 当前状态：未关闭
- 缓解措施：
  默认不提交人体生理原始数据；
  需要 Owner 明确确认脱敏与用途。

## R14. `quality_flag` 与 `allow_measure` 混淆

- 严重性：中
- 当前状态：已识别
- 缓解措施：Data Contract 明确二者不同。
