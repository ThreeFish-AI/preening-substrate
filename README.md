# preening-substrate — 梳理清减

> 遵循「先全面梳理与护栏锚定，再提炼核心、正交分解，最后激进剪枝与独立对抗核验」认知演进法则，对目标模块执行结构性重构、深模块重塑与语义规范化，实现可度量系统性熵减的 Agent Skill。

**preening-substrate** 是一个纯方法论与工程规约技能（零可执行脚本、零运行时外部依赖）：以目标项目当前工程实况为事实基准，融合软件工程经典理论（Lehman 软件演化定律、Brooks 本质/偶然复杂性二分、Parnas 信息隐藏与变化轴分解、Ousterhout 深模块理论、Fowler 行为保持重构纪律与 Feathers 特征化测试接缝），将复杂模块的结构性熵减固化为 **6 阶段自治演进流水线 (6-Phase Autonomous Pipeline)**。

**核心机制创新**：
- **前置护栏与四级爆炸半径分流（Phase 0 Seam Protection & Triage）**：动刀前先判定爆炸半径（L1 单文件私有域 / L2 模块内正交重组 / L3 跨模块公共契约演进 / L4 无测试存量泥球）；遇无测试模块强制先提取软件接缝（Seam）建立 Golden Master 特征化测试护栏。
- **动态引用四维雷达（Quad-Radar）与切斯特顿围栏考古（Chesterton's Fence）**：物理删除任何「0 引用」或看似冗余的分支前，强制完成「反射/DI、序列化/ORM、配置/CLI 字符串、动态导出」四维排雷与 `git log -S` / `git blame` 历史初衷溯源，杜绝盲删隐式防御护栏。
- **双代理对抗核验（Dual-Agent Adversarial Verification）**：由主 Agent（**Refactorer**）操刀正交重塑与剪枝内联，Phase 5 出闸前派发全新只读上下文的 **Verifier Subagent**（仅读 `baseline.md`、`git diff` 与测试实况日志，不读主 Agent 自辩），独立执行四门禁盲审（Quad-Gate，Gate 1–4）：测试防篡改、公开契约零断裂、悬挂导入清零与度量数据真实性。
- **三层渐进式披露与抗压缩落盘（Progressive Disclosure & Context Resilience）**：精炼主控 [SKILL.md](SKILL.md) 配合按阶段按需加载的 `references/` 深度操作手册，并将中间测绘与审计状态实时落盘至自忽略的 `.temp/preening-<slug>-lab/`，彻底免疫长链路上下文压缩（Context Compaction）引发的后半程失忆盲改。

---

## 安装

本技能遵循 [Agent Skills 开放标准](https://agentskills.io/specification)，兼容 Claude Code、Antigravity IDE 等所有支持 `SKILL.md` 的 Agent Harness，差异仅在技能目录：

| 宿主 | 全局技能目录 | 工作区级技能目录 |
| :--- | :--- | :--- |
| Claude Code | `~/.claude/skills/` | `<repo>/.claude/skills/` |
| Antigravity IDE | `~/.gemini/config/skills/` | `<workspace>/.agents/skills/` |
| 其他遵循跨客户端约定的宿主 | `~/.agents/skills/` | `<repo>/.agents/skills/` |

```bash
# 方式一：克隆直装（外部用户，SKILL_DIR 取上表对应目录）
SKILL_DIR=~/.claude/skills   # Antigravity IDE 改为 ~/.gemini/config/skills
git clone https://github.com/ThreeFish-AI/preening-substrate.git $SKILL_DIR/preening-substrate

# 方式二：symlink（本机维护模式——改仓库即改 Skill，多宿主共享单一事实源）
git clone https://github.com/ThreeFish-AI/preening-substrate.git ~/projects/preening-substrate
ln -sfn ~/projects/preening-substrate ~/.claude/skills/preening-substrate           # Claude Code（-n 防目录自引用嵌套）
ln -sfn ~/projects/preening-substrate ~/.gemini/config/skills/preening-substrate    # Antigravity IDE
```

> 技能清单在会话开始时扫描，安装后须重启 Agent 会话方可被发现。也可用 [vercel-labs/skills](https://github.com/vercel-labs/skills) CLI 安装：`npx skills add ThreeFish-AI/preening-substrate`。
>
> 注：Antigravity 对 symlink 安装的技能存在不识别的已知问题（[vercel-labs/skills#633](https://github.com/vercel-labs/skills/issues/633)），该宿主建议改用克隆直装或 `npx skills add --copy`。

---

## 使用

### 触发方式
- **显式调用**：`/preening-substrate <目标模块或路径>`（Claude Code 与 Antigravity 通用）
- **自然语言触发**：
  - 「请全面梳理这个模块，提炼核心并进行正交分解」
  - 「梳理 / 清减这个模块」
  - 「对 X 做正交分解，把机制与业务策略拆开」
  - 「消除死代码与过度抽象，拍平透传层与浅模块」
  - 「规范化这批命名与职责边界」
  - 「执行结构性熵减」
  - 「先对 X 做一次只读梳理与熵增审计（Dry-Run），暂不改代码」

全新业务特性开发、紧急单点 Bug 热修（Hotfix）、纯代码格式化（`ruff format` / `prettier`）或跨语言推倒重写不会触发重型重构流水线；完整正负边界契约以 [SKILL.md](SKILL.md) frontmatter `description` 为准（触发评测集见 [evals/trigger-evals.json](evals/trigger-evals.json)）。

### 名字即契约
[ThreeFish-AI/negentropy](https://github.com/ThreeFish-AI/negentropy) 的 routine preset `preening_substrate` 以「使用 Skill 工具调用 `preening-substrate` 技能」按名消费本技能；技能名称、输入契约及交付报告**四大核心板块标题**保持 100% 向后兼容稳定（板块内的度量行与核验清单允许增量演进）。

**RSI 自我改进**：执行过程中由用户指出或 Agent 捕获的本 Skill 规约缺陷会旁路记录于 `.temp/preening-substrate-rsi/backlog.md`、不打断重构主流程；交付后由 Steward 与独立 Verifier 经五道门禁（G0–G4）核验并向用户一次性确认后，方以 PR 回馈本仓库（详见 [references/rsi-hook.md](references/rsi-hook.md)）。

---

## 工作流（六阶段自治演进流水线）

完整铁律、分流矩阵与阶段门禁以 [SKILL.md](SKILL.md#阶段验收与自治流转对照总表) 及其按需加载的 `references/` 为唯一事实源（SSOT）：

| 阶段 | 核心内容与权威规约 | 协同执行机制 | 门禁验收标准 (Exit Criteria) |
| :--- | :--- | :--- | :--- |
| **Phase 0** | **预检分流与特征化测试护栏**（[verification-and-delivery §1](references/verification-and-delivery.md#1-phase-0-预检分流与特征化测试护栏)）：初始化 `.temp/preening-<slug>-lab/`，登记 Git 基线 SHA 与原始 LOC；判定爆炸半径（L1–L4）；无测试模块提取接缝补齐 Golden Master 特征化测试 | Refactorer 基线采集与接缝测试构建 | `baseline.md` 落盘；分流等级确定；既有测试或新增特征化测试实跑全绿（Dry-Run 模式豁免：仅登记既有测试实况） |
| **Phase 1** | **全面梳理与拓扑测绘**（[context-and-audit §1–§2](references/context-and-audit.md#1-全面梳理资产盘点与拓扑测绘)）：盘点公开导出契约、入站/出站调用图谱、全局状态与副作用点；启动动态引用四维雷达（反射/DI、序列化、配置字符串、动态导出） | Refactorer 全局只读扫描与登记 | `topology-and-audit.md` 完备记录公开契约、调用拓扑与四维动态引用雷达表 |
| **Phase 2** | **核心提炼与熵增审计**（[context-and-audit §3–§5](references/context-and-audit.md#3-切斯特顿围栏chestertons-fence考古规程)）：萃取一句话领域本质（Brooks 二分法）；构建 Parnas 变化维度矩阵（机制 vs. 策略）；执行 Chesterton's Fence Git 考古；定量审计浅模块、透传层与死代码 | Refactorer 深度审计（Dry-Run 模式在此交付方案并安全停机） | 变化维度矩阵完备；怪异分支完成 `git log -S` / `git blame` 考古；待清理负债逐项附 `文件:行号` |
| **Phase 3** | **正交分解与深模块重塑**（[orthogonal-refactoring §0–§3](references/orthogonal-refactoring.md#0-重构步法总则双阶段帽子与两振出局回退)）：戴「结构重塑帽」，按变化轴拆解概念主体、合并连体浅类锻造深模块；解耦机制与策略；收敛 SSOT；L3 跨模块变更同步迁移调用点或加兼容垫片 | Refactorer 小步结构迁移 + 两振出局（2-Strike）回滚护栏 | 模块呈单向无环依赖（DAG）；SSOT 唯一；入站调用点或兼容垫片就绪；阶段测试全绿 |
| **Phase 4** | **代码清减与语义规范化**（[orthogonal-refactoring §4–§5](references/orthogonal-refactoring.md#4-激进剪枝与过度封装拍平)）：换戴「清减内联帽」，物理剪除死代码与废弃分支；拍平单实现接口套娃与 Pass-Through 透传函数；重铸精准领域命名并保留 Canonical English 术语 | Refactorer 外科手术清减 + 静态检查与测试实跑 | 死代码与透传层清零；模糊命名（`Manager`/`Utils` 等）全部重铸；Lint/Typecheck 与测试全绿 |
| **Phase 5** | **对抗核验与量化交付**（[verification-and-delivery §2–§4](references/verification-and-delivery.md#2-phase-5-独立-verifier-盲审与二阶效应核验)）：派发全新只读 Verifier 盲审测试等价性、二阶防震、死引用清零与度量真实性；清理 `.temp/preening-<slug>-lab/` 内临时脚本（保留自忽略的 4 份审计回执）并交付报告 | Refactorer 汇总 ↔ **Verifier Subagent** 四门禁盲审（Quad-Gate） | 四道核验门禁（Gate 1–4）全绿；输出标准化《梳理清减交付报告》 |
| **RSI（横切）** | **自我改进钩子**（[rsi-hook](references/rsi-hook.md)）：旁路捕获本 Skill 规约缺陷，交付后隔离 clone 最小修复并独立核验，经确认提 PR | Steward Subagent 改进 ↔ Verifier Subagent 核验（G0–G4） | G0–G4 全绿并经用户一次性确认后提 PR，否则仅输出报告 |

---

## 仓库结构

```text
SKILL.md                                   # 技能主控契约（Frontmatter + 重构十铁律 + 分流矩阵 + 阶段门禁总表 + 六阶段流水线 + SSOT 指针）
references/context-and-audit.md            # Phase 1–2 规约 SSOT（拓扑测绘 / 动态引用四维雷达 / Chesterton's Fence 考古 / Parnas 矩阵 / Ousterhout 浅模块审计）
references/orthogonal-refactoring.md       # Phase 3–4 规约 SSOT（双阶段帽子与两振出局回退 / 深模块重塑 / 机制与策略分离 / SSOT 与兼容垫片 / 激进剪枝与语义重铸）
references/verification-and-delivery.md    # Phase 0 & 5 规约 SSOT（Feathers 特征化测试护栏 / 独立 Verifier 四门禁盲审 / 多维熵减度量 / 标准化交付报告模板）
references/plain-expression.md             # 文本表达质感 SSOT（读者四结果 / 句构三律 / 术语治理 / 双通道条目模板 / 改写筛选与禁则 / 表达力四维 / 改动验证）
references/rsi-hook.md                     # RSI 自我改进钩子协议 SSOT（触发白名单 / 旁路捕获 / G0–G4 核验门禁 / PR 模板与降级矩阵）
evals/trigger-evals.json                   # 触发边界评测集（13 正向应触发 + 14 近邻不触发，用于校验 description 路由精度）
evals/evals.json                           # 端到端任务评测集（7 大重构场景：L1 单文件清减 / L2 正交重组 / L4 无测试遗产护栏 / L3 跨模块兼容垫片 / 动态反射排雷 / Dry-Run / 职责边界）
docs/.agents/knowledge-map.md              # 仓库文档与知识索引
docs/.agents/issue.md                      # 工程 Issue 根因与防范台账
README.md                                  # 安装、使用、六阶段方法论与仓库结构说明
CHANGELOG.md                               # Keep a Changelog 版本演进记录
LICENSE                                    # MIT
```

---

## 许可

[MIT](LICENSE) — Copyright (c) 2026 ThreeFish-AI。

## 出处与致谢

本技能自本机用户级 slash command `~/.claude/commands/preening-substrate.md`（2026-05-16 创建）迁出为独立技能，并在 v1.0.2 参照 [Agent Skills 开放标准](https://agentskills.io/specification)与同系标杆技能 [guided-learn](https://github.com/ThreeFish-AI/guided-learn) 升级为三层渐进披露与双代理对抗核验架构。
