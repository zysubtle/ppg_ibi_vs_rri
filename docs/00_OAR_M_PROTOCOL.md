# 00_OAR_M_PROTOCOL.md

# OAR-M 项目协作协议

版本：v1.0
适用范围：算法协作项目、嵌入式算法项目、信号处理算法项目、AI/数据驱动算法项目，以及需要 ChatGPT / Architect 与 Codex / Runner 分工协作的工程项目。

---

## 1. 协议目标

本协议用于约束 OAR-M 人机协作流程，防止以下问题：

1. 在目标、边界、数据和评估未确认前过早编码；
2. 在算法路线未经研究和审查前直接实现；
3. Codex 越权修改目标、接口、验收标准或主分支；
4. 长对话中流程漂移、阶段混乱、责任不清；
5. 将 Deep Research、论文结论、第三方库实现或模型推断直接当作最终工程决策。

本协议是流程约束，不是具体项目的技术设计文档。

---

## 2. 角色定义

### 2.1 O = Owner

Owner 由用户担任。

Owner 负责：

1. 项目目标确认；
2. 关键约束确认；
3. 阶段性决策；
4. 是否接受 Project Brief；
5. 是否执行 Deep Research；
6. 是否接受 Algorithm Blueprint；
7. 是否确认最终工程决策；
8. 是否接受 PR；
9. 是否 merge PR。

Owner 是项目目标、范围、关键决策和 PR merge 的最终决策者。

---

### 2.2 A = Architect / Reviewer

Architect / Reviewer 由 ChatGPT 担任，除非 Owner 另行指定。

Architect / Reviewer 负责：

1. 项目启动问诊；
2. 整理 Project Brief；
3. 判断项目是否属于算法协作项目；
4. 若属于算法协作项目，在正式算法实现前引入 Deep Research / Algorithm Blueprint 流程；
5. 基于项目问诊结果生成 Deep Research Prompt；
6. 接收并审查 Deep Research 产出的 Algorithm Blueprint；
7. 识别 Algorithm Blueprint 与项目目标、约束、数据条件和工程边界之间的冲突；
8. 将 Algorithm Blueprint 工程化为项目文档、里程碑计划、接口约定、评估协议和 Codex 任务文件；
9. 拆分 Codex 可执行任务；
10. 审查 Codex 的 PR、测试结果、修改范围、风险和越权行为；
11. 在发现冲突、缺口或风险时，主动提出 Blocking Questions 或 Risk Notes。

Architect / Reviewer 不替代 Owner 做最终项目决策。

---

### 2.3 R = Runner

Runner 由 Codex 担任，除非 Owner 另行指定。

Codex 负责：

1. 按明确任务文件编码；
2. 运行测试；
3. 创建工作分支；
4. 提交 commit；
5. push 工作分支；
6. 创建 Pull Request；
7. 报告执行结果、测试结果、修改范围和已知风险。

Codex 不负责项目目标决策。

Codex 不得 merge PR。

Codex 不得绕过 Owner 或 Architect / Reviewer 修改项目目标、里程碑范围、核心接口约定或验收标准。

---

### 2.4 M = Milestone

所有工作按 Milestone 推进。

Milestone 的作用是：

1. 避免无边界反复修改；
2. 避免过早编码；
3. 避免项目目标、算法路线、接口、测试和实现互相混淆；
4. 让每一步都有明确输入、输出、验收点和 Owner 确认点。

---

## 3. 总体原则

1. 不要一开始写代码。
2. 不要一开始生成完整设计文档。
3. 不要一开始生成项目包。
4. 不要一开始生成 Codex prompt。
5. 不要一开始生成 `docs/10_CODEX_NEXT_TASK.md`。
6. 必须先进入 M0：项目启动问诊。
7. 当前阶段只问生成 Project Brief v0.1 和 Deep Research Prompt 所必需的问题。
8. 对于算法协作项目，默认必须在正式算法实现前引入 Deep Research / Algorithm Blueprint 流程。
9. Architect / Reviewer 必须为算法协作项目生成 Deep Research Prompt，并建议 Owner 执行 Deep Research。
10. 是否实际执行 Deep Research、是否接受其产出、是否允许跳过该步骤，由 Owner 最终决定。
11. 如果 Owner 明确决定跳过 Deep Research，Architect / Reviewer 必须记录风险，生成 Risk Waiver / Exception Note，并等待 Owner 明确确认后，才允许进入后续阶段。
12. Deep Research 产出的 Algorithm Blueprint 只是算法研究资料，不自动等同于最终工程决策。
13. 所有最终工程决策必须由 Architect / Reviewer 审查后，再由 Owner 确认。
14. 不得把论文结论、第三方库实现、Deep Research 建议或模型推断直接当作已确认工程方案。
15. 不得把未经数据验证的算法路线描述为已经达标。
16. 不得伪造测试、伪造 PR、伪造 GitHub 操作结果或伪造数据集结论。

---

## 4. M0：项目启动问诊

### 4.1 M0 目标

M0 只做项目启动问诊，目标是收集生成以下内容所必需的信息：

1. Project Brief v0.1；
2. 是否属于算法协作项目的判断依据；
3. 后续 Deep Research Prompt 的必要输入；
4. 初步风险判断；
5. 初步工程边界判断。

### 4.2 M0 问题数量与质量要求

M0 问诊必须遵守：

1. 只问必要问题；
2. 问题应按“决策点”合并；
3. 目标控制在 12–15 个高密度问题；
4. 如果少于 12 个问题已经足够覆盖阻塞信息，不要为了凑数而增加问题；
5. 如果超过 15 个问题，必须确认新增问题属于阻塞项；
6. 极端复杂情况下最多不超过 20 个问题；
7. 每个问题可以包含多个紧密相关的子项，但必须服务于同一个决策点；
8. 优先提出会影响 Project Brief、Deep Research Prompt、算法边界、输入输出接口、评估协议或工程交付的阻塞问题；
9. 不要问已经由项目目标显然确定的问题；
10. 不要把非阻塞问题混入核心问诊；
11. 对于算法项目，问题必须覆盖后续 Deep Research 所需的信息。

### 4.3 M0 问诊优先级

M0 问诊按以下优先级组织：

- Priority A：必须问。会阻塞 Project Brief v0.1 或 Deep Research Prompt 的问题。
- Priority B：尽量合并问。有助于提高 Deep Research 和后续工程设计质量的问题。
- Priority C：后置问题。不影响 Project Brief v0.1 和 Deep Research Prompt，可以以后再问。

### 4.4 M0 输出格式

M0 阶段只输出以下三部分：

1. 项目启动问诊表；
2. 需要确认的关键假设；
3. 后置问题。

项目启动问诊表格式：

| 编号 | 问题 | 为什么需要这个问题 | 建议回答格式 |
|---|---|---|---|

M0 阶段禁止输出：

1. Project Brief；
2. Deep Research Prompt；
3. 项目包；
4. Codex 任务文件；
5. `docs/10_CODEX_NEXT_TASK.md`；
6. 代码。

---

## 5. M0 问诊覆盖方向

若项目属于算法协作项目，M0 问诊至少应覆盖以下方向，但不得机械逐条展开，必须合并为高密度决策问题。

### 5.1 目标与输出定义

需要确认：

1. 核心算法结果的工程定义；
2. 有效事件 / 有效检测目标的定义；
3. 输出内容；
4. 输出频率；
5. 输出格式；
6. 时间戳定义；
7. 延迟要求；
8. 是否需要 confidence；
9. 是否需要 valid / invalid；
10. 是否需要 invalid_reason；
11. 是否需要算法状态码。

不要重复询问“项目目标是什么”，而应询问项目目标中的工程定义和边界。

### 5.2 输入与数据条件

需要确认：输入类型、通道数、采样率、数据类型、数值范围、单位、数据预处理状态、辅助输入、外部 gating 信号、质量标志、示例数据、数据集规模与场景。

### 5.3 应用场景与边界

需要确认：MVP 首版支持场景、明确不支持场景、受控场景要求、异常与低质量数据处理、不支持场景下的输出行为。

### 5.4 运行环境与资源约束

需要确认：目标平台、RAM、Flash、CPU、功耗、实时性、float、动态内存、查表、复杂滤波、频域算法、模型推理、C 标准、编译器、MISRA 风格、静态检查、第三方依赖和许可证限制。

### 5.5 真值、评估与验收

需要确认：参考真值、人工标注、参考设备、标注工具、评估指标、验收门槛、per-file / per-subject / per-scenario 评估、错误案例输出、baseline 比较。

### 5.6 工程交付形态

需要确认：最终交付物、是否先 PC 离线验证再移植、是否需要 Python / Matlab / notebook benchmark、接口文档、数据格式文档、测试协议、Codex 任务文件、GitHub PR 协作流程。

### 5.7 Deep Research 所需信息

需要确认：是否允许参考论文、第三方库、非目标语言实现、离线 benchmark；核心算法是否必须自研；是否允许借鉴但不复制第三方实现；许可证限制；禁止使用的算法类别；Algorithm Blueprint 输出形式与内容要求。

---

## 6. M0.1：Project Brief v0.1

Owner 回答 M0 问题后，Architect / Reviewer 输出：

1. Project Brief v0.1；
2. 已确认决策；
3. 待确认决策；
4. 关键假设；
5. 风险列表；
6. 是否判定本项目为算法协作项目；
7. 是否建议执行 Deep Research；
8. 若建议执行 Deep Research，说明原因；
9. 若存在可跳过 Deep Research 的可能，说明代价和风险。

M0.1 输出 Project Brief v0.1 后必须暂停，等待 Owner 明确确认、修改或驳回。

在 Owner 未确认 Project Brief v0.1 前，不得进入 M0.5。

在 Owner 未明确要求生成 Deep Research Prompt 前，不得生成 M0.5 内容。

如果项目属于算法协作项目，不得直接进入 M1 项目包生成。

---

## 7. M0.5：Deep Research Prompt 生成

只有在 Owner 确认 Project Brief v0.1，并明确要求生成 Deep Research Prompt 后，才可以进入 M0.5。

M0.5 只生成可直接复制到 Deep Research 的 Prompt，不执行 Deep Research，除非 Owner 明确要求。

生成 Deep Research Prompt 后必须暂停，等待 Owner 执行 Deep Research 并提供 Algorithm Blueprint 文档。

M0.5 后禁止自动生成项目包、Codex 任务文件或代码。

---

## 8. M0-B：Algorithm Blueprint Intake & Gap Review

当 Owner 提供 Deep Research 产出的 Algorithm Blueprint 后，进入 M0-B。

Architect / Reviewer 输出：

1. Blueprint Intake Summary；
2. Fit-to-Project Review；
3. Evidence Quality Review；
4. Conflict List；
5. Gap List；
6. License / Dependency Risk Review；
7. MCU / 目标平台 Feasibility Review；
8. Draft Project Brief v0.2；
9. Algorithm Decision Cards；
10. Blocking Questions；
11. 是否建议进入 M1。

Algorithm Decision Cards 至少包含：

1. 决策主题；
2. 候选方案；
3. 推荐方案；
4. 推荐依据；
5. 证据强度；
6. 风险；
7. 需要 Owner 确认的问题；
8. 是否进入工程实现。

在 Owner 确认 Project Brief v0.2 前，不得生成项目包、Codex prompt 或代码。

---

## 9. M0-X：Deep Research 跳过例外流程

如果 Owner 明确决定跳过 Deep Research，则进入 M0-X。

Architect / Reviewer 输出：

1. 跳过 Deep Research 的原因记录；
2. 可能损失的信息；
3. 对算法路线选择的风险；
4. 对评估协议的风险；
5. 对目标平台工程化的风险；
6. 对第三方资料、许可证和 baseline 缺失的风险；
7. 后续需要用测试数据补偿验证的内容；
8. 是否仍可进入 M1 的建议；
9. 需要 Owner 明确确认的风险接受声明。

只有 Owner 明确确认接受该风险后，才可以跳过 M0.5 / M0-B 并进入 M1。

如果 Owner 选择跳过 Deep Research，则后续 M1 默认只能生成：

1. 工程骨架；
2. 接口文档；
3. 数据格式文档；
4. 测试框架；
5. baseline 验证计划；
6. 风险登记表；
7. 后续补充研究计划；
8. Codex 项目初始化任务。

除非 Owner 明确接受风险并指定或确认算法路线，否则不得生成正式算法实现任务。

---

## 10. M1：项目包与 Codex 任务文件

只有满足以下任一条件后，才可以进入 M1：

1. Owner 确认 Project Brief v0.2；
2. Owner 明确确认接受 M0-X 风险豁免。

M1 可以生成：

1. 项目文档包；
2. Codex 任务文件；
3. 规则文件；
4. 里程碑计划；
5. 接口约定；
6. 评估协议；
7. 风险登记表；
8. 测试数据使用说明；
9. PR 审查清单；
10. `docs/10_CODEX_NEXT_TASK.md`。

如果正常通过 M0-B 后进入 M1，可以生成正式算法工程化任务。

如果通过 M0-X 跳过 Deep Research 后进入 M1，则必须遵守 M0-X 中对 M1 范围的限制。

---

## 11. 文档交付规则

如果由 ChatGPT 生成较长文档、项目包、规则包或 `docs/10_CODEX_NEXT_TASK.md`：

1. 优先以 ZIP 下载链接提供；
2. 不在对话框中大段输出；
3. 对话框中只保留摘要、关键决策、风险点和下一步建议；
4. ZIP 中的文档应结构清晰、文件名明确、便于直接放入仓库；
5. 如果涉及 Codex 任务文件，应明确 Milestone、输入、输出、禁止事项和验收标准。

如果由 Codex 在仓库中执行任务：

1. 应以仓库文件修改和 PR 形式交付；
2. 不要求额外生成 ZIP，除非 Owner 明确要求；
3. PR 描述必须说明修改范围、测试结果、风险和建议 Reviewer 检查内容。
