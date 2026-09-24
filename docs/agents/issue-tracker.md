# Issue tracker: Local Markdown

本项目需要落盘的需求规格与实施任务使用本地 Markdown，存放在仓库内的 `.scratch/` 中。

## 文件约定

- 每项功能使用独立目录：`.scratch/<feature-slug>/`。
- 需求规格：`.scratch/<feature-slug>/spec.md`。
- 实施任务：`.scratch/<feature-slug>/issues/<NN>-<slug>.md`。
- 任务编号在各功能目录内从 `01` 开始，每个任务独立成文件。
- 讨论记录追加到任务文件末尾的 `## Comments` 下。

## 发布与读取

技能要求“发布到 issue tracker”时，按上述约定创建本地文件。

技能要求“读取相关 ticket”时，读取指定的任务文件。

编号仅在所属功能目录内唯一；只提供编号且无法确定目录时，先澄清任务归属，不猜测目标文件。

## Wayfinding 约定

使用 wayfinder 工作流时：

- 地图：`.scratch/<effort>/map.md`，记录笔记、已作出的决策和未决问题。
- 子任务：`.scratch/<effort>/issues/<NN>-<slug>.md`。
- Wayfinding 的 `<effort>` 表示本次工作的目录标识；对应已有功能时，沿用该功能的 `<feature-slug>` 目录及任务编号。
- `Type:` 记录 research、prototype、grilling 或 task。
- `Status:` 记录 open、claimed 或 resolved，新任务为 open。
- `Blocked by:` 列出同目录内的依赖任务编号；无依赖时省略。
- 所有依赖任务均为 resolved 时，任务才解除阻塞。
- 按编号选择首个 open 且未被阻塞的任务。
- 开始处理前，将状态保存为 claimed。
- 完成后，在 `## Answer` 下追加结果，将状态改为 resolved，并在 map.md 中追加简述与任务链接。
- 恢复工作时，先检查已认领任务；确认原执行者不再处理后，可继续该任务，或将其恢复为 open，并记录原因。
