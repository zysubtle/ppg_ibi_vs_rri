# IO Contract v0.1

状态：M1 初版。Owner 已确认核心输出字段和主要语义；部分枚举和门槛允许在 M2/M3 后细化。

## 1. 输入契约

### 1.1 PPG 样本输入

| 字段 | 类型建议 | 单位 | 范围 | 说明 |
|---|---|---|---|---|
| `timestamp_ms` | uint32 或 uint64 | ms | 单调递增 | 由 `time_s_50hz` 换算时应乘以 1000 |
| `ppg` | float32 in PC；MCU 可转定点 | normalized | 0～1 | 已扣环境光，未预处理 |
| `allow_measure` | uint8 / bool | - | 0/1 | 1=允许测量，0=不允许测量 |

工程要求：

- 采样率 MVP 固定为 50 Hz。
- 缺省测试 fixture 若无 `allow_measure` 字段，可按全 1 处理，但必须在日志中标记为 fixture fallback。
- 不得把 `quality_flag=OK/ok` 直接等同于工程输入 `allow_measure=1`。

### 1.2 初始化参数

| 参数 | 初始建议 | 说明 |
|---|---:|---|
| `fs_hz` | 50 | MVP 固定采样率 |
| `max_output_delay_ms` | 2000 | 逐搏输出最大延迟 |
| `min_ibi_ms` | 300 | 开发初值，后续数据验证 |
| `max_ibi_ms` | 2000 | 开发初值，后续数据验证 |
| `confidence_range` | 0～100 | 初版采用整数评分 |
| `allow_backfill` | true | 允许回填上一搏 |

## 2. 输出契约

### 2.1 公开输出字段

| 字段 | 类型建议 | 单位 | 有效范围 | 说明 |
|---|---|---|---|---|
| `IBI_ms` | int32 或 float32 | ms | `>0`，invalid 时可为 0 或 sentinel | 相邻有效 PPG 主峰间期 |
| `beat_timestamp_ms` | uint32 / uint64 | ms | 单调近似递增 | 当前有效 PPG 主峰时间戳 |
| `confidence` | uint8 | - | 0～100 | 当前 IBI 可靠度评分 |
| `valid_flag` | uint8 / bool | - | 0/1 | 1=当前 IBI 可用；0=invalid |

### 2.2 输出字段语义

`beat_timestamp_ms`：

- 表示当前有效 PPG systolic apex，即 PPG 主峰时间戳。
- 时间基准必须与输入 `timestamp_ms` 一致，单位为 ms。
- 它不是 ECG R peak 时间戳，也不要求与 ECG R peak 绝对硬对齐。

`IBI_ms`：

- 表示当前有效 PPG 主峰与上一有效 PPG 主峰之间的时间差。
- 只在当前 beat 与上一有效 beat 都满足 valid 条件时输出有效值。
- invalid beat、不支持场景、质量不足片段不得参与有效 IBI 链。

`confidence`：

- 初版范围为 0～100。
- 表示当前 IBI 的经验可靠度评分，不是正式性能达标声明。
- 发布阈值必须在后续 threshold sweep 后确认，M1 不冻结阈值。

`valid_flag`：

- `1` 表示当前 IBI 可供上游 IBI/HRV/PRV 计算使用。
- `0` 表示当前输出不可用，具体原因应由 internal/debug `invalid_reason` 记录。

### 2.3 内部/debug 建议输出

| 字段 | 建议类型 | 说明 |
|---|---|---|
| `invalid_reason` | enum | invalid 原因 |
| `sqi_beat` | uint8 | beat 级质量分 0～100 |
| `sqi_window` | uint8 | 窗口级质量分 0～100 |
| `state` | enum | 算法状态机 |
| `peak_amplitude` | float / fixed | 当前峰幅值或相对幅值 |

## 3. `valid_flag` 语义

`valid_flag=1` 必须同时满足：

1. `allow_measure=1`；
2. 非 warmup；
3. PPG 信号未触发硬 invalid；
4. 当前 beat 已确认；
5. 当前 beat 与上一有效 beat 可形成合法 IBI；
6. IBI 通过基础生理范围和 interval sanitizer；
7. confidence 不低于发布门槛。

`valid_flag=0` 表示当前输出不可用于上游 IBI/HRV/PRV 计算。

## 4. `invalid_reason` 建议枚举

| 枚举 | 含义 |
|---|---|
| `NONE` | valid，无 invalid 原因 |
| `NOT_ALLOWED` | `allow_measure=0` |
| `WARMUP` | 启动期或历史不足 |
| `TIMESTAMP_ERROR` | 丢样、时间戳跳变或非单调 |
| `FLATLINE_OR_CLIPPED` | 平线或饱和 |
| `LOW_PERFUSION` | 幅值过低 / 低灌注风险 |
| `NO_CANDIDATE` | 无合法候选峰 |
| `AMBIGUOUS_CANDIDATE` | 多候选且无法裁决 |
| `PHYS_LIMIT_FAIL` | IBI 超出生理范围 |
| `SHAPE_FAIL` | 峰形态或模板相关失败 |
| `SANITIZER_REJECT` | interval sanitizer 拒绝 |
| `INTERNAL_ERROR` | 内部状态异常 |

## 5. 状态机建议

M1 只冻结建议状态，不实现状态机。

| 状态 | 含义 |
|---|---|
| `INIT` | 初始化，尚未积累足够历史 |
| `ACQUIRE` | 正在捕获稳定 pulse |
| `TRACK` | 稳定跟踪并可输出有效 IBI |
| `INVALID` | 当前片段不可测或质量不达标 |
| `REACQUIRE` | 从 invalid 或丢失后重新捕获 |

## 6. 回填策略边界

- 允许回填上一搏结果。
- 回填不得超过 `max_output_delay_ms=2000`。
- 回填必须记录输出时间与 beat 时间戳的区别。
- 回填不得修改已确认并已上报很久的历史输出，避免上游状态不一致。

## 7. 公开 API 是否包含 `invalid_reason`

M1 默认策略：

- `invalid_reason` 至少作为 internal/debug 字段存在；
- 是否进入正式公开输出，由后续产品接口决策确认；
- Codex 不得自行删除该内部字段，因为它对调试、QA、回归测试非常重要。
