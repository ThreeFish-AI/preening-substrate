# Knowledge Map (知识索引)

> 本索引维护 `preening-substrate` 仓库内全部核心契约、渐进式披露操作手册（References）、评测基准集（Evals）与工程治理文档的单一事实源（SSOT）拓扑。

## 1. 核心入口与总控契约 (Core Entrypoints)

| 文档路径 | 权威职责定位 (SSOT) | 目标读者 / 激活时机 |
| :--- | :--- | :--- |
| [SKILL.md](../../SKILL.md) | 技能总控骨架：Frontmatter 路由契约、重构十铁律（Iron Laws 1–10）、双代理对抗核验、易错点（Gotchas）、四级输入分流矩阵（L1–L4）、阶段验收总表与六阶段自治演进流水线 | Agent 技能激活时首先加载 |
| [README.md](../../README.md) | 人类可读全景指南：核心机制创新、跨宿主安装矩阵（Claude Code / Antigravity）、触发方式、`negentropy` 契约说明与仓库拓扑 | 开发者与用户在安装/查阅时阅读 |
| [CHANGELOG.md](../../CHANGELOG.md) | 版本演进历史（Keep a Changelog 规范） | 版本发布与升级审计时查阅 |

## 2. 渐进式披露操作手册 (On-Demand Reference Manuals)

| 文档路径 | 覆盖阶段 | 权威内容与理论锚点 |
| :--- | :--- | :--- |
| [references/context-and-audit.md](../../references/context-and-audit.md) | `Phase 1` · `Phase 2` | 公开契约与入站/出站拓扑测绘、动态引用四维雷达、Chesterton's Fence Git 考古规程、Brooks 本质复杂性萃取、Parnas 变化维度矩阵、Ousterhout 浅模块与透传层定量审计 |
| [references/orthogonal-refactoring.md](../../references/orthogonal-refactoring.md) | `Phase 3` · `Phase 4` | 双阶段帽子（适配自 Fowler Two Hats）与 2-Strike 微步回退状态机、深模块重塑、机制与策略（Mechanism vs. Policy）解耦、SSOT 收敛、向后兼容重导出垫片（Deprecation Shims）、激进死代码剪枝与 Canonical English 语义重铸 |
| [references/verification-and-delivery.md](../../references/verification-and-delivery.md) | `Phase 0` · `Phase 5` | Feathers 特征化测试（Characterization Tests / Golden Master）接缝护栏、独立 Verifier Subagent 四门禁盲审清单（Quad-Gate / Gate 1–4）、防 Goodhart 异化的多维熵减量化指标与标准化《梳理清减交付报告》模板 |
| [references/plain-expression.md](../../references/plain-expression.md) | `Phase 5` · `文本修改（横切）` | 本 Skill 文本表达质感 SSOT：读者四结果（ISO 24495-1）、句构三律、术语治理与首用释义、双通道条目模板、改写筛选与四条禁则、改动验证四查 |
| [references/rsi-hook.md](../../references/rsi-hook.md) | `RSI（横切）` | 跨会话自我改进（Recursive Self-Improvement）协议：触发白名单、`.temp/preening-substrate-rsi/` 旁路捕获、Steward ↔ Verifier 五道核验门禁（G0–G4）、背压限流与 PR 模板 |

## 3. 评测集与工程治理台账 (Evaluation Suites & Governance)

| 文档路径 | 权威职责定位 (SSOT) |
| :--- | :--- |
| [evals/trigger-evals.json](../../evals/trigger-evals.json) | 27 条正负双向触发评测集（13 正向应触发 + 14 近邻不触发），用于回归校验 `SKILL.md` frontmatter `description` 的路由召回率与特异性 |
| [evals/evals.json](../../evals/evals.json) | 7 大典型重构场景（L1 单文件清减、L2 模块内正交重组、L4 无测试存量泥球、L3 跨模块公共契约演进、动态反射防误删、Dry-Run 只读诊断、重构与新特性职责边界）端到端验收测试集 |
| [docs/.agents/issue.md](./issue.md) | 历史工程 Issue 台账（现象、表因、根因、处理方式与长效防范约束） |
