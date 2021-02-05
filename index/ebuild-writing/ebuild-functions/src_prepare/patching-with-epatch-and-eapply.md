# 使用 epatch 和 eapply 修复

在 ebuild 中应用补丁的规范方法是在 src_prepare 中使用 epatch（来自 epatch.eclass，必须确保继承！）。此功能可根据需要自动处理-p 级别，gunzip 等。

从 EAPI = 7 开始，该功能被禁止，必须使用 `eapply`。

从 EAPI = 6 开始，添加了一个新函数 `eapply` 来应用补丁程序，而无需 eclass。此功能与 epatch 在以下方面有所不同：

- `eapply` 不会为你解压补丁。
- 缺省补丁程序级别为-p1。必须手动指定其他补丁程序级别，否则命令将失败。
- 指定目录时，必须至少存在一个名称以.patch 或.diff 结尾的文件，否则命令将失败。其他文件将被忽略。

请注意，强烈建议相比原始压缩包和补丁更不要分发经过修改的压缩包。

## `eapply`基础

默认的 src_prepare 函数将查找全局 PATCHES 数组以为你应用补丁列表。

```bash
PATCHES=(
	"${FILESDIR}/${P}-destdir.patch"
	"${FILESDIR}/${P}-parallel_build.patch"
)
```

## `eapply`进阶

此示例显示了如何应用不同的补丁程序级别：

```bash
src_prepare() {
	eapply -p2 "${WORKDIR}/${P}-suse-update.patch"
	eapply -p0 "${FILESDIR}/${PV}-no-TIOCGDEV.patch"
	eapply "${FILESDIR}/${PV}-gcc-6.patch"
	eapply_user
}
```

## `epatch`基础

`epatch`以最简单的形式使用一个文件名并应用该补丁。如果申请失败，它将自动`die`。以下摘自`app-misc/detox`：

```bash
src_prepare() {
	epatch "${FILESDIR}/${P}-destdir.patch"
	epatch "${FILESDIR}/${P}-parallel_build.patch"
}
```

对于较大的补丁程序，使用 [你的 devspace](./../../../general-concepts/mirrors.md) 而不是 [\${FILESDIR}](./../../variables.md)更为合适。在这些情况下，通常最好用 `xz` 或 `bzip2`（与`${FILESDIR}`不得压缩的补丁程序相对）压缩有问题的补丁程序。例如，`app-admin/showconsole`示例：

```bash
src_prepare() {
	epatch "${WORKDIR}/${P}-suse-update.patch.bz2"
	epatch "${FILESDIR}/${PV}-no-TIOCGDEV.patch"
}
```

记得将补丁添加到 `SRC_URI`。

## `epatch`多个补丁

epatch 还可以从单个目录中应用多个补丁（可以有选择地基于 arch）。如果上游发行了需要更多补丁的发行版，这将很有用。

一个简单的例子：

```bash
src_prepare() {
	EPATCH_SOURCE="${WORKDIR}/patches" EPATCH_SUFFIX="patch" \
		EPATCH_FORCE="yes" epatch
}
```

这里，SRC_URI 组件之一是一个压缩包，其中包含许多带有文件扩展名的补丁.patch。

可以定义的变量包括：

| **变量**         | **目的**                                                               |
| :--------------- | :--------------------------------------------------------------------- |
| `EPATCH_SOURCE`  | 指定 epatch 在其中查找修补程序的目录。                                 |
| `EPATCH_SUFFIX`  | 修补程序的文件扩展名。                                                 |
| `EPATCH_OPTS`    | 默认选项为 `patch`。                                                   |
| `EPATCH_EXCLUDE` | 要排除的补丁列表。                                                     |
| `EPATCH_FORCE`   | 即使补丁不遵循规范的命名形式（设置为 `yes`），也可以强制补丁应用补丁。 |

批量补丁应以形式命名 `??\_${ARCH}_foo.${EPATCH_SUFFIX}`。如果不是，则 `EPATCH_FORCE="yes"`必须设置。要在 `all` 架构上应用补丁，请对`${ARCH}`部分使用全部。
