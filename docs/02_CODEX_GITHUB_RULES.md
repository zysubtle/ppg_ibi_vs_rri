# 02_CODEX_GITHUB_RULES.md

# Codex 与 GitHub 协作规则

版本：v1.0
适用范围：使用 Codex 作为 Runner，在 GitHub 仓库中编码、测试、提交分支、创建 PR 的协作项目。

---

## 1. 协议目标

本协议用于约束 Codex 在仓库中的执行权限，防止以下问题：

1. 直接修改 main / master；
2. 未经审查合并 PR；
3. force push 或覆盖历史；
4. 未经确认改变项目目标、接口或验收标准；
5. 未经确认引入第三方依赖或许可证风险；
6. 提交 secrets、隐私数据、受限数据或人体生理敏感数据；
7. 伪造测试结果、PR 链接或 benchmark 结果。

---

## 2. Codex 运行前提

Codex 可以在满足运行环境条件的前提下执行 GitHub 协作操作。

运行环境条件包括：

1. 当前目录是有效 Git 仓库；
2. 已配置正确 remote；
3. 当前环境具备必要 Git 权限；
4. 当前环境具备 GitHub 访问权限；
5. 如需使用 GitHub CLI，则 GitHub CLI 已安装且已登录；
6. 网络访问可用；
7. 仓库分支保护规则允许创建分支和 PR。

如果上述条件不满足，Codex 必须报告缺失条件，不得伪造 push、PR 或测试结果。

---

## 3. Codex 可以执行的操作

在明确任务文件授权范围内，Codex 可以：

1. 基于任务要求自行创建新的工作分支；
2. 在工作分支上修改代码、文档、测试或配置；
3. 运行必要的测试、构建、静态检查或 smoke test；
4. 创建 commit；
5. 将工作分支 push 到远程仓库；
6. 基于该分支创建 Pull Request；
7. 在 PR 描述中报告执行结果。

Codex 的执行必须受以下文件约束：

1. `docs/00_OAR_M_PROTOCOL.md`；
2. `docs/01_DEEP_RESEARCH_PROTOCOL.md`；
3. `docs/02_CODEX_GITHUB_RULES.md`；
4. 当前里程碑任务文件；
5. Owner 明确确认的 Project Brief；
6. Owner 明确确认的接口、评估协议和验收标准。

---

## 4. 分支规则

Codex 不得在 `main` / `master` 分支上编码。

每个里程碑或明确任务应创建独立工作分支。

推荐分支命名：

```text
feature/m{milestone}-{short-name}
fix/m{milestone}-{short-name}
docs/m{milestone}-{short-name}
test/m{milestone}-{short-name}
```

示例：

```text
feature/m1-project-skeleton
docs/m1-interface-contract
test/m2-fixture-smoke
```

如仓库已有分支命名规范，应优先遵守仓库规范。

---

## 5. Commit 规则

Commit message 应简洁说明里程碑和变更内容。

推荐格式：

```text
M{n}: <summary>
```

示例：

```text
M1: add project skeleton and interface docs
M2: add baseline evaluation CLI
M3: implement peak candidate detector
```

不得在 commit 中包含 secrets、token、证书、隐私信息或受限数据。

---

## 6. PR 描述要求

Codex 创建 PR 时，PR 描述必须包含：

```markdown
## Summary

## Changed Files

## Algorithm Logic Change
- Yes / No
- Explanation:

## Interface / IO Contract Change
- Yes / No
- Explanation:

## Dependency Change
- Yes / No
- Explanation:

## Data Format Change
- Yes / No
- Explanation:

## Test Protocol Change
- Yes / No
- Explanation:

## Test Commands and Results

## Tests Not Run and Reasons

## Known Risks

## Suggested Reviewer Focus

## Out-of-scope Items
```

如果未能运行测试，必须说明原因。

不得声称测试通过，除非实际执行过对应测试。

---

## 7. Codex 绝对禁止的操作

Codex 绝对禁止：

1. 直接在 `main` / `master` 分支上编码；
2. 直接 push 到 `main` / `master`；
3. merge PR；
4. 执行 `gh pr merge`；
5. 执行 GitHub 网页端 merge；
6. 启用 auto-merge；
7. 执行 squash merge、rebase merge 或普通 merge；
8. 删除远程主分支；
9. 覆盖主分支历史；
10. 执行 `git push --force`；
11. 执行 `git push --force-with-lease`；
12. 未经 Owner 明确确认，修改项目目标；
13. 未经 Owner 明确确认，修改里程碑范围；
14. 未经 Owner 明确确认，修改核心接口约定；
15. 未经 Owner 明确确认，修改 IO Contract；
16. 未经 Owner 明确确认，修改验收标准；
17. 未经 Owner 明确确认，引入新的第三方生产依赖；
18. 未经 Owner 明确确认，引入许可证风险不明的代码；
19. 未经 Owner 明确确认，提交原始大数据集；
20. 未经 Owner 明确确认，提交隐私数据、受限数据或人体生理敏感数据；
21. 提交密钥、token、证书、账号凭据或任何 secrets；
22. 伪造测试结果；
23. 伪造 benchmark 结果；
24. 伪造 PR 链接；
25. 声称完成但未实际执行。

PR 的最终 review、是否接受、是否 merge，只能由 Owner 决定。

---

## 8. 第三方依赖与源码规则

Codex 必须区分以下四类行为：

1. 参考第三方资料；
2. 使用第三方工具做离线 benchmark；
3. 引入第三方代码作为生产依赖；
4. 复制或改写第三方源码。

规则如下：

1. 可以根据任务文件阅读、总结或比较第三方资料；
2. 可以在任务明确授权时使用第三方工具做离线 benchmark；
3. 不得未经 Owner 明确确认引入新的生产依赖；
4. 不得未经 Owner 明确确认复制或改写第三方源码；
5. 对许可证、来源或用途不清楚的第三方代码，不得进入工程实现；
6. 如果任务需要参考第三方库，PR 描述必须说明是否复制代码、是否引入依赖、许可证是什么。

---

## 9. 测试数据与 fixture 规则

如果项目涉及真实数据、人体数据、生理数据、设备数据或受限数据，必须遵守：

1. 不得未经 Owner 明确确认提交原始完整数据集；
2. 不得未经 Owner 明确确认提交隐私数据、受限数据或人体生理敏感数据；
3. 不得提交密钥、token、证书、账号凭据或任何 secrets；
4. 允许提交经过 Owner 明确确认的、脱敏的、小型 fixture 数据，用于单元测试、smoke test 或 CI；
5. fixture 数据应尽量小、可复现、用途明确，并在文档中说明来源、用途和限制；
6. 如果不能提交真实 fixture，应使用合成数据或最小模拟数据进行测试；
7. 不得把 fixture 测试结果夸大为真实数据集评估结果。

---

## 10. 测试执行规则

Codex 在 PR 中必须尽量运行与任务相关的测试。

测试类型包括但不限于：

1. 单元测试；
2. smoke test；
3. CLI 自测；
4. 构建测试；
5. 静态检查；
6. 数据解析测试；
7. baseline 评估测试；
8. 回归测试。

如果无法运行测试，必须说明：

1. 缺少什么环境；
2. 缺少什么数据；
3. 缺少什么依赖；
4. 是否可由 Reviewer 或 Owner 后续运行。

---

## 11. 任务范围控制

Codex 只能执行当前任务文件授权的内容。

如果执行中发现任务文件不完整、目标冲突、接口不清、测试不可运行或依赖缺失，Codex 应停止扩大实现范围，并在报告中说明 Blocking Issues。

不得自行扩大里程碑范围。

不得因为测试失败而未经确认重写算法路线、接口或验收标准。

---

## 12. Owner Review 与 Merge

PR 创建后：

1. Codex 不得 merge；
2. Architect / Reviewer 负责审查 PR、测试、风险和越权行为；
3. Owner 决定是否接受 PR；
4. Owner 决定是否 merge PR；
5. Owner 可以要求 Codex 追加 commit 或开新 PR。

如果 PR 涉及算法逻辑、核心接口、数据格式、评估协议或第三方依赖，必须重点审查。
