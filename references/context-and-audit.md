# 全面梳理与熵增审计规约（Context Mapping & Entropy Audit）

> 本文是 Phase 1（全面梳理与拓扑测绘）与 Phase 2（核心提炼与熵增审计）的唯一事实源（SSOT）：盘点清单、动态引用四维雷达、Chesterton's Fence 考古规程、Parnas 变化维度矩阵与 Ousterhout 熵增负债判据均以此为准；总控流转见 [SKILL.md](../SKILL.md#工作流规约六阶段自治演进流水线)。

**目录**：[0. 落盘契约与上下文防腐](#0-落盘契约与上下文防腐) · [1. 全面梳理：资产盘点与拓扑测绘](#1-全面梳理资产盘点与拓扑测绘) · [2. 隐式依赖与动态引用四维雷达](#2-隐式依赖与动态引用四维雷达) · [3. 切斯特顿围栏（Chesterton's Fence）考古规程](#3-切斯特顿围栏chestertons-fence考古规程) · [4. 本质复杂性萃取与 Parnas 变化维度矩阵](#4-本质复杂性萃取与-parnas-变化维度矩阵) · [5. Ousterhout 熵增负债定量审计清单](#5-ousterhout-熵增负债定量审计清单) · [参考（IEEE）](#参考ieee)

---

## 0. 落盘契约与上下文防腐

重构中大型模块时，跨文件检索与调用图追踪极易耗尽或触发宿主自动上下文压缩（Context Compaction）。为防止后半程（Phase 3–4）因遗忘 Phase 1–2 的排查细节而发生盲删误改，**所有梳理与审计产物必须随产随存于 `.temp/preening-<slug>-lab/topology-and-audit.md`**：

1. **只读测绘铁律**：在 `topology-and-audit.md` 全部章节填写完毕并勾选 `progress.md` 的 Phase 1、Phase 2 门禁前，严禁对目标业务文件执行任何 `Write` / `Edit` 修改。
2. **锚点精确到行**：所有识别出的公开入口、隐式副作用点、动态绑定点与待清理负债，必须登记精确的 `<文件路径>:<起止行号>` 锚点，拒绝模糊印象描述。
3. **敏感值脱敏红线**：环境变量与配置扫描**只登记键名 / 变量名及其绑定位置（`文件:行号`）**；严禁将 `.env` 值、凭证、Token、连接串等敏感值写入任何 `.temp/` 产物、交付报告或 RSI 条目。宿主安全策略禁读 `.env` 类文件时，跳过该文件并在 `topology-and-audit.md` 注明跳过原因。

---

## 1. 全面梳理：资产盘点与拓扑测绘

对目标模块执行自顶向下的全量扫描，回答以下四个结构性问题并写入 `topology-and-audit.md`：

### 1.1 资产清单与公开契约边界 (Public Surface Inventory)
- **模块物理边界**：列出模块下所有源文件、子目录、行数（LOC）及模块级配置文件。
- **公开导出面（Public API & Entrypoints）**：
  - 显式导出：Python 的 `__all__` / 顶层 `__init__.py` 重导出，TypeScript/ESM 的 `index.ts` / `export` 声明，Go 的首字母大写符号，Rust 的 `pub` 符号；
  - 隐式被外部消费的内部符号：在目标模块目录之外执行全仓 `Grep`，找出所有直接跨过模块顶层入口、深入内部子文件引用的入站调用点（Inbound Deep Imports）。

### 1.2 入站与出站调用拓扑 (Inbound / Outbound Call Graph)
- **上游入站消费方（Inbound Callers / Fan-in）**：谁在调用本模块？调用频率、参数契约、异常捕获预期与返回值结构是什么？
- **下游出站依赖方（Outbound Dependencies / Fan-out）**：本模块依赖哪些内部基础设施、第三方库、网络 RPC、文件系统或数据库表？
- **环形依赖检测（Circular Dependency Probe）**：检查是否存在 `A -> B -> A` 的直接或间接循环导入（包括运行时函数内延迟 `import` 掩盖的逻辑环）。

### 1.3 全局状态与副作用热点 (State & Side-Effect Hotspots)
逐一排查并标记以下高危耦合点：
- 模块导入期副作用（Import-time Execution）：模块加载时即执行的 I/O、单例初始化、全局注册表写入（Registry Mutation）或 monkey-patching；
- 可变全局状态（Mutable Global / Class-level State）：跨请求共享的模块级字典、缓存、线程池、锁对象或环境变量直读（`os.environ` / `process.env`）；
- 隐式时序契约（Temporal Coupling）：必须先调用 `init()` / `prepare()` 才能调用 `run()` 的状态机顺序依赖。

---

## 2. 隐式依赖与动态引用四维雷达

**核心警示**：静态文本检索（`Grep` / IDE Find Usages）报告「0 引用」**绝不等于死代码**。在将任何类、函数、字段或常量列入待删清单前，必须通过以下**四维动态引用雷达**逐项排雷：

| 雷达维度 | 高频隐式调用机制 | 必查检索策略与判据 |
| :--- | :--- | :--- |
| **① 框架反射与依赖注入 (Reflection & DI)** | FastAPI/Flask/Spring 路由与依赖装饰器、Pytest fixture 动态注入、插件钩子（`pluggy` / `entry_points`）、元类（Metaclass）自动注册子类、`getattr(obj, f"handle_{kind}")` 拼接分发 | 检索模块内所有装饰器（`@`）、基类 `__init_subclass__`、以及按前缀/后缀拼接函数名的 `getattr` / `reflect` 模式 |
| **② 序列化与数据契约映射 (Serialization & ORM)** | Pydantic `BaseModel` / `dataclass` 字段、SQLAlchemy/Prisma/TypeORM 列映射、Protobuf/GraphQL schema、JSON/YAML 反序列化模型字段 | 凡属于对外 API 请求/响应模型、持久化实体或事件消息 Payload 的字段，改名或删除前必须核查上下游序列化契约与数据库迁移记录 |
| **③ 配置与 CLI 字符串绑定 (Config & CLI Binding)** | `pyproject.toml` (`[project.scripts]`)、`package.json` (`bin`/`exports`)、Celery/Cron 任务路径字符串（`"pkg.module:func"`）、YAML/JSON/TOML 配置文件中的类名或策略名 | 以符号名（及 kebab-case / snake_case 变体）对全仓 `.toml`, `.json`, `.yaml`, `.yml`, `.ini`, `.env.example` 与测试夹具做全字面量扫描；`.env` 类真实环境文件仅做**键名匹配、不读取值**（见 §0 脱敏红线） |
| **④ 动态导出与跨仓按名契约 (Dynamic Exports & Cross-Repo)** | `__all__` 动态拼接、`importlib.import_module`、动态插件加载，以及外部宿主/调度器按约定名称调用（如 `negentropy` routine preset 消费 `preening_substrate`） | 检查顶层导出表、插件清单与 README/文档中声明的外部集成契约；凡属外部契约锚点，严禁单方面删改名称 |

---

## 3. 切斯特顿围栏（Chesterton's Fence）考古规程

面对代码中看似怪异、啰嗦、反直觉或疑似「多余」的逻辑（如特定的 `sleep`、特殊字符清洗、空值双检、看似不会触发的异常分支、版本兼容分支），**严禁凭主观洁癖直接删除** [1]。必须先执行 Git 考古三步法：

```bash
# 1. 追溯目标代码段的首次引入与历次修改提交
#    -S / --grep 的检索词必须为标识符级 token（[A-Za-z0-9_.-]）或经变量双引号传入
#    （如 git log -S "$sym"）；含引号/元字符的多行片段改用 git log -G 配合从文件读取的正则，
#    严禁将原始代码文本直接内插进命令字符串
git log -S '<标识符级符号名>' -p -- <文件路径>

# 2. 定位精确行号的最后修改原因与关联上下文
git blame -L <起始行>,<结束行> -- <文件路径>

# 3. 检索 docs/.agents/issue.md 或提交历史中的关联缺陷记录
git log --grep='<关键词或 Issue 编号>' -n 5
```

**去留裁决矩阵**：
- **保留并补齐精简注释（Keep & Document）**：若考古证实该逻辑是为了防御真实发生过的线上边缘 Bug、第三方 SDK 怪癖、并发竞态或平台兼容性问题，且触发条件依然存在——**严禁删除**；若原代码缺乏说明，补一行精炼注释指明其防御意图。
- **物理剪除（Safe Prune）**：仅当考古证实该逻辑服务于早已下线的旧版本、已废弃的特性开关（Feature Flag）、或纯属合并冲突遗留的死分支，且通过 §2 四维雷达确认零消费时，方可标记为 `[可安全剪除]`。

---

## 4. 本质复杂性萃取与 Parnas 变化维度矩阵

存量模块因持续演化而结构熵增、偶然复杂性不断累积（Lehman 软件演化定律 [5]）。遵循 Fred Brooks 的复杂性二分法 [2] 与 David Parnas 的信息隐藏准则 [3]，在 `topology-and-audit.md` 中完成核心提炼：

### 4.1 本质复杂性（Essential Complexity）一句话定义
剔除所有框架胶水、缓存包装、格式转换与历史补丁后，用**不超过一句话**回答：
> 「本模块之所以必须存在的唯一领域职责是什么？它接收什么本质输入，维护什么核心不变量（Invariants），产生什么本质输出？」

凡不直接服务于该核心不变量与本质契约的代码，默认归入**偶然复杂性（Accidental Complexity）**候选审视池。

### 4.2 Parnas 变化维度矩阵 (Change-Dimension Matrix)
切分模块的黄金标准**绝不是按代码执行的先后流程图（Step 1 → Step 2 → Step 3）分段**，而是**按驱动代码未来发生修改的独立诱因（Axes of Change / Secrets）正交分解** [3]。在审计报告中构建下表：

| 独立变化维度 (Axis of Change) | 层级属性 | 包含的核心决策 / 被隐藏的秘密 (Hidden Secret) | 当前散落的源码位置 (`文件:行号`) | 目标正交归属模块 |
| :--- | :--- | :--- | :--- | :--- |
| **底层执行机制 (Mechanism)** | 稳定底座（低频变更） | 状态机流转引擎、并发调度器、通用重试/管道骨架、基础协议编解码 | `...` | `<core_engine>` |
| **业务规则与策略 (Policy)** | 演进策略（高频变更） | 评分权重、路由启发式规则、领域校验阈值、特定场景分支策略 | `...` | `<policies_or_rules>` |
| **外部基础设施适配 (Adapter)** | 边界适配（随外部依赖变） | 数据库/存储读写、外部 API 客户端、CLI/HTTP 入参解析与格式化 | `...` | `<adapters_or_io>` |

**正交性检验判据**：当未来某一项业务策略改变（或更换底层存储实现）时，修改是否能**严格闭合在单一目标模块内部**，而无需同时触碰另外两个维度的代码？若一处变更需跨多个维度联动修改（Shotgun Surgery），说明分解尚未正交。

---

## 5. Ousterhout 熵增负债定量审计清单

依据 John Ousterhout 在 *A Philosophy of Software Design* 中提出的模块深度理论 [4]，以「模块价值 ≈ 功能深度 / 接口复杂度」（$\text{Module Value} = \frac{\text{Functionality Depth}}{\text{Interface Complexity}}$，系本规约对其深模块思想的公式化，非原书公式），逐类扫描并登记四大熵增负债：

### 5.1 浅模块与过度拆分（Shallow Modules & Classitis）
- **症状**：类或函数的接口声明（参数列表、配置项、类型包装）极其繁琐，但内部实际只做了几行微不足道的赋值或简单调用；或者将一个完整的 150 行内聚算法拆成 6 个互相调用、被迫暴露大量中间状态的微小文件。
- **处方**：标记为 `[待内联合并 -> 重塑深模块]`，在 Phase 3–4 将连体浅模块合并，收窄对外接口，把实现复杂性深藏于模块内部。

### 5.2 透传层与单实现接口套娃（Pass-Through Layers & Single-Impl Interfaces）
- **症状**：
  - **透传方法（Pass-Through Methods）**：方法 `A(x, y, z)` 除了原封不动调用下层 `B(x, y, z)` 外不承担任何领域逻辑转换或权限边界控制；
  - **接口套娃（Speculative Generality）**：为「未来某天可能替换实现」而预先定义的 `AbstractFooService` / `IFooRepository`，但全仓库自始至终只有唯一一个实现类 `FooServiceImpl`；
  - **冗余 DTO/Model 搬运**：在同一模块内部跨两层函数调用时，将完全相同的字段在 `Dict`、`Dataclass`、`VO`、`DTO` 之间反复拆包重装。
- **处方**：标记为 `[待拍平/内联]`，在 Phase 4 直接由调用方对接真实实现或统一使用单一领域数据结构。

### 5.3 信息泄漏与单一事实源（SSOT）分裂
- **症状**：同一个魔法常量、正则表达式、状态枚举、默认配置字典或业务校验公式，通过复制粘贴（Copy-Paste）散落在 2 个及以上文件中；或模块内部的实现细节（如底层特定数据表结构、第三方库专属异常类型）直接穿透泄漏到上层业务调用方。
- **处方**：标记为 `[SSOT 收敛]`，指定唯一的权威定义文件与符号名，其余副本全部改为指针引用。

### 5.4 语义失真与低信噪比命名（Semantic Distortion）
- **症状**：
  - 万能垃圾桶命名：`utils.py`, `helpers.py`, `common.py`, `misc.py`, `DataManager`, `ProcessHandler`, `InfoObject`, `do_work()`；
  - 名实不符：函数名为 `get_xxx()` 却在内部暗中修改全局状态或写入磁盘（违背 Command-Query Separation）；
  - 术语不统一：同一概念在不同文件里混用 `task` / `job` / `work_item` 或将标准英文技术术语生硬直译为拼音/含糊词。
- **处方**：登记 `[原标识符 -> 目标精准标识符]` 映射表，供 Phase 4 一次性原子重铸。

---

## 参考（IEEE）

[1] G. K. Chesterton, *The Thing*. London, U.K.: Sheed & Ward, 1929.

[2] F. P. Brooks, "No silver bullet—Essence and accidents of software engineering," *Computer*, vol. 20, no. 4, pp. 10–19, Apr. 1987, doi: 10.1109/MC.1987.1663532.

[3] D. L. Parnas, "On the criteria to be used in decomposing systems into modules," *Communications of the ACM*, vol. 15, no. 12, pp. 1053–1058, Dec. 1972, doi: 10.1145/361598.361623.

[4] J. Ousterhout, *A Philosophy of Software Design*. Palo Alto, CA, USA: Yaknyam Press, 2018.

[5] M. M. Lehman, "Programs, life cycles, and laws of software evolution," *Proceedings of the IEEE*, vol. 68, no. 9, pp. 1060–1076, Sept. 1980, doi: 10.1109/PROC.1980.11805.
