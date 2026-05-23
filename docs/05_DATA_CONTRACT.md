# Data Contract v0.1

## 1. 目标

定义 PC 离线验证阶段的 CSV 输入字段、时间单位、fixture 使用边界和测试数据提交规则。

## 2. Fixture CSV 字段

当前已知 CSV 文件名：

```text
bidmc_01_Signals_4000_gui_result.csv
```

字段定义：

| 字段 | 定义 | 单位 / 类型 | 处理要求 |
|---|---|---|---|
| `time_s_50hz` | 时间戳 | 秒 | 测试阶段换算为 ms |
| `ecg_50hz` | ECG 单导联原始信号 | arbitrary | 仅用于参考 / 可视化 / R peak 对齐 |
| `ppg_50hz` | 单通道绿光 PPG 信号 | normalized | 算法输入 |
| `r_peak_sample_index` | R 波波峰在 ECG 序列中的位置 | sample index | 空值表示非 R peak 行 |
| `r_peak_time_s` | R 波波峰时间戳 | 秒 | 换算为 ms |
| `rr_ms` | ECG RR 间期 | ms | interval reference |
| `quality_flag` | 信号质量是否达标 | string | `OK/ok` 大小写归一，但不得等同于 `allow_measure` |

## 3. `allow_measure` 处理

工程输入 `allow_measure` 的正式语义：

```text
allow_measure = 1：允许测量
allow_measure = 0：不允许测量
```

若 fixture 中没有 `allow_measure` 字段：

```text
默认所有样本 allow_measure = 1
```

但必须在测试报告中说明：

```text
allow_measure was not present in fixture; fallback all-allowed mode used.
```

## 4. 时间戳处理

- `time_s_50hz` 转换为 `timestamp_ms = time_s_50hz * 1000`。
- 检查时间戳单调性。
- 50 Hz 下相邻采样点理论间隔约 20 ms。
- 超出容差应记录为 timestamp anomaly。

建议初始容差：

| 检查项 | 初始建议 |
|---|---:|
| 单步采样间隔 | 20 ms |
| 时间戳 jitter 容差 | ±2 ms |
| 明显跳变阈值 | >40 ms 或 <1 ms |

实际阈值需在 M2 解析框架中用数据验证。

## 5. Fixture 使用边界

当前上传 CSV 仅用于：

- 字段解析 smoke test；
- 时间轴解析 smoke test；
- R peak / RR 字段读取 smoke test；
- 输出格式 smoke test；
- 算法流程是否能跑通的最小样例。

不得用于：

- 正式性能结论；
- 算法达标声明；
- 大样本 MAE / F1 结论；
- 代表真实场景覆盖率；
- 代表量产可靠性。

## 6. 数据提交规则

默认不把当前真实/生理 CSV 提交到 GitHub。

如后续需要提交 fixture，必须满足：

1. Owner 明确确认可提交；
2. 数据已脱敏；
3. 文件足够小；
4. 用途仅限单元测试或 smoke test；
5. 文档中明确说明来源、用途和限制；
6. 不得把 fixture 结果夸大为真实评估结果。

若无法提交真实 fixture，应使用合成数据或最小模拟数据替代。
