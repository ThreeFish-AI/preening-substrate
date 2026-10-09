# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [1.0.3] - 2026-10-09

### Added

- 新增 [references/plain-expression.md](references/plain-expression.md)：本 Skill 文本表达质感 SSOT，涵盖读者四结果（ISO 24495-1）、§0「可懂性下限 + 表达力上限」双线划界（均为读者结果）、句构三律、术语治理与首用释义、双通道条目模板、改写筛选与四条禁则（禁则 3 重切营销腔边界：禁不可证伪断言，不禁可指认修辞）、表达力四维（具象优先 / 关系结构类比 / 节奏与压力位 / AI 腔防治）、外行盲评七问（先读后问防刷分）与改动验证四查，附 12 条 IEEE 参考文献。

### Changed

- [SKILL.md](SKILL.md) 叙述性文字改写：重构十铁律各附一句「人话」复述，易错点中「过程记录」「测试替身」两条同附（判定规则语义不变，不降级任何模态词；铁律 7 把原「分步执行」直接写成「严禁混在同一次修改里」，并把 Fowler 出处括注改为指向 [orthogonal-refactoring §0](references/orthogonal-refactoring.md#0-重构步法总则双阶段帽子与两振出局回退) 的指针）；开篇「唯一目标」段改写为结论先行的三步式并给「偶然复杂性」「低熵」补首用释义（原「认知演进法则」长句拆入三步，「正交内聚」由铁律 6 与 Phase 3 承载），铁律 1 给「熵增负债」补首用释义；分流矩阵、双代理核验与 Phase 0–2 各段先给一句问题陈述，Phase 3 开篇点明「只动结构」，Phase 5 开篇交代执行顺序；「边界与触发契约」拆为四段；SSOT 指针索引登记新手册，Phase 5 必读挂载扩至 §7；frontmatter 版本升至 1.0.3；README 仓库结构树新增 plain-expression 登记行、knowledge-map 登记行同步。标题零触碰，冻结面除 README 仓库结构树新增 1 行登记外零变动，非本规约文件净增 ≤ 60 行达标，16 词硬约束台账无 LOSS（SKILL.md「严禁」+1，见铁律 7）。

## [1.0.2] - 2026-09-30

### Added

- **三层渐进式披露架构 (3-Tier Progressive Disclosure)**：将原单体 `SKILL.md` 解耦为「精炼主控骨架 [SKILL.md](SKILL.md) + 按阶段按需加载的四大权威规约手册 `references/*.md`」，显著降低常驻上下文负载并防止长链路 Context Rot：
  - 新增 [references/context-and-audit.md](references/context-and-audit.md)：涵盖公开契约盘点、动态引用四维雷达（Quad-Radar：反射/DI、序列化、配置字符串、动态导出）、Chesterton's Fence（切斯特顿围栏）Git 考古规程、Brooks 本质复杂性萃取、Parnas 变化维度矩阵与 Ousterhout 浅模块/透传层定量审计及 IEEE 参考文献；
  - 新增 [references/orthogonal-refactoring.md](references/orthogonal-refactoring.md)：涵盖双阶段帽子（Structural/Pruning Hats，适配自 Fowler Two Hats 单一活动纪律）与两振出局微步回滚状态机、深模块重塑、机制与策略解耦、SSOT 收敛、跨模块向后兼容重导出垫片（Deprecation Shims）、激进剪枝与 Canonical English 语义重铸规范；
  - 新增 [references/verification-and-delivery.md](references/verification-and-delivery.md)：涵盖 Phase 0 Feathers 特征化测试（Characterization Tests / Golden Master）接缝提取护栏、Phase 5 独立 Verifier Subagent 四门禁盲审（Quad-Gate）清单、防 Goodhart 异化的多维熵减量化指标及标准化《梳理清减交付报告》模板；
  - 新增 [references/rsi-hook.md](references/rsi-hook.md)：引入横切 Phase 0–5 的跨会话 RSI（Recursive Self-Improvement）自我改进协议，支持规约缺陷旁路捕获与 G0–G4 五道门禁独立核验 PR 回馈。
- **标准化评测基准集 (`evals/`)**：
  - 新增 [evals/trigger-evals.json](evals/trigger-evals.json)：27 条正负双向触发评测用例（13 应触发 + 14 近邻不触发，含 TS/JS 技术栈样本与近邻技能负例），精确校准 `description` 路由边界；
  - 新增 [evals/evals.json](evals/evals.json)：7 大典型工程重构场景（L1 单文件清减、L2 模块内正交重组、L4 无测试遗产代码护栏、L3 跨模块公共契约演进、动态反射防误删、Dry-Run 只读审计、重构与新特性边界）的端到端验收契约集。
- **知识索引与工作区卫生**：新增 [docs/.agents/knowledge-map.md](docs/.agents/knowledge-map.md) 统一维护全仓知识索引；`.gitignore` 新增 `.temp/` 规则以配合 `.temp/preening-<slug>-lab/` 抗压缩状态落盘。

### Changed

- **流水线升维为六阶段自治演进体系 (`Phase 0` → `Phase 5`)**：
  - 增设 `Phase 0 · 预检分流与特征化测试护栏` 与 **四级输入分流及爆炸半径矩阵（L1–L4 + Dry-Run）**，彻底解决无测试存量代码盲目重构与跨模块契约断裂风险；Dry-Run 模式全程零写入（特征化护栏延后至正式执行时补建）；
  - 确立 **重构十条铁律（Iron Laws 1–10）** 与 **双代理对抗核验（Refactorer ↔ Independent Verifier Subagent）**，消除单 Agent 自改自审的确认偏误；Verifier diff 契约对工作区取全量变更（`git diff <baseline_sha>`，不依赖已提交），微步绿灯强制本地 checkpoint 回退锚点；
  - Frontmatter 升级：补齐 `compatibility` 跨宿主与降级声明，扩展 `description` 显式包含中英双语正向触发词与负向禁用边界，`allowed-tools` 改空格分隔对齐开放标准，同时 100% 保持 `name: preening-substrate` 及与 `ThreeFish-AI/negentropy` routine preset `preening_substrate` 的调用与交付模板（四大板块标题级）兼容性。

### Fixed

- 修复 README 安装命令：symlink 安装统一改用 `ln -sfn`，杜绝重复执行时在仓库内部产生自引用嵌套目录；`.gitignore` 追加 `/preening-substrate` 兜底（详见 [docs/.agents/issue.md](docs/.agents/issue.md)）。

## [1.0.1] - 2026-09-22

### Changed

- **方法论体系升维**：全面确立「先全面梳理，再提炼核心、正交分解」的认知演进法则，将技能从扁平原则升维为标准化 **5 阶段结构性熵减流水线 (5-Phase Execution Pipeline)**：
  - `Phase 1`: 全面梳理 (Exhaustive Context Mapping)
  - `Phase 2`: 提炼核心 (Core Distillation & Entropy Audit)
  - `Phase 3`: 正交分解 (Orthogonal Decomposition)
  - `Phase 4`: 代码清减与语义规范化 (Pruning & Semantic Normalization)
  - `Phase 5`: 闭环验证与量化交付 (Verification & Quantified Delivery)
- **强化道法术规范体系**：确立道（认知心法：上下文质量第一、熵减、演进式设计、二阶思维）、法（架构原则：先全貌后局部、正交分解、单一事实源、交付前验证定式）与术（5 阶段细则）三层结构，补充 Mermaid 执行管线图。
- **标准化交付模板**：明确结构性演进对比与熵减量化度量表格（LOC 增减、文件变动、优化率）及客观自证核验清单。
- **文档与元数据同步**：更新 [SKILL.md](SKILL.md) 版本至 `1.0.1`，同步升级 [README.md](README.md) 的工作流描述。

## [1.0.0] - 2026-09-22

### Added

- 自本机用户级 slash command `~/.claude/commands/preening-substrate.md`（2026-05-16 创建）迁出为独立可安装技能：对目标模块执行正交分解、语义规范化与代码清减的结构性熵减方法论。
- 技能正文零改动迁移（byte-identical）；frontmatter 升级为开放 schema（name / description 含 Use when 触发契约 / license / metadata / allowed-tools）。
