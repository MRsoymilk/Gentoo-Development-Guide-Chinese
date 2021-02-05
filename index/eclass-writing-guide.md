# Eclass 书写指南

本节简要介绍了 eclass 的书写。

<div class="alert alert-important">
<b>重要说明</b>：在继续之前，你应该重新阅读<a href="./general-concepts/overlay.md">Overlay和Eclasse</a> 和<a href="./general-concepts/the-portage-cache.md">Portage Cache</a>部分。
</div>

## Eclass 的目的

eclass 是一组可以由多个 ebuild 使用的代码。在撰写本文时，所有 eclass 都位于 tree 中的 `eclass/`目录。粗略地说，有三种 eclass：

- 为许多 ebuild 提供通用函数（例如，`eutils`，`eapi7-ver`，`cvs`，`bash-completion`）
- 为许多相似软件包提供基本构建系统（例如 `vim-plugin`，`kde`）
- 解决一个或少数软件包提供复杂编译系统（例如，`vim`，`toolchain`，`kernel-2`）

## 添加和更新 Eclass

在将新的 eclass 提交给 tree 之前，必须将理由和建议的实施通过邮件发送到 gentoo-dev 邮件列表。不要跳过此步骤——有时会产生一个更好的实现或不需要新 eclass 的替代方法。

在更新任何 eclass 之前，请将补丁程序通过邮件发送到 gentoo-dev 列表。由于你的提议可能会以一种未曾预料的方式被破坏，或者存在具有相同函数的函数，或者你的函数只在自己的 eclass 很好。如果你不首先通过邮件发送 gentoo-dev 并最终破坏了某些东西，那将导致很多麻烦。

此规则的例外是对每个软件包 eclass 的更新。例如，`apache-2`eclass 仅由 `www-servers/apache`软件包使用，因此通常不需要将更改通过邮件发送以供审核。

删除或更改 eclass 的函数和 API 时，确保它不会破坏 Gentoo 存储库中的任何 ebuild。

如果有 eclass 的现有维护者（通常是这种情况），**必须**在进行任何更改之前与维护人员联系。

<div class="alert alert-warning">
<b>警告</b>：提交损坏的 eclass 可能会杀死大量软件包。由于 <code>repoman</code> 不支持 eclass-aware，因此请务必进行正确的测试。
</div>

一种简单验证语法的方法是 `bash -n foo.eclass`。

## 删除 Eclass

不再使用的 eclass 可能会从 tree 中移除，但是开发人员必须遵循以下过程：

1. 确保 tree 中没有任何其他软件包或其他 eclass `inherit`此 eclass。
2. 向 gentoo-dev 和 gentoo-dev-announce 邮件列表发送 lastrite 消息，宣布将在 30 天内删除未使用的 eclass。
3. 在 eclass 上添加一行注释，准确地说一句，`# @DEAD` 以便 eclass-manpages 包不会尝试对其进行文档化。
4. 等待 30 天，然后从 tree 中删除 此 eclass。

## 记录 Eclass

Eclass 使用遵循特殊标记语法的注释块记录。注释块由空行分隔，每个块记录 eclass 的单个元素。

各种 eclass 元素的文档标签在下面的相应部分中说明。对于所有接受多行自由文本的标签来说，`@CODE`标签是通用的，在必要时应将`@CODE`标签放在开头和结尾来创建未格式化的代码块（例如示例代码）。

## 基本 Eclass 格式

eclass 是在 ebuild 环境中使用的`bash`脚本。文件应该是带有`.eclass` 扩展名的简单文本文件。文件名中允许的字符为小写字母，数字 0-9，连字符，下划线和点。本质上 Eclass 不具有版本。

Eclass 应该以标准 ebuild 标头开头，并附带解释 eclass 的维护者和用途的注释以及任何其他相关文档。Eclass 文档块应该是出现在 eclass 中的第一个文档块。下表总结了可用的文档标签：

| **标签**        | **可选的？** | **参数**                 | **描述**                                                         |
| :-------------- | :----------- | :----------------------- | :--------------------------------------------------------------- |
| `@ECLASS:`      | 否           | 被记录的 eclass 的名称。 | 记录有关 eclass 的各种信息。必须显示为注释块中的第一个标签。     |
| `@MAINTAINER:`  | 否           | 一个或多个名称和邮件对。 | 相应 eclass 的维护者列表。每个维护者一行。必须在标签下一行开始。 |
| `@AUTHOR:`      | 是           | 一个或多个名称和邮件对。 | eclass 的作者列表。每位作者一行。标签后另起一行开始。            |
| `@BUGREPORTS:`  | 是           | 多行自由文本。           | 应当说明关于错误的描述。                                         |
| `@VCSURL:`      | 是           | URI 字符串。             | 指向包含 eclass 源的 VCS 的 URL。                                |
| `@BLURB:`       | 否           | 单行自由文本。           | 包含有关 eclass 的简短描述。必须与标记在同一行。                 |
| `@DESCRIPTION:` | 是           | 多行自由文本。           | 有关 eclass 的详细说明。                                         |
| `@EXAMPLE:`     | 是           | 多行自由文本。           | 说明此 eclass 的各种用法示例。                                   |

## Eclass 变量

顶级变量可以定义为 ebuilds。如果使用了任何 USE 标志，则 `IUSE` 必须设置。不得在 eclass 中设置该 `KEYWORDS` 变量。

你应该记录 eclass 中每个变量的含义，用法和默认值。可用于记录 eclass 变量的标签如下：

| **标签**            | **可选的？** | **参数**                       | **描述**                                                                                                                                                    |
| :------------------ | :----------- | :----------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `@ECLASS-VARIABLE:` | 否           | 文档适用的 eclass 变量的名称。 | 此标签适用于特定于 eclass 的变量，这些变量会影响 eclass 的默认行为。eclass 导出供开发人员使用的只读变量也记录在此标记中。必须作为第一个标签出现在注释块中。 |
| `@DEFAULT_UNSET `   | 是           | ——                             | 指示如果开发人员未设置默认情况下未设置此变量。                                                                                                              |
| `@INTERNAL`         | 是           | ——                             | 指示该变量在 eclass 内部。                                                                                                                                  |
| `@REQUIRED`         | 是           | ——                             | 指示此变量必须由开发人员设置。                                                                                                                              |
| `@DESCRIPTION:`     | 否           | 多行自由文本。                 | eclass 变量的详细说明。                                                                                                                                     |

## Eclass 函数

Eclass 可以定义函数。这些函数对继承 eclass 的任何对象都是可见的。

你应该记录 eclass 中每个函数的用途，参数，用法和返回值。可用于文档的标准标签为：

| **标签**        | **可选的？** | **参数**                     | **描述**                                                                         |
| :-------------- | :----------- | :--------------------------- | :------------------------------------------------------------------------------- |
| ` @FUNCTION:`   | 否           | 文档模块所适用的函数的名称。 | 记录有关 eclass 函数的信息，例如其调用约定等。必须在注释块中作为第一个标签出现。 |
| `@USAGE:`       | 否           | 函数的必需和可选参数的列表。 | eclass 函数接受的参数列表，用以下语法指定：`<`必需参数`>` `[`可选参数`]`         |
| `@RETURN: `     | \*           | 函数的返回值。               | 函数返回值的说明。**\***：对于有返回值的函数非可选。                             |
| `@MAINTAINER: ` | 是           | 多行自由文本。               | eclass 函数的联系人列表。每个维护者一行。标签后另起一行开始。                    |
| `@INTERNAL`     | 是           | ——                           | 指示该函数在 eclass 内部，并且不应从外部调用。                                   |
| `@DESCRIPTION:` | \*           | 多行自由文本。               | eclass 函数的详细说明。**\***：如果 没有`RETURN:`，则无须选择                    |

## Eclass 函数变量

Eclass 函数可以像其他任何 bash 函数一样使用变量。特殊函数特定的变量应使用以下标记进行记录：

| **标签**          | **可选的？** | **参数**                           | **描述**                                                                   |
| :---------------- | :----------- | :--------------------------------- | :------------------------------------------------------------------------- |
| `@VARIABLE:`      | 否           | 文档适用的特定于函数的变量的名称。 | 此标记适用于 eclass 的特定函数下使用的变量。仅在调用特定函数时它们才有效。 |
| `@DEFAULT_UNSET ` | 是           | ——                                 | 指示如果开发人员未设置默认情况下未设置此变量。                             |
| `@INTERNAL`       | 是           | ——                                 | 指示该变量在 eclass 函数内部。                                             |
| `@REQUIRED`       | 是           | ——                                 | 指示此变量必须由开发人员设置。                                             |
| `@DESCRIPTION:`   | 没有         | 多行自由文本。                     | 函数变量的详细说明。                                                       |

## 简单通用函数 Eclass 示例

作为示例，编写了以下 eclass 来说明在此主题下安装 OSX 应用程序文件的更好方法。它定义了一个函数 `domacosapp`。

```bash
# Copyright 1999-2020 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2

# @ECLASS: macosapp.eclass
# @MAINTAINER:
# Ciaran McCreesh <ciaranm@gentoo.org>
# @BLURB: install macos .app files to the relevant location.

# @FUNCTION: domacosapp
# @USAGE: <app-file> [new-file]
# @DESCRIPTION:
# Install the given .app file into the appropriate location. If
# [new-file] is given, it will be used as the new (installed) name of
# the file. Otherwise <app-file> is installed as-is.
domacosapp() {
	[[ -z "${1}" ]] && die "usage: domacosapp <file> [new file]"
	if use ppc-macos ; then
		insinto /Applications
		newins "$1" "${2:-${1}}" || die "Failed to install ${1}"
	fi
}
```

## 导出函数

eclass 可以为任何标准 ebuild 函数（`src_unpack`，`pkg_postinst`等）提供默认实现。可以作为简单的函数定义（不兼容多个 eclass）或使用`EXPORT_FUNCTIONS`来完成。赋予`EXPORT_FUNCTIONS`的函数正常实现，但其名称带有`${ECLASS}_`前缀。

<div class="alert alert-note">
<b>重要提示</b>：中 仅应指定“标准”函数 <code><pre>EXPORT_FUNCTIONS</pre></code>。
</div>

<div class="alert alert-note">
<b>注意</b>：<code><pre>EXPORT_FUNCTIONS</pre></code> 是函数，而不是变量。
</div>

如果多个 eclass 导出相同的函数，则以最新（继承的最后一个）定义的版本为准。如果 ebuild 定义了要导出的函数，则它的优先级高于任何 eclass 版本。这可以用于覆盖 eclass 定义的默认值——例如，我们有 `fnord.eclass`：

```bash
EXPORT_FUNCTIONS src_compile

fnord_src_compile() {
	do_stuff || die
}
```

然后 ebuild 可以执行以下操作：

```bash
inherit fnord.eclass

src_compile() {
	do_pre_stuff || die
	fnord_src_compile
	do_post_stuff || die
}
```

## 简单构建系统 Eclass 示例

一个简单的 eclass，它定义了（假设的）`jmake`构建系统的许多函数和默认的`src_compile`，可能类似于以下内容：

```bash
# Copyright 1999-2020 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2

# @ECLASS: jmake.eclass
# @MAINTAINER:
# Gentoo Devmanual Project <devmanual@gentoo.org>
# @AUTHOR:
# Ciaran McCreesh <ciaranm@gentoo.org>
# @BLURB: Demonstrate a simple build system eclass.
# @DESCRIPTION:
# Demonstrates EXPORT_FUNCTIONS and defines simple wrappers for the
# (hypothetical) jmake build system and a default src_compile.

EXPORT_FUNCTIONS src_compile

DEPEND=">=sys-devel/jmake-2"

# @FUNCTION: jmake-configure
# @USAGE: [additional-args]
# @DESCRIPTION:
# Passes all arguments through to the appropriate "jmake configure"
# command.
jmake-configure() {
	jmake configure --prefix=/usr "$@"
}

# @FUNCTION: jmake-build
# @USAGE: [additional-args]
# @DESCRIPTION:
# First builds all dependencies, and then passes through its arguments
# to the appropriate "jmake build" command.
jmake-build() {
	jmake dep && jmake build "$@"
}

# @FUNCTION: jmake-src_compile
# @DESCRIPTION:
# Calls jmake-configure() and jmake-build() to compile a jmake project.
jmake_src_compile() {
	jmake-configure || die "configure failed"
	jmake-build || die "build failed"
}
```

## 处理 eclass 的错误用法

有时 ebuild 会错误地使用 eclass，并且 eclass 知道其使用不正确 —— 常见的示例是仅适用于一组特定 EAPI 的 eclass，但是 eclass 继承了具有不同 EAPI 的 eclass。在这些情况下，作为最后的手段而很少使用，这将允许 eclass 从全局范围调用 die。例如：

```bash
# Copyright 1999-2020 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2

# @ECLASS: eapi-die.eclass
# @MAINTAINER:
# Gentoo Devmanual Project <devmanual@gentoo.org>
# @BLURB: Calls die when used with an invalid EAPI.

case ${EAPI:-0} in
	0) die "this eclass doesn't support EAPI 0" ;;
	*) ;;
esac
```
