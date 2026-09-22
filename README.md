# preening-substrate — 梳理清减

> 对目标模块执行正交分解、语义规范化与代码清减，实现结构性熵减的 agent skill。

**preening-substrate** 是一个纯方法论技能（无脚本、无运行时依赖）：对目标模块进行全面扫描与深度分析，以项目当前工程现状为事实基准，参考业界经典设计模式与最佳实践，执行结构性重构与代码清减。核心动作三件套——**正交分解**（职责单一、边界清晰、子路径物理隔离）、**代码清减**（死代码 / 冗余逻辑 / 过度抽象，交付量化汇报行数与减少率）、**语义规范化**（命名精确反映业务语义）——并以「不破坏现有功能、不引入新缺陷、遵循工程行为准则」为硬约束。

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

## 使用

显式调用：`/preening-substrate <目标模块>`；或自然语言触发——「梳理 / 清减这个模块」「对 X 做正交分解」「消除死代码与过度抽象」「规范化这批命名」「结构性熵减」。完整执行规范见 [SKILL.md](SKILL.md)（唯一事实源）：正交分解 → 代码清减（量化汇报）→ 语义规范化，全程受约束节管束。

**名字即契约**：[ThreeFish-AI/negentropy](https://github.com/ThreeFish-AI/negentropy) 的 routine preset `preening_substrate` 以「使用 Skill 工具调用 preening-substrate 技能」按名消费本技能；如需更名，必须同步消费方（preset 种子、测试与文档）。

## 仓库结构

```
SKILL.md       # 技能唯一事实源（frontmatter + 执行规范正文）
README.md      # 安装与使用说明
LICENSE        # MIT
CHANGELOG.md   # Keep a Changelog
```

## 许可

[MIT](LICENSE) — Copyright (c) 2026 ThreeFish-AI。

## 出处与致谢

本技能自本机用户级 slash command `~/.claude/commands/preening-substrate.md`（2026-05-16 创建）迁出为独立技能，正文零改动；外置形态沿用同一作者的 [to-video](https://github.com/ThreeFish-AI/to-video) 与 [guided-learn](https://github.com/ThreeFish-AI/guided-learn) 先例。
