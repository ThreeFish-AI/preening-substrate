# 文本表达质感规约（Plain Expression）

> 本文是本仓库全部文本表达质感的唯一事实源（SSOT）：读者四结果、句构三律、术语治理、双通道条目模板、改写筛选与禁则、改动验证均以此为准；总控流转见 [SKILL.md](../SKILL.md#引用与指针索引-single-source-of-truth)。

**目录**：[0. 定位、适用范围与冻结面](#0-定位适用范围与冻结面) · [1. 读者四结果](#1-读者四结果) · [2. 句构三律](#2-句构三律) · [3. 术语治理](#3-术语治理) · [4. 双通道条目模板](#4-双通道条目模板) · [5. 改写筛选与四条禁则](#5-改写筛选与四条禁则) · [6. 改动验证](#6-改动验证) · [参考（IEEE）](#参考ieee)

---

## 0. 定位、适用范围与冻结面

本规约治理本仓库全部人类可读文本：[SKILL.md](../SKILL.md)、`references/` 全部手册、[README](../README.md)、交付报告叙述区与 RSI PR 正文。表达合格的唯一标准是读者结果，不是文风偏好：**读者拿到需要的、找得到需要的、首读就懂、拿来能用** [1]。

**冻结面清单**（承载既有契约，改写一律绕行；冻结面内的历史措辞不受本规约追溯）：

1. 全部标题行（`#` – `######`）：跨文件锚点与目录锚的字节级依赖；
2. 一切既有 fenced code block：命令、脚本、路径约定、报告模板与 mermaid 图；
3. `evals/` 两份 JSON 引用的契约字符串（`.temp/preening-<slug>-lab`、`baseline.md`、`topology-and-audit.md`、`verification-receipt.md`、`progress.md`、《梳理清减交付报告》等）：正文中只能原样出现；
4. 交付报告模板整块：`negentropy` 消费四大板块标题；
5. 分流矩阵 L1–L4 行标签与「判定特征」列：评测语义引用；
6. frontmatter 触发契约字段与全部「」触发短语。

## 1. 读者四结果

每段文本交付前过四问（依据 ISO 24495-1:2023 [1]，各操作化为本仓可勾验判据）：

| 结果 | 问题 | 可勾验判据 |
| :--- | :--- | :--- |
| **Relevant（相关）** | 读者需要的是不是这段话？ | 删掉后任务执行不受影响的句子，删 |
| **Findable（可找到）** | 需要时找得到吗？ | 标题写答案、关键词前置、指针表可导航 |
| **Understandable（首读即懂）** | 外行首读能否复述这条规则要求什么？ | 句构三律（§2）+ 术语治理（§3） |
| **Usable（可用）** | 读者能直接照做吗？ | 判据、命令、锚点齐全，无「按老规矩办」式引用 |

## 2. 句构三律

1. **一句一事**：每句只做一个断言；40 字为参考值而非硬上限（规约定义句豁免）。命中即拆句。禁令被长句淹没时，拆出来的短句反而更有力——「无基线护栏，严禁进入结构变更」单列成句即是此类。
2. **旧先新后**：句首主语位放读者已知的信息，句末压力位放希望读者带走的新信息 [2]。本仓应用：开篇先给 payoff（「行为严格不变」），手段与出处后置；出处声明不夹在操作命令中间。
3. **定语限高三层**：中心名词前的修饰链超过三层，拆句后置。本仓应用：「独立行为等价性、二阶涟漪效应与度量对账核验员」→「独立核验者，负责行为等价性、改动波及面与度量对账」。

## 3. 术语治理

1. **Canonical English 白名单保留**：行业公认技术术语保留英文原词，词表权威定义见 [orthogonal-refactoring §5.3](orthogonal-refactoring.md#5-语义精准化重铸与术语规范)；
2. **自造术语首用必须带一句人话释义**（内联括注或「人话」通道）：实验证据显示，术语即使附上定义，仍会降低处理流畅性与读者卷入度 [3]——所以治理靠限量而非只靠加注，新增自造术语须走 RSI 评审并挂首用释义；
3. **单段新术语 ≤ 2 个**：工作记忆按组块计费，每个首读可见的陌生术语各占一个组块 [4]；
4. **能平实说清的不造术语**：无谓的复杂词汇会让作者显得更不聪明 [5]；「低熵」「熵增负债」这类自造词，首用处必须让外行能顺出意思，必要时直接用平实表达替换（知识的诅咒是对晦涩行文的最佳单一解释 [6]）。

## 4. 双通道条目模板

规约条目同时服务两类读者：执行 Agent 需要可判定精度，人需要可读性。两条通道各自验收、互不越界：

```markdown
N. **<条目名（Canonical English Name）>**：<单句判定规则：条件 + 模态词 + 动作，不夹带出处、原理与反例>。
   > 人话：<一句因果复述：这条为什么存在、违反会发生什么；不用规约腔、不出现模态词>。
```

**防漂移三规则**（违反任一即违规，防第二事实源）：

1. 通道 A（判定规则）唯一承载可执行约束与模态词，可独立勾验；
2. 通道 B（人话）只解释动机与后果——出现 A 没有的约束即违规；
3. 首用术语在 B 通道或紧邻括注内一句话释义，此后全文直用同一名字。

`人话：` 前缀可 grep 统计：`grep -rn "人话：" SKILL.md references/`。

## 5. 改写筛选与四条禁则

**改写三问**（筛选器；三问全过的段落不动，全仓需改写处远少于全部段落）：

1. 首句是否立刻让人知道在讲什么？
2. 每句是否只做一件事？
3. 新信息是否在旧信息之后？

**四条禁则**：

1. **不软化模态词**：`严禁/必须/仅限/一律` 可拆句、可移位，不可降级为「避免/尽量/不应」；
2. **不搬 SSOT**：细节他处已定义则留指针，不复制内容；
3. **不写营销腔**：「彻底免疫/杜绝/赋能」类不可证伪的绝对化断言，改写为机制描述（写「防止盲删」，不写「杜绝盲删」）[5]；
4. **不为对账凑数**：16 词台账（见 §6）出现 LOSS 时，补位词必须落在真实判定句中，禁止无语义填充。

## 6. 改动验证

改写提交前过四查（命令以 `origin/main...HEAD` 为例）：

1. **标题零触碰**：`git diff origin/main...HEAD -- SKILL.md README.md references/ docs/ | grep -E '^[+-]#{1,6} '`，输出必须为空；
2. **16 词台账**：对 [rsi-hook §5](rsi-hook.md#5-核验门禁) G3 反刷分词表（必须/严禁/绝不/禁止/不得/不可/一律/仅限/严格/强制/铁律/硬约束/门禁/全绿/不放行/未通过）逐文件计数，任何 LOSS 须在同文件真实判定句中补位；
3. **行数预算**：`git diff --numstat origin/main...HEAD` 合计，不含本规约新增文件的净增 ≤ 60 行；
4. **外行盲评**：至少 1 个未读过本仓的全新上下文代理盲读五问（复述要求 / 不懂词数 / 违反后果 / 愿意读 1–5 / 复述判定规则），新版任一题更差则该段回炉，不得以「整体更好」豁免单段。

---

## 参考（IEEE）

[1] ISO, *Plain language — Part 1: Governing principles and guidelines*, ISO 24495-1:2023. Geneva, Switzerland: International Organization for Standardization, 2023.

[2] G. D. Gopen and J. A. Swan, "The science of scientific writing," *American Scientist*, vol. 78, no. 6, pp. 550–558, Nov.–Dec. 1990.

[3] H. C. Shulman, E. N. Dixon, N. C. Bullock, and D. M. DeAndrea, "The effects of jargon on processing fluency, self-perceptions, and scientific engagement," *Journal of Language and Social Psychology*, vol. 39, no. 5-6, pp. 579–597, 2020, doi: 10.1177/0261927X20902177.

[4] J. Sweller, J. J. G. van Merriënboer, and F. Paas, "Cognitive architecture and instructional design: 20 years later," *Educational Psychology Review*, vol. 31, no. 2, pp. 261–292, 2019, doi: 10.1007/s10648-019-09465-5.

[5] D. M. Oppenheimer, "Consequences of erudite vernacular utilized irrespective of necessity: Problems with using long words needlessly," *Applied Cognitive Psychology*, vol. 20, no. 2, pp. 139–156, 2006, doi: 10.1002/acp.1178.

[6] S. Pinker, *The Sense of Style: The Thinking Person's Guide to Writing in the 21st Century*. New York, NY, USA: Viking, 2014.

[7] Government Digital Service, "Style guide," *GOV.UK*. [Online]. Available: https://www.gov.uk/guidance/style-guide

[8] W3C, "Making content usable for people with cognitive and learning disabilities," *W3C Working Group Note*. [Online]. Available: https://www.w3.org/TR/coga-usable/
