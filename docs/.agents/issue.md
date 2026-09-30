# Issue Log

## 仓库根出现 `preening-substrate/preening-substrate/...` 无限嵌套（2026-09-30）

- **现象**：IDE 资源管理器中目录无限展开，`git status` 出现未跟踪项 `?? preening-substrate`。
- **表因**：仓库根存在自引用 symlink `preening-substrate -> <repo>`。
- **根因**：`~/.claude/skills/preening-substrate` 已是指向目录的 symlink 时，重复执行 `ln -s <repo> ~/.claude/skills/preening-substrate`，`ln` 跟随目录链接，将新链接创建在目标目录（即仓库）内部，形成环。
- **处理**：`rm <repo>/preening-substrate`（仅删链接）；README 安装命令改为 `ln -sfn`；`.gitignore` 追加 `/preening-substrate` 兜底。
- **防范**：凡安装目标可能已存在目录 symlink 的场景，一律使用 `ln -sfn`；删除 symlink 时勿带尾斜杠、勿用 `-r`。
