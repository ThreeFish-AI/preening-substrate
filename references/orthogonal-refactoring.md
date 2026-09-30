# 正交分解与代码清减规约（Orthogonal Decomposition & Pruning）

> 本文是 Phase 3（正交分解与深模块重塑）与 Phase 4（代码清减与语义规范化）的唯一事实源（SSOT）：重构步法状态机、深模块重塑、机制与策略解耦、SSOT 收敛、激进剪枝与 Canonical English 语义规范均以此为准；总控流转见 [SKILL.md](../SKILL.md#工作流规约六阶段自治演进流水线)。

**目录**：[0. 重构步法总则：双阶段帽子与两振出局回退](#0-重构步法总则双阶段帽子与两振出局回退) · [1. 概念主体正交化与深模块重塑](#1-概念主体正交化与深模块重塑) · [2. 机制与策略分离（Mechanism vs. Policy）](#2-机制与策略分离mechanism-vs-policy) · [3. 物理边界隔离、SSOT 收敛与兼容垫片](#3-物理边界隔离ssot-收敛与兼容垫片) · [4. 激进剪枝与过度封装拍平](#4-激进剪枝与过度封装拍平) · [5. 语义精准化重铸与术语规范](#5-语义精准化重铸与术语规范) · [参考（IEEE）](#参考ieee)

---

## 0. 重构步法总则：双阶段帽子与两振出局回退

遵循 Martin Fowler 的行为保持重构纪律与单一活动纪律（Two Hats：一次只戴一顶——重构时不添加功能，添加功能时不重构）[1]，并将其适配到重构内部的两个子阶段，在执行 Phase 3 与 Phase 4 代码修改时，必须恪守以下操作状态机：

1. **Phase 3（结构重塑帽）与 Phase 4（清减内联帽）严格分离**：
   - 在 **Phase 3** 中，只做概念主体的拆分/合并、机制与策略的边界解耦、函数/类的物理迁移以及单一事实源（SSOT）引用切换，**保持算子内部细粒度逻辑不变**；完成迁移后必须立即运行测试护栏确认全绿。
   - 在 **Phase 4** 中，在已稳定的新正交结构上，再执行死代码物理删除、Pass-Through 透传层内联、冗余分支化简与标识符重命名。
2. **微步验证与两振出局回滚（2-Strike Rollback）**：
   - 每次修改控制在单一正交概念或单一重构手法（如 Move Function、Inline Class、Rename Symbol）粒度；
   - 每完成一个微步即运行目标测试套件：
     - **绿灯（PASS）**：保留当前修改，并**必须**以本地 checkpoint commit 固化绿灯锚点（`git add -A && git commit --no-verify -m "wip: <phase>-<n>"`，至迟每完成一个概念主体迁移即打一次），随后继续下一微步；
     - **第 1 次红灯（FAIL）**：仅允许针对刚才那一步修改引发的导入路径或签名遗漏做 1 次即时修正；
     - **连续第 2 次红灯（2nd FAIL）**：**严禁继续在红灯状态下打补丁或猜测性修改**！必须立即执行 `git checkout <上一绿灯SHA> -- <本微步修改过的文件>` 回退（无 checkpoint 时，仅回退本微步在 `progress.md` 登记过的文件清单，并立即补打 checkpoint），将刚才的重构步骤拆解为更小粒度的子步骤重新推进。
   - **回退命令白名单（全程硬约束）**：仅允许限定 pathspec 的 `git checkout -- <pathspec>` / `git checkout <SHA> -- <pathspec>` 与 `git stash push/pop`；严禁 `rm -rf`、`git reset --hard`、`git clean`、`git checkout <sha>`（detach）及对业务仓库的 `git push` / `git rebase`——回退只许触及本流水线自身引入的变更，严禁波及 Phase 0 之前已存在的用户未提交内容。

---

## 1. 概念主体正交化与深模块重塑

针对 Phase 2 识别出的变化维度矩阵与浅模块负债，按以下准则重塑模块结构：

### 1.1 正交提取概念主体 (Orthogonal Concept Extraction)
- **单一变更轴收敛**：每个模块/类/核心函数簇只对**一个独立的变化维度**负责 [2]。
- **单向无环依赖（DAG Dependency Graph）**：
  - 严格确立模块内部层级：**领域核心模型/类型（Domain Types & Invariants） $\leftarrow$ 核心执行机制（Mechanism / Engine） $\leftarrow$ 业务策略/外部适配器（Policies / Adapters） $\leftarrow$ 顶层编排入口（Facade / Entrypoints）**；
  - 下层模块严禁反向 `import` 上层模块；若两模块需要共享契约，将该契约下沉至无依赖的基础模型层，彻底清零循环导入。

### 1.2 合并连体浅模块，锻造深模块 (Forging Deep Modules)
- **反碎片化（Anti-Fragmentation）** [3]：若发现两个或多个类/文件在每次修改时总是共同变化（Co-change）、互相频繁访问对方的私有字段、或一个流程被生硬切成了 `step1.py`, `step2.py`, `step3.py` 且各自只有二三十行——**果断将它们内联合并为单一深模块**。
- **深模块特征（Narrow Interface, Deep Implementation）**：
  - 对外仅暴露极少数高表达力、参数精简、前置条件明确的公开函数或类方法；
  - 将状态校验、边缘分支处理、默认策略降级与算法细节完全封装在模块内部私有域（以 `_` 前缀或模块内局部函数隐藏）。

---

## 2. 机制与策略分离（Mechanism vs. Policy）

将纠缠在同一长函数或上帝类（God Class）中的「如何运行（How to execute）」与「按什么业务规则决策（What rule to apply）」彻底剥离：

1. **底座机制层（Mechanism）**：
   - 提取与具体业务场景无关的通用骨架（如状态流转循环、重试退避框架、事件分发管道、数据流批处理骨架）；
   - 机制层只依赖抽象的函数签名（Callable / Protocol）或标准化配置结构，不包含任何硬编码的业务特判（如 `if tenant == "vip": ...`）。
2. **可插拔策略层（Policy）**：
   - 将多变的业务规则、过滤条件、评分公式与场景特判提取为纯函数（Pure Functions）或轻量无状态策略表（Declarative Rule Table / Strategy Registry）；
   - 优先使用**高阶函数组合、字典映射表（Dispatch Table）或轻量数据声明**替代笨重的面向对象多级继承树（Inheritance Hierarchy）。

---

## 3. 物理边界隔离、SSOT 收敛与兼容垫片

### 3.1 物理目录与导出入口纪律
- 当模块处于 **L2（模块内正交重组）** 或 **L3（跨模块演进）** 且包含多个独立概念主体时，使用清晰的子目录/子文件强化物理边界；
- **收敛门面入口（Facade Entrypoint）**：在模块根入口（如 `__init__.py` 或 `index.ts`）显式声明 `__all__` 或公开 `export` 清单；禁止外部模块直接穿透引用子目录内的私有实现文件。

### 3.2 建立唯一权威定义源 (Single Source of Truth)
- 针对 Phase 2 查出的重复代码、魔法常量、错误码、默认配置与正则规则，保留或提取至唯一的权威定义处（SSOT）；
- 其余所有消费处一律替换为对该权威符号的直接导入引用，确保全模块内**同一领域知识在源码中只出现一次（DRY / Once and Only Once，Hunt & Thomas [4]；Fowler 亦以 duplicated code smell 论之 [1]）**。

### 3.3 跨模块契约演进的向后兼容垫片 (Backward-Compatible Shims)
当属于 **L3（跨模块公共契约演进）** 且重构移动或重命名了曾被外部使用的公开符号时：
1. **首选方案（全仓原子迁移）**：若所有调用方均在当前仓库内且可测试，使用批量替换同步更新全仓所有入站 `import` 与调用点，并跑通全仓测试。
2. **兜底方案（重导出弃用垫片 Deprecation Shim）**：若该符号可能被仓库外组件（如外部插件 `Plugin`、`negentropy` routine、CLI 用户脚本）按旧路径导入，必须在原位置保留极薄的重导出别名（如 `OldName = NewName` 并在注释或 `warnings.warn(..., DeprecationWarning)` 中指明新指针），确保外部消费端 100% 零断裂。

---

## 4. 激进剪枝与过度封装拍平

在 Phase 3 结构稳定且测试全绿后，进入 Phase 4 执行外科手术式清减：

1. **死代码物理清除（Zero-Mercy Dead Code Pruning）**：
   - 彻底删除经 [context-and-audit §2](context-and-audit.md#2-隐式依赖与动态引用四维雷达) 四维雷达与 [§3](context-and-audit.md#3-切斯特顿围栏chestertons-fence考古规程) Chesterton's Fence 考古双重确认无用的：
     - 未调用的私有函数、方法与类；
     - 永远为真或永远为假的死条件分支（Dead Branches）；
     - 函数签名中传入但函数体内从未读取的僵尸参数（Zombie Parameters）；
     - 注释掉的历史废弃代码块（Commented-out Code——历史记录属于 Git，不属于工作区源码）与未使用的导入（Unused Imports）。
2. **拍平单实现接口与透传代理层（Flattening Pass-Throughs）**：
   - 删除仅有一个实现且无多态扩展需求的空洞抽象基类/接口（`IFoo` / `BaseFoo`），将调用方直接绑定到具体深模块；
   - 内联无业务增值的中间透传函数（Pass-Through Functions）与仅做一行的简单包装器（Trivial Wrappers）；
   - 消除模块内部不必要的 `Model A -> Dict -> Model B` 往返转换胶水代码。
3. **控制流扁平化（Guard Clauses over Deep Nesting）**：
   - 使用卫语句（Guard Clauses / Early Return）消除超过 3 层的 `if-else` 金字塔缩进，让主干正常路径（Happy Path）保持顶格清晰直叙。

---

## 5. 语义精准化重铸与术语规范

1. **剿灭泛化垃圾桶词汇（Eliminate Low-Information Identifiers）**：
   - 严禁保留或新建语义宽泛、职责不明的容器与方法名：
     - ❌ 反例：`utils.py`, `helper.py`, `common.py`, `Manager`, `Processor`, `Handler`, `Controller`, `Data`, `Info`, `process_data()`, `handle_item()`, `do_execute()`；
     - ✅ 正例：按其真实领域职责重铸，如 `token_budget_calculator.py`, `CheckpointStore`, `RetryBackoffPolicy`, `normalize_citation_links()`, `prune_orphan_imports()`。
2. **读写语义分离（Command-Query Separation）**：
   - 查询/计算函数以名词或无副作用动词命名（如 `compute_entropy_delta`, `find_inbound_callers`, `is_shallow_module`）；
   - 产生状态变更或 I/O 副作用的命令函数以明确动作动词命名（如 `write_baseline_snapshot`, `rollback_to_checkpoint`, `register_runtime_hook`）。
3. **Canonical English 技术术语保真**：
   - 凡属行业公认且具明确技术语义的核心术语（如 `Harness`, `Agent`, `Subagent`, `Runtime`, `Pipeline`, `Substrate`, `Benchmark`, `Prompt`, `Context`, `Checkpoint`, `Hook`, `SSOT`, `AST`, `LOC`），在代码标识符、注释与文档中**一律保留标准英文原词**，杜绝生硬中译或自创非标简写。
4. **注释精简与对齐（Low-Entropy Comments）**：
   - 代码本身应通过精准命名自解释「做什么（What）」与「怎么做（How）」；
   - 删除所有复述代码字面意思的废话注释；仅在关键架构边界、数学/算法不变量及 Chesterton's Fence 历史防御点保留精炼的「为什么（Why）」注释。

---

## 参考（IEEE）

[1] M. Fowler, *Refactoring: Improving the Design of Existing Code*, 2nd ed. Boston, MA, USA: Addison-Wesley Professional, 2018.

[2] D. L. Parnas, "On the criteria to be used in decomposing systems into modules," *Communications of the ACM*, vol. 15, no. 12, pp. 1053–1058, Dec. 1972, doi: 10.1145/361598.361623.

[3] J. Ousterhout, *A Philosophy of Software Design*. Palo Alto, CA, USA: Yaknyam Press, 2018.

[4] A. Hunt and D. Thomas, *The Pragmatic Programmer: From Journeyman to Master*. Reading, MA, USA: Addison-Wesley, 1999.
