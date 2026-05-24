# Evaluation Protocol v0.1

## 1. 总原则

PPG 主峰时间戳与 ECG R 峰时间戳之间存在
pulse transit / pulse arrival delay。

不能简单做绝对时间硬匹配后直接评价 IBI。

首版评估必须拆成两层：

1. beat-level detection evaluation；
2. interval-level IBI accuracy evaluation。

## 2. 参考真值

参考真值来自 ECG R peak / RRI。

当前 fixture 字段：

- `r_peak_sample_index`；
- `r_peak_time_s`；
- `rr_ms`。

后续正式评估建议使用两个 ECG detector 或人工审核结果形成参考，但 M1 不强制实现。

## 3. Beat-level evaluation

### 3.1 目标

评价 PPG beat detection 是否找到了与 ECG reference beats 对应的脉搏事件。

### 3.2 Lag compensation

因 PPG beat 相对 ECG R peak 有生理延迟，beat-level 匹配前应估计 PPG 与 ECG 的整体 lag。

M2 初版可采用：

- 在合理范围内搜索整体 lag；
- 使 matched beat 数最大或 median timing error 最小；
- fixture 太短时可只实现接口和占位统计，不输出正式结论。

### 3.3 匹配窗口

初始建议：

```text
±150 ms
```

PPG beat 经 lag compensation 后，落在 ECG reference beat ±150 ms 内，计为 TP。

### 3.4 指标

| 指标 | 定义 |
|---|---|
| TP | 匹配成功的 PPG beat |
| FP | 未匹配到 ECG reference 的 PPG beat |
| FN | 未被 PPG beat 匹配的 ECG reference beat |
| Sensitivity | TP / (TP + FN) |
| PPV / Precision | TP / (TP + FP) |
| F1 | 2 * PPV * Sensitivity / (PPV + Sensitivity) |
| beat timing error | matched PPG beat timestamp - reference timestamp after lag compensation |

M1 已确认用户核心指标包含 sensitivity。

协议建议同时加入 PPV / F1，以防只靠多报 peak 提升 sensitivity。

## 4. Interval-level evaluation

### 4.1 目标

评价 PPG-derived `IBI_ms` 与 ECG-derived `RRI_ms` 的一致性。

### 4.2 配对规则

只在满足以下条件时形成一对 interval：

1. 当前 PPG beat matched 到 ECG reference beat；
2. 上一个 PPG beat 也 matched 到相邻 ECG reference beat；
3. 两个 PPG beats 均 `valid_flag=1`；
4. 中间没有 invalid 断点；
5. 对应 ECG RRI 有效。

### 4.3 指标

| 指标 | 定义 |
|---|---|
| `IBI_MAE` | mean(abs(PPG_IBI_ms - ECG_RRI_ms)) |
| `IBI_RMSE` | sqrt(mean(error^2)) |
| `bias` | mean(PPG_IBI_ms - ECG_RRI_ms) |
| `median_AE` | median(abs(error)) |
| `hit_rate_300ms` | abs(error) <= 300 ms 的比例 |
| `coverage` | 有效 IBI 数 / eligible reference intervals 数 |
| `invalid_ratio` | invalid 输出或 invalid 片段占 eligible intervals 的比例 |

建议后续同时输出：

- `hit_rate_20ms`；
- `hit_rate_30ms`；
- `hit_rate_50ms`；
- Bland–Altman / LoA；
- per-file / per-subject / per-scenario 统计。

## 5. Coverage 与 invalid ratio

建议初版定义：

```text
eligible_reference_interval = allow_measure=1 且 ECG quality OK 的参考 RR interval
coverage = valid_ppg_ibi_count / eligible_reference_interval_count
invalid_ratio = invalid_interval_count / eligible_reference_interval_count
```

若 fixture 无 `allow_measure`，默认全为 eligible，但测试报告必须说明 fallback。

## 6. Confidence threshold sweep

`confidence` 初版范围 0～100。

M2/M3 后应做 threshold sweep：

| threshold | availability / coverage | MAE | hit_rate_300ms | sensitivity | PPV | invalid_ratio |
|---:|---:|---:|---:|---:|---:|---:|

用途：

- 找到发布阈值；
- 量化 confidence 与误差之间的关系；
- 不把单一阈值的结果当作全部性能。

## 7. Fixture / smoke test 报告要求

fixture 报告必须显式写明：

```text
This is a fixture / smoke test only. It is not a formal performance evaluation.
```

不得输出类似“算法已达标”的结论。

## 8. 当前未冻结的验收门槛

以下门槛尚未确认：

- Sensitivity 下限；
- PPV / F1 下限；
- IBI MAE 目标；
- coverage 下限；
- invalid ratio 上限；
- confidence 发布阈值。

M2/M3 可基于更多数据和 baseline 结果提出建议，但必须由 Owner 最终确认。
