# 包和 slot 移动

本章介绍包装和 slot 移动的用法。软件包更新机制是强大的工具，需要谨慎使用。特别要注意以下几点：

1. 更新不是一次性操作，也不是有状态的。可以将所有更新多次重新应用于同一系统，并且所有旧更新都将应用于新的 Gentoo 安装。
2. 创建更新条目后，旧的软件包名称（或 slot）将无法重复使用。尝试重用它会导致更新再次应用到重用的名称。这也意味着无法撤消更新。
3. 更新只能执行一对一的移动。它们不能用于合并软件包。尝试将两个或多个软件包合并为一个名称可能会给用户带来严重的问题。

## 移动或重命名软件包

移动或重命名软件包需要若干操作。首先，请确认 ebuild 在移动后将继续正常工作。如果类别更改，则必须验证所有`${CATEGORY}`的使用。如果软件包名称更改，则必须验证`${PN}`，`${P}`等。每当需要旧值时，请用逐字记录文本替换变量引用，以使它不受移动的影响。移动包之前，请分别提交更改。

然后，使用`git mv`移动软件包文件。以以下格式将移动条目添加到`profiles/updates/`：

```bash
move old-category/old-name new-category/new-name
```

在更新条目之后，找到对旧软件包名称的所有引用并进行更新。这些包括：

- 其他 ebuild 和 eclass 中的[依赖项](./../general-concepts/dependencies.md)，`has_version`和`best_version`使用
- 所有`profiles/` tree 条目（例如，`profiles/package.mask`）
- `restrict`软件包`metadata.xml`文件中的条目以及`<pkg/>`标记
- 新闻项目显示（如果已安装）
- 打开错误摘要

最好将所有这些更改组合到一个提交中，以确保更新过程中的原子性。

## 更改 ebuild 的 slot

更改 ebuild 的 SLOT 的过程与之前的过程非常相似。除了更改 ebuild 文件中的 SLOT 之外，还需要在 Gentoo 存储库中的`profile/updates/`中以以下格式创建新条目：

```bash
slotmove app-text/gtkspell 0 2
```

确保已解决所有反向依赖关系，并更新了`profiles/`目录中的每个文件，其中恰巧包含可能受更改影响的条目。
