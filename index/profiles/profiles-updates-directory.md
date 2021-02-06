# 个人文件 `updates/` 目录

`updates/`目录中的文件按季度进行组织，并命名为`<quarter>Q-<year>`，例如 `1Q-2010` （第一季度）。

这些文件是简单的文本文件，其中每一行包含以下命令之一。请注意，这些仅会使软件包管理器调整其元数据，对软件包的实际更改必须手动完成。

| **命令**                           | **描述**                                             |
| :--------------------------------- | :--------------------------------------------------- |
| `move oldcat/oldpkg newcat/newpkg` | 表示软件包已被重命名或已移至另一个类别，或两者都有。 |
| `slotmove spec oldslot newslot`    | 表示软件包匹配依赖项规范 `spec` 已更改 slot。        |