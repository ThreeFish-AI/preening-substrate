# preening-substrate — 梳理清减

> 遵循「先全面梳理，再提炼核心、正交分解」认知演进法则，对目标模块执行结构性重构、代码清减与语义规范化，实现系统性熵减的 Agent Skill。

**preening-substrate** 是一个纯方法论技能（无脚本、无运行时依赖）：以项目当前工程现状为事实基准，参考业界经典设计模式与最佳实践，驱动模块的演进式重构。技能严格遵循认知心法，将重构作业标准化为 **5 阶段结构性熵减流水线 (5-Phase Execution Pipeline)**：

1. **Phase 1: 全面梳理 (Exhaustive Context Mapping)**：资产与入口全景盘点、上下游拓扑链路追踪、隐式副作用排查、测试基线固化；
2. **Phase 2: 提炼核心 (Core Distillation & Entropy Audit)**：剥离偶然复杂性、萃取领域本质、识别独立变化维度（机制 vs. 策略）、审计过度抽象与冗余；
3. **Phase 3: 正交分解 (Orthogonal Decomposition)**：概念主体正交化、机制与策略解耦、子路径物理隔离、收敛单一事实源 (SSOT)；
4. **Phase 4: 代码清减与语义规范化 (Pruning & Semantic Normalization)**：激进剪除死代码、拍平过度包装、精准语义对齐、保留标准技术术语 (Canonical English)；
5. **Phase 5: 闭环验证与量化交付 (Verification & Quantified Delivery)**：行为等价自证、二阶效应审查、输出标准化熵减量化度量报告。

并以「不破坏现有功能特性、不引入新缺陷、严禁盲目重构、遵循工程行为准则」为硬约束。

---

## 安装

兼容任何支持 SKILL.md 标准的 Agent Harness（Claude Code / Antigravity 等），差异仅在技能目录（`~/.claude/skills/` / `~/.gemini/config/skills/`）。

```bash
# 方式一：克隆直装（外部用户，以 Claude Code 为例）
git clone https://github.com/ThreeFish-AI/preening-substrate.git ~/.claude/skills/preening-substrate

# 方式二：symlink（维护模式——改仓库即改 Skill，单一事实源）
git clone https://github.com/ThreeFish-AI/preening-substrate.git ~/projects/preening-substrate
ln -s ~/projects/preening-substrate ~/.claude/skills/preening-substrate
```

> 技能清单在会话开始时扫描，安装后须重启 Agent 会话方可被发现。也可用 [vercel-labs/skills](https://github.com/vercel-labs/skills) CLI 安装：`npx skills add ThreeFish-AI/preening-substrate`。

---

## 使用

### 触发方式
- **显式调用**：`/preening-substrate <目标模块>`
- **自然语言触发**：
  - 「请全面梳理这个模块，提炼核心并进行正交分解」
  - 「梳理 / 清减这个模块」
  - 「对 X 做正交分解」
  - 「消除死代码与过度抽象」
  - 「规范化这批命名与职责边界」
  - 「执行结构性熵减」

完整执行规范见 [SKILL.md](SKILL.md)（唯一事实源）。

### 名字即契约
[ThreeFish-AI/negentropy](https://github.com/ThreeFish-AI/negentropy) 的 routine preset `preening_substrate` 以「使用 Skill 工具调用 preening-substrate 技能」按名消费本技能；技能名称、输入契约保持绝对兼容稳定。

---

## 交付规范

每次执行后均会输出包含以下维度的标准化交付报告：
- **全面梳理与核心提炼摘要**：本质领域模型、正交变化维度、清理的负债清单；
- **结构性演进对比**：重构前 As-is 结构与重构后 To-be 正交结构对比；
- **熵减量化度量 (Entropy Metrics)**：代码行数 (LOC) 变化、文件数变化、优化率统计；
- **验证自证结论**：测试通过证明、行为等价性自证、二阶效应影响确认。

---

## 仓库结构

```
SKILL.md       # 技能唯一事实源（frontmatter + 5 阶段执行规范与模板）
README.md      # 安装、使用与方法论说明
LICENSE        # MIT
CHANGELOG.md   # Keep a Changelog
```

---

## 许可

[MIT](LICENSE) — Copyright (c) 2026 ThreeFish-AI。

## 出处与致谢

本技能自本机用户级 slash command `~/.claude/commands/preening-substrate.md`（2026-05-16 创建）迁出为独立技能；外置形态沿用同一作者的 [to-video](https://github.com/ThreeFish-AI/to-video) 与 [guided-learn](https://github.com/ThreeFish-AI/guided-learn) 先例。
