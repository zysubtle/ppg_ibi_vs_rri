# 01_DEEP_RESEARCH_PROTOCOL.md

# Deep Research 与 Algorithm Blueprint 协议

版本：v1.0
适用范围：算法协作项目，包括但不限于嵌入式信号处理算法、可穿戴健康算法、医学数据算法、机器学习算法、强化学习算法、传感器融合算法。

---

## 1. 协议目标

本协议用于规范 Deep Research 与 Algorithm Blueprint 在 OAR-M 流程中的角色、边界、输出要求和审查机制。

Deep Research 的目标不是直接产出最终工程决策，而是提供可审查、可追溯、可比较的算法研究资料，供 Architect / Reviewer 审查，并由 Owner 最终确认。

---

## 2. 适用条件

满足以下任一条件的项目，应默认视为算法协作项目，并在正式算法实现前引入 Deep Research / Algorithm Blueprint 流程：

1. 需要选择或设计核心算法路线；
2. 需要参考论文、公开 benchmark、第三方库或工具箱；
3. 涉及信号处理、统计建模、机器学习、强化学习、传感器融合或医学 / 生理数据分析；
4. 算法效果依赖数据分布、真值定义、评估协议或边界条件；
5. 存在 MCU / 嵌入式资源约束、实时性约束或功耗约束；
6. 存在许可证、第三方代码或生产依赖风险；
7. 需要在正式工程实现前比较 baseline 与候选方案。

若 Owner 明确决定跳过 Deep Research，必须走 `00_OAR_M_PROTOCOL.md` 中定义的 M0-X 风险豁免流程。

---

## 3. Deep Research 执行边界

Deep Research 默认由 Owner 在以下方式之一中执行：

1. 单独的 Deep Research 工具；
2. 单独的新对话；
3. Owner 自行组织的人工调研流程；
4. Owner 指定的其他研究流程。

Architect / Reviewer 在 M0.5 阶段只负责生成 Deep Research Prompt，不自动执行 Deep Research，除非 Owner 明确要求。

生成 Deep Research Prompt 后，Architect / Reviewer 必须暂停，等待 Owner 提供 Algorithm Blueprint 文档。

Owner 提供 Algorithm Blueprint 后，Architect / Reviewer 才进入 M0-B：Algorithm Blueprint Intake & Gap Review。

---

## 4. Deep Research Prompt 必须覆盖的内容

Deep Research Prompt 应要求输出 Algorithm Blueprint，并至少覆盖：

1. 项目目标与约束回顾；
2. 输入、输出、场景和边界条件；
3. 相关论文调研；
4. 相关第三方库 / 工具箱调研；
5. 候选算法路线对比；
6. 推荐主算法路线；
7. 推荐 baseline 算法；
8. 预处理方案；
9. 核心检测 / 估计 / 推理流程；
10. 核心事件检测策略；
11. 结果计算策略；
12. SQI 或质量控制策略；
13. 异常处理与 invalid 机制；
14. 后处理策略；
15. confidence 或可靠性评分；
16. 评估协议；
17. 参数初值建议；
18. 计算量、内存、实时性风险；
19. MCU 或目标平台工程化风险；
20. 许可证风险；
21. 不建议采用的算法及原因；
22. 推荐里程碑计划；
23. Open Questions；
24. References；
25. Evidence Traceability；
26. Evidence Strength；
27. Reproducibility Notes；
28. Engineering Decision Boundary。

上述内容可按项目复杂度合并章节，但不得删除关键风险项。

---

## 5. Evidence Traceability 要求

Algorithm Blueprint 对每个关键算法建议必须标注依据来源。

来源类型至少包括：

1. 论文；
2. 标准、指南或官方文档；
3. 开源库；
4. 工具箱；
5. 公开 benchmark；
6. 工程经验；
7. 作者推断；
8. 待验证假设。

不得把“作者推断”或“工程经验”描述为已被论文或数据验证的事实。

---

## 6. Evidence Strength 分级

Algorithm Blueprint 应对关键建议标注证据强度。

建议使用以下分级：

| 等级 | 含义 |
|---|---|
| Strong | 有高相关论文、公开实现、公开 benchmark 或多来源一致支持 |
| Moderate | 有相关论文或工程实现支持，但与当前项目约束不完全一致 |
| Weak | 主要来自有限经验、类比或非完全匹配资料 |
| Assumption | 工程假设，需要后续数据验证 |
| Unknown | 证据不足，必须列为风险或 Open Question |

---

## 7. Reproducibility Notes 要求

Algorithm Blueprint 应说明每个关键建议如何验证。

至少区分：

1. 可用用户数据验证；
2. 可用公开数据验证；
3. 可用合成数据或仿真验证；
4. 可用离线 benchmark 验证；
5. 只能通过后续工程实验验证；
6. 当前无法验证。

不得把 fixture、smoke test 或小样本验证结果夸大为真实数据集评估结论。

---

## 8. Engineering Decision Boundary 要求

Algorithm Blueprint 必须明确区分：

1. 研究结论；
2. 推荐候选方案；
3. 需要 Owner 决策的工程选项；
4. 需要数据验证的假设；
5. 不建议进入工程实现的方案；
6. 已知风险；
7. 尚未解决的问题。

Algorithm Blueprint 不得自动生成最终工程决策。

最终工程决策必须经过 Architect / Reviewer 审查，并由 Owner 确认。

---

## 9. 第三方资料、benchmark、依赖与源码使用规则

必须区分以下四类行为：

1. 参考第三方资料；
2. 使用第三方工具做离线 benchmark；
3. 引入第三方代码作为生产依赖；
4. 复制或改写第三方源码。

规则如下：

1. Deep Research 可以调研、总结和引用论文、第三方库、工具箱、公开 benchmark 和非目标语言实现。
2. 使用第三方工具做离线 benchmark 必须标明工具名称、用途、许可证和是否仅用于研究验证。
3. 引入第三方代码作为生产依赖，必须经过 Owner 明确确认。
4. 复制或改写第三方源码，必须经过 Owner 明确确认，并明确许可证、来源、版权和合规风险。
5. Codex 不得未经 Owner 明确确认复制第三方源码。
6. Codex 不得未经 Owner 明确确认引入新的生产依赖。
7. 对许可证不清楚、来源不清楚或用途不清楚的第三方代码，不得进入工程实现。

---

## 10. 推荐的 Algorithm Blueprint 结构

Deep Research 产出的 Algorithm Blueprint 建议采用以下结构：

```markdown
# Algorithm Blueprint

## 1. Executive Summary

## 2. Project Goal and Constraints

## 3. Input / Output / Scenario Boundary

## 4. Evidence and Source Map

## 5. Related Literature Review

## 6. Third-party Libraries and Toolboxes

## 7. Candidate Algorithm Routes

## 8. Baseline Algorithm Recommendation

## 9. Recommended Main Algorithm Route

## 10. Detailed Processing Pipeline

## 11. Quality Control / SQI / Invalid Strategy

## 12. Confidence / Reliability Scoring

## 13. Exception and Edge-case Handling

## 14. Evaluation Protocol

## 15. Parameter Initial Values

## 16. MCU / Target Platform Feasibility

## 17. License and Dependency Risks

## 18. Algorithms Not Recommended

## 19. Milestone Recommendation

## 20. Open Questions

## 21. References

## 22. Evidence Traceability Appendix
```

---

## 11. M0-B 审查要求

Architect / Reviewer 在接收 Algorithm Blueprint 后，必须执行 M0-B：Algorithm Blueprint Intake & Gap Review。

M0-B 输出至少包括：

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

---

## 12. Algorithm Decision Card 模板

每个关键算法决策应形成一张 Decision Card。

```markdown
# Algorithm Decision Card

## Decision Topic

## Candidate Options

## Recommended Option

## Rationale

## Evidence Source

## Evidence Strength

## Risks

## Validation Required

## Owner Confirmation Required

## Engineering Decision
- [ ] Accept
- [ ] Reject
- [ ] Defer
- [ ] Needs more data
```

---

## 13. 禁止事项

在 Owner 确认前，不得：

1. 把 Deep Research 结论当作最终工程决策；
2. 把未经验证的算法路线描述为已经达标；
3. 将第三方源码复制进工程；
4. 引入新的生产依赖；
5. 改变核心接口、验收标准或里程碑范围；
6. 生成正式算法实现任务；
7. 跳过 Evidence Quality Review；
8. 忽略许可证风险或数据验证风险。
