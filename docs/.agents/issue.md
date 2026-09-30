# Issue Log

## 仓库根出现 `preening-substrate/preening-substrate/...` 无限嵌套（2026-09-30）

- **现象**：IDE 资源管理器中目录无限展开，`git status` 出现未跟踪项 `?? preening-substrate`。
- **表因**：仓库根存在自引用 symlink `preening-substrate -> <repo>`。
- **根因**：`~/.claude/skills/preening-substrate` 已是指向目录的 symlink 时，重复执行 `ln -s <repo> ~/.claude/skills/preening-substrate`，`ln` 跟随目录链接，将新链接创建在目标目录（即仓库）内部，形成环。
- **处理**：`rm <repo>/preening-substrate`（仅删链接）；README 安装命令改为 `ln -sfn`；`.gitignore` 追加 `/preening-substrate` 兜底。
- **防范**：凡安装目标可能已存在目录 symlink 的场景，一律使用 `ln -sfn`；删除 symlink 时勿带尾斜杠、勿用 `-r`。

## v1.0.2 入库前深度 Review 揪出三处规约自相矛盾（2026-09-30）

- **现象**：六维对抗审查（78 个审查/核验 Agent）确认 50 条问题，其中三处为「逻辑 Bug 级」规约矛盾。
- **表因**：① Dry-Run 承诺「只读」但 Phase 0 无条件要求补写特征化测试文件；② Verifier 输入契约 `git diff <baseline_sha>..HEAD` 隐含「变更已提交」，而 checkpoint 为可选、`/commit` 排在盲审之后，未提交项目盲审拿到空 diff；③ progress.md 落盘条件在 SKILL.md（TodoWrite 分支互斥）与 references（无条件落盘）间矛盾。另有 Fowler Two Hats 错归指（被重铸为 Phase 3/4 分离语义，原义为重构 vs 添加功能）与回退安全缺口（无脏工作区预检、绿灯锚点未定义）。
- **根因**：三层渐进披露拆分后，同一机制的定义分散在 SKILL.md 与多份 references，缺少跨文档口径对账；`..HEAD` 等隐含前提在「可选 checkpoint」设计下失效（git 实证：未提交变更对 commit 间 diff 不可见）。
- **处理**：Dry-Run 显式豁免（Phase 0 仅登记实况）；diff 契约改 `git diff <baseline_sha>` 对工作区取全量 + `git add -N` 覆盖 untracked；progress.md 双写；帽子隐喻改名「双阶段帽子（Structural/Pruning Hats）」并标注与 Fowler 原义的适配关系；Phase 0 增脏工作区预检、绿灯微步强制 checkpoint。
- **防范**：跨文档规约变更必须做全仓口径对账（本仓库后续以对抗审查工作流承接）；涉及 git 命令契约的规约，须先在临时仓库实证其隐含前提（如「已提交」假设）再落笔；术语引入权威归指（Two Hats 等）须核对原典语义。
- **同类影响**：另修复 allowed-tools 空格分隔（对齐 Agent Skills 开放标准）、RSI 注入面与 commit 元数据脱敏、评测集 L1/TS 覆盖缺口等 44 项 P2 级打磨，详见本次提交 diff。
