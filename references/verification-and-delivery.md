# 预检护栏、对抗核验与量化交付规约（Verification & Quantified Delivery）

> 本文是 Phase 0（预检分流与特征化测试护栏）与 Phase 5（对抗核验与量化交付）的唯一事实源（SSOT）：特征化测试接缝提取、独立 Verifier Subagent 盲审协议、多维熵减量化指标与标准化《梳理清减交付报告》模板均以此为准；总控流转见 [SKILL.md](../SKILL.md#工作流规约六阶段自治演进流水线)。

**目录**：[0. 验证黄金准则与反异化防线](#0-验证黄金准则与反异化防线) · [1. Phase 0 预检分流与特征化测试护栏](#1-phase-0-预检分流与特征化测试护栏) · [2. Phase 5 独立 Verifier 盲审与二阶效应核验](#2-phase-5-独立-verifier-盲审与二阶效应核验) · [3. 多维熵减量化度量体系](#3-多维熵减量化度量体系) · [4. 标准化《梳理清减交付报告》模板](#4-标准化梳理清减交付报告模板) · [参考（IEEE）](#参考ieee)

---

## 0. 验证黄金准则与反异化防线

1. **行为等价高于一切（Behavior Preservation First）**：重构不是重写。若清减 50% 代码但破坏了 1 个边缘业务契约，则重构彻底失败 [1]。
2. **对抗 Goodhart 定律（Anti-Goodhart's Law）** [2]：当「代码净减行数（LOC Delta）」成为唯一考核目标时，Agent 极易将易读的多行逻辑压成晦涩的单行嵌套，或删除必要的防御断言。因此，本规约强制采用**多维熵减度量**并配合**独立 Verifier 盲审**，严惩任何牺牲可读性或删除有效测试断言的作弊行为。

---

## 1. Phase 0 预检分流与特征化测试护栏

在触碰目标模块任何一行代码前，Refactorer 必须完成以下预检并写入 `.temp/preening-<slug>-lab/baseline.md`：

### 1.1 工作区初始化与基线采集
```bash
# 0. 脏工作区预检：非空时必须先请用户 commit / stash，
#    或经用户授权将未提交改动纳入行为基线并在 baseline.md 显式登记「含未提交改动」
git status --porcelain -- <目标模块路径>

# 1. 建立自忽略的 lab 目录（防止临时分析文件被 git 提交）
mkdir -p .temp/preening-<slug>-lab && printf '*\n' > .temp/preening-<slug>-lab/.gitignore

# 2. 记录重构前 Git 基线 SHA、目标模块文件数与原始代码行数 (LOC)
git rev-parse HEAD
wc -l <目标模块文件列表>
```

> **回退安全边界**：后续所有微步回退（见 [orthogonal-refactoring §0](orthogonal-refactoring.md#0-重构步法总则双阶段帽子与两振出局回退)）只允许触及本流水线自身引入的变更；若 Phase 0 已登记「含未提交改动」，回退前必须先以 `git stash create` 或 `git diff > .temp/preening-<slug>-lab/wip-snapshot.patch` 留档用户 WIP，严禁直接丢弃。

### 1.2 Feathers 特征化测试护栏（Characterization Tests & Seams）
遵循 Michael Feathers 在 *Working Effectively with Legacy Code* 中的软件接缝（Seam）理论 [3]，对目标模块的测试保护度进行分流处置：

- **工况 A · 既有测试覆盖充分且全绿**：
  - 实跑目标单元测试与集成测试（Python 统一 `uv run pytest <path>`，JS/TS 统一 `pnpm test <path>`），将通过数、耗时与覆盖率快照记入 `baseline.md`；
  - 记录测试文件的 `sha256` 或行数基准，供 Phase 5 核查测试断言是否被偷偷删改。
- **工况 B · 无测试或核心分支裸奔（L4 存量泥球）**：
  - **触发硬门禁**：严禁在无测试保护下直接进入 Phase 3/4 拆解代码！
  - **Dry-Run 模式豁免**：Dry-Run（仅梳理诊断）下本工况**不补写**特征化测试，仅在《全貌梳理与正交分解方案》蓝图中标注「正式执行前须先补建 Golden Master 特征化护栏」；硬门禁在用户确认正式执行时生效。
  - **建立接缝护栏（Seam Protection）**：
    1. 定位目标模块的公开入口函数/类（Public Entrypoints）作为特征化观察点（在 Feathers 意义上的接缝处捕获行为；仅当需要替换协作对象/链接实现时才涉及 Object Seam / Link Seam）；
    2. 构造 3–5 组代表性输入（含典型正常路径、边界空值/极值路径、典型异常路径），实跑当前存量代码，捕获其**真实返回结果、状态变更或输出快照（Golden Master）**；
    3. 将上述输入-输出固化为特征化测试用例（放置在项目 `tests/` 对应目录），实跑确认 100% 全绿后，方可宣布 Phase 0 出闸。

---

## 2. Phase 5 独立 Verifier 盲审与二阶效应核验

为杜绝 Refactorer（主 Agent）既改代码又自己打勾验收的确认偏误（Confirmation Bias），Phase 5 必须执行**双代理对抗核验**——独立 evaluator 的价值已在多代理编排实践中得到验证 [4]。

### 2.1 Verifier Subagent 派发契约
主 Agent 派发一个**全新只读上下文**的 `Verifier Subagent`（或 `research` 子代理），输入严格限制为以下四项客观材料（**绝不传入主 Agent 的重构自评或辩解**）：
1. `.temp/preening-<slug>-lab/baseline.md`（重构前 Git SHA、公开契约表、原始测试基准）；
2. `git diff <baseline_sha>` 与 `git diff --stat <baseline_sha>`——**对工作区取全量变更**（已提交 + 未提交，不依赖变更是否已 commit）；untracked 新文件（特征化测试、新建模块文件）先以 `git add -N` 标记纳入 diff，或附 `git status --porcelain` 清单一并交验；
3. 重构后实跑测试套件与静态检查（Linter / Typecheck）的终端原始输出日志；
4. 下方 §2.2 的《四维盲审核查表》。

### 2.2 四维盲审核查表 (Quad-Gate Audit Checklist)

Verifier Subagent 必须逐项核对并向主 Agent 出具回执（`.temp/preening-<slug>-lab/verification-receipt.md`）：

| 核验维度 | 确定性通过判据（任一不满足即判 `REJECT`） | 核验方法与命令证据 |
| :--- | :--- | :--- |
| **Gate 1 · 测试防篡改与等价性** | 既有业务测试与 Phase 0 特征化测试 100% 通过；**未发生删减有效测试断言、`@skip` 屏蔽用例、或放宽断言精度的 Objective Hacking**（仅随合法内部符号重命名同步更新导入路径除外） | `git diff <baseline_sha> -- tests/` 逐行审阅（覆盖工作区未提交变更）+ 实跑测试日志 |
| **Gate 2 · 公开契约与二阶涟漪防震** | 模块对外 Public API 签名、返回值语义、序列化字段（JSON/Pydantic/ORM）、CLI/配置文件键名及跨仓按名调用入口（如 `negentropy`）零意外破坏；若有跨模块重命名，全仓入站调用已迁移完毕或已保留重导出兼容垫片 | 对比 `baseline.md` 公开契约表与当前导出表；对原符号名执行全仓 `Grep` 确认无悬挂调用 |
| **Gate 3 · 代码卫生与可读性不退步** | 零悬挂导入（Orphan Imports）、零未定义符号、零循环导入；未出现为刷低 LOC 而将清晰多行逻辑塞成一行的反模式；注释精简且无残留的注释掉的代码块 | 静态检查（`ruff check` / `mypy` / `tsc --noEmit`）零报错 + Diff 人工审阅 |
| **Gate 4 · 度量数据真实性对账** | 拟交付报告中的改造前/后 LOC、文件数、净减少行数与优化率，与 `wc -l` 及 `git diff --numstat` 实测数值**逐字精确一致**（零估算、零虚构） | 核对 `wc -l` 与 `git diff --stat` 真实数字 |

- **闭环修复**：若 Verifier 任一 Gate 返回 `REJECT`，Refactorer 必须按回执定位的 `文件:行号` 就地修复（或回滚问题切片）并重新提请核验，直至四门全绿方可出闸。
- **单 Agent 运行时降级**：当宿主环境不支持派发 Subagent 时，主 Agent 必须在完成修改后显式切换至「独立审计员视角」，从磁盘重新执行 `git diff` 与 `wc -l` 逐项填写 `verification-receipt.md`，并在最终交付报告的验证结论栏注明 `（单 Agent 角色隔离核验）`。

---

## 3. 多维熵减量化度量体系

为全面刻画软件架构的真实熵减成效，交付时除统计代码总行数（LOC）外，须从以下多维度量架构收敛度：

1. **代码体量维度（Volume Metrics）**：
   - `代码总行数 (LOC)`：改造前 vs. 改造后、净变化量（$\Delta \text{LOC} = \text{LOC}_{\text{after}} - \text{LOC}_{\text{before}}$）及缩减率（$\frac{|\Delta \text{LOC}|}{\text{LOC}_{\text{before}}} \times 100\%$）。
2. **结构与耦合维度（Structural & Coupling Metrics）**：
   - `文件 / 子模块数`：体现概念主体提取或碎片化浅模块合并后的物理拓扑收敛；
   - `冗余 / 死代码与透传层消除`：物理剪除的废弃分支、死函数、单实现接口套娃与 Pass-Through 代理行数；
   - `公开暴露面收敛`：模块顶层公开导出符号数（Public Surface Area，口径以显式声明为准——Python `__all__` / TS 显式 `export` 计数）；
   - `圈复杂度 / 最大缩进深度`：**定性观察项，不强制入表**（工具口径差异大，如 `radon cc -s <path> --total-average`；仅在显著改善或恶化时以一句话附注）。

---

## 4. 标准化《梳理清减交付报告》模板

每次完成 Phase 5 核验并清理 `.temp/preening-<slug>-lab/` 内临时脚本（保留自忽略的 `progress.md`、`baseline.md`、`topology-and-audit.md`、`verification-receipt.md` 供溯源与评测）后，必须向用户输出严格符合以下结构的标准化交付报告（保持与上游消费方 `ThreeFish-AI/negentropy` 100% 结构兼容）：

```markdown
### 梳理清减交付报告：<模块名称或路径>

> **核心结论**：<一句话总结本次重构的本质演进、核心解耦成果、LOC 净减比例及测试验证结论（附爆炸半径分级 L1/L2/L3/L4）>

#### 1. 全面梳理与核心提炼摘要

- **核心领域职责（Domain Essence）**：<剥离偶然复杂性后，该模块不可剥离的最小本质职责与核心不变量>
- **解耦的正交维度（Parnas Axes of Change）**：
  - **稳定底座机制（Mechanism）**：<提炼出的通用引擎/骨架及其归属路径>
  - **演进业务策略（Policy / Adapter）**：<剥离出的策略规则/外部适配及其归属路径>
- **清理的熵增负债与围栏考古**：
  - **死代码与过度抽象剪除**：<消除的单实现接口套娃、Pass-Through 透传层、重复拷贝与死分支清单>
  - **Chesterton's Fence 保留项**：<经 Git 考古确认为必要防御护栏而予以保留并补注的边缘逻辑（若无则写「无」）>
  - **语义规范化重铸**：<关键模糊标识符 `OldName -> NewName` 对齐摘要>

#### 2. 结构性演进对比 (Architecture Evolution)

- **重构前结构 (As-is)**：
  - <原始拓扑痛点、职责纠缠点、跨层穿透或浅模块碎片化描述>
- **重构后正交结构 (To-be)**：
  - <重构后的概念主体划分、单向依赖流向与 SSOT 权威定义源指针>

#### 3. 熵减量化度量 (Entropy Metrics)

| 指标 | 改造前 (Before) | 改造后 (After) | 净变化 (Delta) | 优化率 (Reduction Rate) |
| :--- | :--- | :--- | :--- | :--- |
| 代码总行数 (LOC) | `X` 行 | `Y` 行 | `-Z` 行 | `XX.X%` |
| 文件/模块数 | `N` 个 | `M` 个 | `±K` 个 | - |
| 冗余/死代码与透传层消除 | - | - | `-D` 行 | - |
| 公开导出符号数 (Public API) | `P1` 个 | `P2` 个 | `±Q` 个 | - |

#### 4. 验证自证结论

- [x] **自动化测试与特征化护栏**：`<测试执行命令与结果：通过用例数 / 耗时，既有断言零弱化>`
- [x] **行为等价性与盲审核验**：`<Verifier Subagent（或单 Agent 角色隔离）四维核验结论：公开契约、序列化与动态反射引用零断裂>`
- [x] **二阶效应与静态检查审查**：`<上下游入站调用兼容性确认 + Ruff/Mypy/TSC 静态检查零告警确认>`
```

---

## 参考（IEEE）

[1] M. Fowler, *Refactoring: Improving the Design of Existing Code*, 2nd ed. Boston, MA, USA: Addison-Wesley Professional, 2018.

[2] M. Strathern, "'Improving ratings': Audit in the British University system," *Eur. Rev.*, vol. 5, no. 3, pp. 305–321, 1997, doi: 10.1017/S1062798700002660.

[3] M. C. Feathers, *Working Effectively with Legacy Code*. Upper Saddle River, NJ, USA: Prentice Hall PTR, 2004.

[4] E. Schluntz and B. Zhang, "Building effective agents," *Anthropic Engineering*, Dec. 19, 2024. [Online]. Available: https://www.anthropic.com/engineering/building-effective-agents
