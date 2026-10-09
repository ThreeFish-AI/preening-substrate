# 文本表达质感规约（Plain Expression）

> 本文是本 Skill 文本表达质感的唯一事实源（SSOT）：读者四结果、句构三律、术语治理、双通道条目模板、改写筛选与禁则、表达力四维、改动验证均以此为准；总控流转见 [SKILL.md](../SKILL.md#引用与指针索引-single-source-of-truth)。

**目录**：[0. 定位、适用范围与冻结面](#0-定位适用范围与冻结面) · [1. 读者四结果](#1-读者四结果) · [2. 句构三律](#2-句构三律) · [3. 术语治理](#3-术语治理) · [4. 双通道条目模板](#4-双通道条目模板) · [5. 改写筛选与四条禁则](#5-改写筛选与四条禁则) · [6. 改动验证](#6-改动验证) · [7. 表达力四维](#7-表达力四维) · [参考（IEEE）](#参考ieee)

---

## 0. 定位、适用范围与冻结面

本规约治理以下人类可读文本：[SKILL.md](../SKILL.md)、`references/` 全部手册、[README](../README.md)、交付报告叙述区（Agent 按模板填写的说明性文字，模板本身整块冻结）与 [RSI PR 正文](rsi-hook.md#9-pr-模板)。表达合格的标准始终是读者结果，不是文风偏好，分两条线：**下限**是读者拿到需要的、找得到需要的、首读就懂、拿来能用 [1]；**上限**是读者读完带得走画面、说得出好在哪（§7）。两线同验，缺一即不合格。

**冻结面清单**（承载既有契约，改写一律绕行；冻结面内的历史措辞不受本规约追溯）：

1. 全部标题行（`#` – `######`）：跨文件锚点与目录锚的字节级依赖；
2. 一切既有 fenced code block：命令、脚本、路径约定、报告模板与 mermaid 图；
3. [evals.json](../evals/evals.json) 引用的契约字符串（`.temp/preening-<slug>-lab`、`baseline.md`、`topology-and-audit.md`、`verification-receipt.md`、`progress.md`、《梳理清减交付报告》等）：正文中只能原样出现；
4. [交付报告模板](verification-and-delivery.md#4-标准化梳理清减交付报告模板)整块：`negentropy` 消费四大板块标题；
5. [分流矩阵](../SKILL.md#输入分流与爆炸半径矩阵) L1–L4 行标签与「判定特征」列：评测语义引用；
6. frontmatter 触发契约字段与全部「」触发短语。

## 1. 读者四结果

每段文本交付前过四问（依据 ISO 24495-1:2023 [1]，各操作化为本 Skill 可勾验判据）：

| 结果 | 问题 | 可勾验判据 |
| :--- | :--- | :--- |
| **Relevant（相关）** | 读者需要的是不是这段话？ | 删掉后任务执行不受影响的句子，删 |
| **Findable（可找到）** | 需要时找得到吗？ | 标题写答案、关键词前置、指针表可导航 |
| **Understandable（首读即懂）** | 外行首读能否复述这条规则要求什么？ | 句构三律（§2）+ 术语治理（§3） |
| **Usable（可用）** | 读者能直接照做吗？ | 判据、命令、锚点齐全，无「按老规矩办」式引用 |

## 2. 句构三律

1. **一句一事**：每句只做一个断言；40 字为参考值而非硬上限（规约定义句豁免；GOV.UK 对超过 25 词的英文句同样要求复查能否拆开 [2]）。命中即拆句。禁令被长句淹没时，拆出来的短句反而更有力——「无基线护栏，严禁进入结构变更」单列成句即是此类。
2. **旧先新后**：句首主语位放读者已知的信息，句末压力位放希望读者带走的新信息 [3]。本 Skill 应用：开篇先给 payoff（「外部行为严格等价」），手段与出处后置；出处声明不夹在操作命令中间。
3. **定语限高三层**：中心名词前的修饰链超过三层，拆句后置。本 Skill 应用：「独立行为等价性、二阶涟漪效应与度量对账核验员」→「独立核验者，负责行为等价性、二阶涟漪效应与度量对账」。

## 3. 术语治理

1. **Canonical English 白名单保留**：行业公认技术术语保留英文原词，判定原则与示例词见 [orthogonal-refactoring §5 第 3 条](orthogonal-refactoring.md#5-语义精准化重铸与术语规范)；
2. **自造术语首用必须带一句人话释义**（内联括注或「人话」通道）：实验证据显示，术语即使附上定义，仍会降低处理流畅性与读者卷入度 [4]——所以治理靠限量而非只靠加注，新增自造术语须在首用处挂释义，并在提交说明中单列，留给评审者把关；
3. **单段新术语 ≤ 2 个**：工作记忆按组块计费，每个首读可见的陌生术语各占一个组块 [5]；
4. **能平实说清的不造术语**：无谓的复杂词汇会让作者显得更不聪明 [6]；W3C COGA 的 Use Clear Words 模式同样要求不自造新词，生僻术语须删去或解释 [7]；「低熵」「熵增负债」这类自造词，首用处必须让外行能顺出意思，必要时直接用平实表达替换（知识的诅咒是对晦涩行文的最佳单一解释 [8]）。

## 4. 双通道条目模板

规约条目同时服务两类读者：执行 Agent 需要可判定精度，人需要可读性。两条通道各自验收、互不越界：

```markdown
N. **<条目名（Canonical English Name）>**：<判定规则：条件 + 模态词 + 动作，每句只做一事，可拆为数个短句；不夹带出处论证、原理与反例，末尾可挂「见 §」指针>。
   > 人话：<因果复述，≤2 个短句：这条为什么存在、违反会发生什么；不用规约腔、不出现模态词>。
```

**防漂移三规则**（违反任一即违规，防第二事实源）：

1. 通道 A（判定规则）唯一承载可执行约束与模态词，可独立勾验；
2. 通道 B（人话）只解释动机与后果——出现 A 没有的约束即违规；
3. 首用术语在 B 通道或紧邻括注内一句话释义，此后全文直用同一名字。

`人话：` 前缀可 grep 统计：`grep -rn "人话：" SKILL.md references/ --exclude=plain-expression.md`。

## 5. 改写筛选与四条禁则

**改写三问**（筛选器；三问全过的段落不动，全仓需改写处远少于全部段落）：

1. 首句是否立刻让人知道在讲什么？
2. 每句是否只做一件事？
3. 新信息是否在旧信息之后？

**四条禁则**：

1. **不软化模态词**：`严禁/必须/仅限/一律` 可拆句、可移位，不可降级为「避免/尽量/不应」；
2. **不搬 SSOT**：细节他处已定义则留指针，不复制内容；
3. **不写营销腔**：「彻底免疫/杜绝/赋能」类不可证伪的绝对化断言，改写为机制描述（写「防止盲删」，不写「杜绝盲删」）；禁的是断言不可证伪，不是禁修辞——可指认的具象、类比与节奏手法（§7）结论仍可证伪，与营销腔不冲突（「体温计放冰水里『退烧』不算治病」不是营销腔）；
4. **不为对账凑数**：16 词台账（见 §6）出现 LOSS 时，补位词必须落在真实判定句中，禁止无语义填充。

## 6. 改动验证

改写提交前过四查。命令以工作区对 merge-base 的差异为准（`B=$(git merge-base origin/main HEAD)`，含未提交改动；新增文件先 `git add -N`）：

1. **标题零触碰**：`git diff $B --diff-filter=M -- SKILL.md README.md references/ docs/ | grep -E '^[+-]#{1,6} '`，输出必须为空（新增文件的标题不计）；
2. **16 词台账**：对 [rsi-hook §5](rsi-hook.md#5-核验门禁) G3 反刷分脚本所列词表逐文件计数（脚本原式以 `origin/main...<分支>` 取差，不含未提交改动；对工作区核对时，把脚本里 `D=` 与 `EMPTY DIFF` 判定行的取差，换成以本节 `$B` 对单个文件取差，即 `git diff -U0 $B -- <file>` 与 `git diff --numstat $B -- <file>`，词表循环不变），任何 LOSS 须在同文件真实判定句中补位；
3. **行数预算**：`git diff --numstat $B -- . ':(exclude)references/plain-expression.md' | awk '{a+=$1;d+=$2} END {print a-d}'`，净增 ≤ 60 行（不含本规约新增文件）；
4. **外行盲评**：至少 1 个未读过本 Skill 的全新上下文代理盲读七问，先读后问（问题在代理读毕后出示，防预埋好句）：① 复述要求；② 不懂词数；③ 违反后果；④ 愿意读 1–5；⑤ 复述判定规则；⑥ 劲句指认——引出最有劲的一句原话，并指认它用的是 §7 哪一手（具象 / 类比 / 节奏），引文须 grep 原文命中；⑦ 画面复述——凭记忆复述至少一个画面或类比，复述须含至少一处与原文不同而语义等价的措辞。①–⑤ 为对比判据：新版任一题更差则该段回炉，不得以「整体更好」豁免单段；⑥–⑦ 为达标判据（首轮改写无旧版基线，不走对比）：说不出手法或复述不出画面即回炉，空泛称赞（「写得很好」）不计为通过。

---

## 7. 表达力四维

可懂性保下限，表达力拔上限。写作是向读者展示眼前之物，不是把作者的私人联想倒给读者——作者中心的写法顺着作者自己的脉络走，读者中心的写法替读者重建脉络（writer-based / reader-based prose）[12]。以下四维逐段过查，判据均为可指认的文本事实：

| 维度 | 一句话原理 | 可勾验判据 | 引证 |
| :--- | :--- | :--- | :--- |
| **具象优先** | 具象词同时走语言与心理意象两条编码通道，比抽象词更易懂、更易记 [9] | 叙述区每个关键主张至少挂一个可指认的实例或场景；连续 3 句无实例即改写；「性能差」须落到「接口 200 ms → 2 s」级事实 | [9] |
| **关系结构类比** | 好类比传递的是关系结构，不是表面相似 [10] | 每个类比能指出至少两条逐项对应；对应不上的表面相似（「代码像人会累」）删；类比不得引入 A 通道没有的新约束（同 §4 防漂移规则 2）。范例：「好模块像充电宝——插口就两个，里面电路再复杂也不归你管」（两插口↔窄接口，内置电路↔深实现） | [10] |
| **节奏与压力位** | 长短句交替生顿挫，句末压力位放最重的话 [3][8]（ch. 2） | 连续 4 句长度相近（±10%）即拆一合并一；关键结论落句末，不埋句中插入语。范例：「无基线护栏，严禁进入结构变更」单列成句 | [3][8] |
| **AI 腔防治** | 「delve / intricate / tapestry」类偏好词是统计性收敛的机器指纹 [11] | 交付文本对 [11] 实证英文词表，及本 Skill 观察的中文对应词（「赋能 / 抓手 / 深耕 / 值得注意的是 / 不难发现」）grep 零命中；同段出现两组以上三连排比即改写 | [11] |

**与 §3 的分工**：术语治理管「词不准」（保留 Canonical English 白名单），AI 腔防治管「腔不对」（机器指纹词）；两份词表不相交——白名单技术术语不进 AI 腔词表。

**与 §5 禁则 3 的衔接**：力量感的判据是可指认，不是可称赞——读者指不出手法的「高级感」按营销腔处理。

---

## 参考（IEEE）

[1] ISO, *Plain language — Part 1: Governing principles and guidelines*, ISO 24495-1:2023. Geneva, Switzerland: International Organization for Standardization, 2023.

[2] Government Digital Service, "A to Z style guide," *GOV.UK content and publishing guidance*. [Online]. Available: https://guidance.publishing.service.gov.uk/writing-to-gov-uk-standards/style-guides/a-to-z-style-guide/

[3] G. D. Gopen and J. A. Swan, "The science of scientific writing," *American Scientist*, vol. 78, no. 6, pp. 550–558, Nov.–Dec. 1990.

[4] H. C. Shulman, G. N. Dixon, O. M. Bullock, and D. Colón Amill, "The effects of jargon on processing fluency, self-perceptions, and scientific engagement," *Journal of Language and Social Psychology*, vol. 39, no. 5-6, pp. 579–597, 2020, doi: 10.1177/0261927X20902177.

[5] J. Sweller, J. J. G. van Merriënboer, and F. Paas, "Cognitive architecture and instructional design: 20 years later," *Educational Psychology Review*, vol. 31, no. 2, pp. 261–292, 2019, doi: 10.1007/s10648-019-09465-5.

[6] D. M. Oppenheimer, "Consequences of erudite vernacular utilized irrespective of necessity: Problems with using long words needlessly," *Applied Cognitive Psychology*, vol. 20, no. 2, pp. 139–156, 2006, doi: 10.1002/acp.1178.

[7] W3C, "Making content usable for people with cognitive and learning disabilities," *W3C Working Group Note*. [Online]. Available: https://www.w3.org/TR/coga-usable/

[8] S. Pinker, *The Sense of Style: The Thinking Person's Guide to Writing in the 21st Century*. New York, NY, USA: Viking, 2014.

[9] M. Sadoski and A. Paivio, *Imagery and Text: A Dual Coding Theory of Reading and Writing*, 2nd ed. New York, NY, USA: Routledge, 2013, doi: 10.4324/9780203801932.

[10] D. Gentner, "Structure-mapping: A theoretical framework for analogy," *Cognitive Science*, vol. 7, no. 2, pp. 155–170, Apr.–Jun. 1983, doi: 10.1207/s15516709cog0702_3.

[11] D. Kobak, R. González-Márquez, E.-Á. Horvát, and J. Lause, "Delving into LLM-assisted writing in biomedical publications through excess vocabulary," *Science Advances*, vol. 11, no. 27, Jul. 2025, doi: 10.1126/sciadv.adt3813.

[12] L. S. Flower, "Writer-based prose: A cognitive basis for problems in writing," *College English*, vol. 41, no. 1, pp. 19–37, Sep. 1979.
