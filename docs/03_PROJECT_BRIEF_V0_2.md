# Project Brief v0.2: Embedded MCU PPG-IBI Algorithm

状态：Owner 已接受。

## 1. 项目目标

开发嵌入式 MCU 端单通道绿光 PPG-IBI 自研算法，用于静息、坐姿、睡眠等低运动场景下，逐搏输出 PPG 主峰到主峰的 IBI。

## 2. IBI 工程定义

- `beat_timestamp_ms`：PPG systolic apex，即 PPG 主峰时间戳。
- `IBI_ms`：相邻两个有效 PPG 主峰时间戳之差。
- 只计算相邻“有效搏动”之间的间期。
- 无效搏动、不支持场景、异常片段输出 invalid，不参与有效 IBI 计算。

## 3. MVP 输入

| 字段 | 定义 |
|---|---|
| PPG 通道数 | 1 |
| 光源 | 绿光 |
| 采样率 | 50 Hz |
| PC 数据类型 | float32 |
| raw 范围 | 0～1 |
| 环境光扣除 | 已完成 |
| 已有预处理 | 无 |
| 外部门控 | `allow_measure` |
| `allow_measure=1` | 外部判断为静止，允许测量 |
| `allow_measure=0` | 外部判断为运动，不允许测量 |

当前无 ACC/IMU、佩戴检测、接触状态、LED 电流、AFE 增益、温度、设备端 quality flag。

## 4. MVP 输出

公开输出字段：

| 字段 | 定义 |
|---|---|
| `IBI_ms` | 相邻有效 PPG 主峰间期，单位 ms |
| `beat_timestamp_ms` | 当前有效 PPG 主峰时间戳，单位 ms |
| `confidence` | 当前 IBI 可靠度评分，初版范围 0～100 |
| `valid_flag` | 当前输出是否可用 |

内部/debug 建议字段：

| 字段 | 用途 |
|---|---|
| `invalid_reason` | invalid 原因枚举，建议内部必须有 |
| `sqi_beat` | beat 级质量分 |
| `sqi_window` | 窗口级质量分 |
| `state` | 算法状态机状态 |
| `peak_amplitude` | 当前峰幅值或脉搏幅值，用于调试 |

## 5. 输出节奏与延迟

- 输出节奏：逐搏输出。
- 最大延迟：≤ 2 s。
- 回填：允许。

## 6. 支持与不支持场景

MVP 支持：

- 静息；
- 坐姿；
- 睡眠；
- `allow_measure=1` 的低运动片段。

MVP 不支持：

- 走路；
- 跑步；
- 日常随机运动；
- 低灌注；
- 松佩戴；
- 显著 AF/PVC/复杂节律异常。

不支持场景输出 invalid。

## 7. 目标 MCU 与工程约束

| 项目 | 约束 |
|---|---|
| MCU | nRF54L15 |
| 算法 RAM 预算 | ≤ 15 KB |
| 算法 Flash 预算 | ≤ 30 KB |
| 语言 | C99 |
| 风格 | MISRA 风格 |
| 动态内存 | 禁止 |
| PC / MCU 双实现 | 是 |
| float | PC 可用；MCU 不把 FPU 作为必须前提 |
| 第三方生产依赖 | 默认零依赖 |

## 8. 推荐算法路线

### 8.1 生产 baseline

Elgendi/Shin 风格低复杂度路线：

- 因果滤波；
- 自适应阈值；
- 局部极大值；
- 最小峰距 / refractory；
- 基础生理范围约束。

用途：M2/M3 离线验证与主算法对照。

### 8.2 推荐主路线

qppgfast-inspired 自研路线：

- `allow_measure` gating；
- 时间戳检查；
- 异常预检查；
- 因果 DC 去除 + 低阶 IIR band-pass；
- positive derivative / slope-sum；
- adaptive threshold candidate detection；
- 原始/弱滤波局部峰顶精修；
- beat/window SQI；
- interval sanitizer；
- calibrated confidence；
- valid/invalid 输出；
- ≤ 2 s 延迟与回填。

### 8.3 离线 benchmark / reference

第三方工具仅用于离线 benchmark、协议验证、对拍或参考，不进入生产 C99 MCU 库。

包括但不限于：WFDB / wfdb-python / XQRS、NeuroKit2、HeartPy、MSPTDfast / ppg-beats、pyPPG、SciPy / MATLAB。

## 9. 真值与评估

真值来源：ECG RRI。

已确认：

- ECG 与 PPG 采样率已对齐；
- ECG 采样率 50 Hz；
- PPG 采样率 50 Hz。

评估必须区分：

1. beat detection 评价；
2. interval accuracy 评价。

不得简单把 PPG 主峰时间戳与 ECG R 峰时间戳做硬对齐后直接评价 IBI。

## 10. 当前数据策略

当前上传的 CSV 只作为 fixture / smoke test，不作为正式评估数据集。

默认不提交原始生理数据或真实 fixture 到 GitHub，除非 Owner 后续明确确认其已脱敏、可提交、用途明确。

## 11. 交付路径

阶段路径：

1. 阶段 1：PC 离线验证工具；
2. 阶段 2：MCU C 库。

最终交付物：

- MCU C 库；
- 接口文档；
- 数据格式文档；
- 测试协议；
- Codex PR 流程；
- milestone 计划。
