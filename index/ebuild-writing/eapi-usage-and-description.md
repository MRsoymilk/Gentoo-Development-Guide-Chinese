# EAPI 用法和说明

[软件包管理器规范（PMS）](https://wiki.gentoo.org/wiki/Project:PMS)是一项标准化工作，旨在确保正确记录 ebuild 文件格式，ebuild 存储库格式（Gentoo 存储库是 Gentoo 的主要体现）以及与这些 ebuild 交互的软件包管理器的行为并同意。

EAPI 是在 ebuilds 和其他与软件包管理器相关的文件中定义的版本，该版本将文件语法和内容通知给软件包管理器。实际上，它是文件所遵循的软件包管理器规范（PMS）的版本。

本节提供了不同 EAPI 的用法和说明。

## EAPI 的用法

<div class="alert alert-important">
<b>重要说明</b>：包管理器规范的附录中提供了有关每个EAPI重要功能的概述。可以打印出两页参考，并且可以在主tree中以<code><pre>app-doc/pms</pre></code>的形式获得。
</div>

```bash
# Copyright 1999-2020 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2

EAPI=7
```

<div class="alert alert-note">
<b>注意</b>：大多数开发人员更喜欢将EAPI版本设置为不带引号。但是，PMS也允许单引号和双引号。
</div>

<div class="alert alert-important">
<b>重要说明</b>：EAPI必须仅在ebuild文件中定义，而不能在eclass中定义。 （eclass可能具有EAPI条件代码）
</div>

在编写新的 ebuild 时，开发人员可以选择他们认为最佳的 EAPI。鼓励使用最新的 EAPI 的功能。

## EAPI 0 到 4

EAPI 0 到 4 已过时，不能再使用。有关它们的详细信息，请参阅“软件包管理器规范”。

## EAPI 5

### EAPI 5 元数据

**REQUIRED_USE 支持最多一个新的运算符**

- 新的**最多一个**运算符由字符串`??`组成，并且如果匹配零个或一个（但不多于）其子元素，则满足该条件。

**slot 支持可选的“子 slot”部分**

- `SLOT`变量可能包含一个可选的**子 slot**部分，该部分位于常规 slot 之后，并以`/`字符分隔。子 slot 必须是有效的 slot 名称。子 slot 用于表示以下情况：升级到具有不同子 slot 的软件包的新版本可能需要重建相关的软件包。如果从 SLOT 定义中省略了子 slot 部分，则认为该软件包具有一个等于常规 slot 的隐式子 slot。

**slot 操作符和子 slot 之间的依赖关系**

- slot 相关性可能包含一个可选的子 slot 部分，该部分位于常规 slot 之后，并以`/`字符分隔。这对于安装预编译二进制文件的软件包很有用，这些软件包需要具有特定 soname 版本（对应于子 slot）的库。例如：

```bash
RDEPEND="dev-libs/foo:0/3"
```

- 包依赖项规范可以使用**slot 操作符**来阐明如果运行时依赖项的 slot 和/或子 slot 发生更改应如何处理：

  - `*`表示任何 slot 值都是可接受的。另外，对于运行时依赖项，指示如果将与依赖项匹配的包替换为具有不同 slot 和/或子 slot 的匹配包，则指定依赖项的包不会中断。
  - `=`表示可接受任何 slot 值。另外，对于运行时依赖项，指示除非存在可用的与该依赖项匹配的包，并且其 slot 和子 slot 等于与此版本匹配的最佳安装版本的 slot 和子 slot，否则指定依赖项的软件包将中断。安装指定此依赖项的软件包时的依赖项。
  - `slot=`表示只能接受特定的 slot 值，否则其行为与`:=`运算符相同。

<div class="alert alert-note">
<b>注意</b>： 使用不带<code><pre>=</pre></code>的<code><pre>:slot/subslot</pre></code>取决于特定的slot和子slot对；在ebuild中使用<code><pre>:slot/subslot=</pre></code>是语法错误。
</div>

- `:slot`依赖项语法继续像在 EAPI = 4 或更早版本中一样表现，即表明仅特定的时隙值是可接受的，但是当将匹配运行时依赖项的版本替换为具有不同子项的版本时，软件包不会中断-slot。

- 例如：

```bash
RDEPEND="dev-libs/foo:2=
    >=dev-libs/bar-0.9:=
    media-gfx/baz:*
    x11-misc/wombat:0"
```

- 意味着在将`foo:2`或`>=bar-0.9`升级到具有不同子 slot 的版本时，应重新构建该软件包，但是应忽略`baz`或`wombat:0`的子 slot 中的更改。

### EAPI 5 配置文件

**配置文件稳定的 USE 强制和屏蔽**

在与 EAPI 支持稳定掩蔽配置文件目录，新的 USE 配置文件的支持：`use.stable.mask`， `use.stable.force`，`package.use.stable.mask` 和 `package.use.stable.force`。这些文件的行为与以前支持的 USE 配置文件类似，除了它们只影响由于稳定关键字而合并的软件包。

### EAPI 5 帮手

**econf 添加了--disable-silent-rules**

如果在`configure --help`的输出中出现`--disable-silent-rules`，则将自动传递此选项。

**new\*命令可以从标准输入中读取**

当第一个参数为`-`（连字符）时，将读取标准输入。

**{has，best} \_version 的新选项--host-root**

此选项`--host-root`将使查询应用于主机根而不是 ROOT。

**新的 doheader 助手函数**

将给定的头文件安装到`/ usr / include /`中。如果指定了选项-r，则递归地进入给定的任何目录。

**新的 usex 助手函数**

```bash
USAGE: usex <USE flag> [true output] [false output] [true suffix] [false suffix]
DESCRIPTION:
If USE flag is set, echo [true output][true suffix] (defaults to "yes"),
 otherwise echo [false output][false suffix] (defaults to "no").
```

### EAPI 5 阶段

**src_test 支持并行测试**

与早期的 EAPI 不同，默认 `src_test` 实现不会将-j1 选项传递给 emake。

### EAPI 5 变量

**EBUILD_PHASE_FUNC**

在执行 ebuild 阶段函数（例如 `pkg_setup` 或 `src_unpack`）期间，`EBUILD_PHASE_FUNC` 变量将包含当前正在执行的阶段函数的名称。

## EAPI 6

### EAPI 6 Bash 版本

**bash 版本**

Ebuild 可以使用 Bash 4.2 版（以前是 3.2 版）的函数。

### EAPI 6 Ebuild 环境

**区域设置**

只要涉及 ASCII 范围内的字符，就可以保证大小写修改和排序规则（`LC_CTYPE`和`LC_COLLATE`）的行为与 C 语言环境相同。

**`failglob` 已启用**

对于`EAPI=6`，bash 的`failglob`选项在 ebuild 的全局范围内设置。如果设置，在扩展文件名期间失败的模式匹配将导致在生成 ebuild 时出错。

## EAPI 6 阶段

**更新`src_prepare`的默认实现**

此阶段不再是无操作，它支持通过`PATCHES`变量应用补丁和通过`eapply_user`应用用户补丁。默认的`src_prepare`如下所示：

```bash
src_prepare() {
    if [[ $(declare -p PATCHES 2>/dev/null) == "declare -a"* ]]; then
        [[ -n ${PATCHES[@]} ]] && eapply "${PATCHES[@]}"
    else
        [[ -n ${PATCHES} ]] && eapply ${PATCHES}
    fi
    eapply_user
}
```

**新的 `src_install` 相函数**

此阶段使用新 `einstalldocs` 函数来安装文档。默认 `src_install` 看起来像这样：

```bash
src_install() {
    if [[ -f Makefile ]] || [[ -f GNUmakefile ]] || [[ -f makefile ]]; then
        emake DESTDIR="${D}" install
    fi
    einstalldocs
}
```

### EAPI 6 助手

**`einstall` 被禁止**

`einstall` 助手已被`EAPI=6`禁止使用。

**`dohtml` 不推荐使用**

`dohtml` 助手已被 `EAPI=6` 弃用。

**`nonfatal die`**

当使用`nonfatal`命令并使用`-n`选项调用`die`或`assert`时，它们不会中止构建过程，但会返回错误。

**`eapply` 支持**

`eapply` 命令是对 `epatch` （来自 `eutils.eclass`）的简化替代，在 package 管理器中实现。它的文件或目录参数中的补丁使用 patch 来应用 `-p1`，但它接受 GNU 补丁中的 `patch(1)` 选项以覆盖默认行为。

**`eapply_user` 支持**

`eapply_user` 命令允许软件包管理器应用用户提供的补丁程序。必须从每个 `src_prepare` 函数中调用它。

<div class="alert alert-note">
<b>注意</b>：<code><pre>eapply_user</pre></code>调用default时，无需显式 <code><pre>src_prepare</pre></code>调用。
</div>

**`econf` 添加`--docdir` 并`--htmldir`**

除现有选项外，还将`--docdir`和`--htmldir`选项传递给`configure`。

**`in_iuse` 支持**

如果给定参数在 ebuild `USE`中可用，则`in_iuse`函数返回`true`。

**`unpack` 变化**

- unpack 支持相对路径，但不以./开头（unpack foo / bar.tar.gz 是有效的相对路径）。
- 解压缩支持.txz（xz 压缩 tarball）。
- 解压缩不区分大小写地匹配文件扩展名。

`einstalldocs` 支持

`einstalldocs`函数将安装由`DOCS`变量（或未设置`DOCS`的默认文件集）和`HTML_DOCS`变量指定的文件。

**`get_libdir` 支持**

该 `get_libdir` 命令输出 `lib*`适用于当前二进制应用程序接口目录基本名称。

## EAPI 7

### EAPI 7 术语

文档可以使用以下术语更好地描述依赖关系和安装目标。

**`CHOST`**

将运行已安装的软件包的系统。

**`CBUILD`**

该系统用于构建包。不使用交叉编译时，CBUILD == CHOST。

**`CTARGET`**

用于某些交叉编译中，通常为空值。

### EAPI 7 变量

**`PORTDIR`和`ECLASSDIR`被删除**

不再定义`PORTDIR`和`ECLASSDIR`，不能在 ebuild 中使用它们来访问这些目录。

**`DESTTREE`和`INSDESTTREE`已删除**

意外的导出变量`PORTDIR`和`ECLASSDIR`不能在 ebuild 中用于操纵安装路径。分别使用`in`或`insinto`代替。

**`D`，`ED`，`ROOT`和`EROOT`已修改**

`EAPI=7`中这些变量不再包含斜杠。

**`BDEPEND`已添加**

以前，所有构建时工具和库都进入`DEPEND`。现在，内置时间依赖项分为`DEPEND`和`BDEPEND`。区别仅在于`BDEPEND`是要在`CBUILD`上执行的依赖项。 `DEPEND`保留用于 CHOST 的其他依赖项，例如库。这改善了交叉编译支持。

**`BROOT`已添加**

`BROOT`是根目录的绝对路径，包括任何前缀，包含 BDEPEND 满足的构建依赖关系，通常是可执行的构建工具。

**`SYSROOT`和`ESYSROOT`已添加**

`SYSROOT`是`DEPEND`中依赖项的安装位置。 `ESYSROOT`是附加了`EPREFIX`的`SYSROOT`。

**`ENV_UNSET`已添加**

空格分隔的变量列表，这些变量将从构建环境中删除。

### EAPI 7 元数据

**空分组被禁止**

空的分组例如`DEPEND="|| ( ${empty_var} )"`现在将产生错误。此外，更严格地执行分组中的条件。例如，`REQUIRED_USE="|| ( foo? ( bar ) baz? ( zoinks )"`以前可以与`USE ="-foo -baz"`一起使用，现在需要`USE="foo bar"`或`USE="baz zoinks"`。

### EAPI 7 配置文件

**`package.provided` 被禁止**

`EAPI=7`中配置文件可能不再包含带有的 `package.provided` 文件 。

### EAPI 7 助手

**`dohtml` 被禁止**

`EAPI=7`禁止使用`dohtml`助手。

**`dolib` 和 `libopts` 被禁止**

`EAPI=7`禁止使用 `dolib` 助手和相关的 `libopts`。

**`has_version` 和 `best_version` 变化**

`has_version` 和 `best_version` 现在支持可选开关来确定要检查的依赖项类型。

- `-r` （默认）将检查运行时依赖项（RDEPEND）
- `-d` 将检查 CHOST 建立时间的依存关系（DEPEND）
- `-b` 将检查 CBUILD 建立时间依赖性（BDEPEND）

**版本操作和比较命令**

EAPI=7 引入了三个用于通用版本号操作的命令。

- `ver_cut` 获取版本字符串的子字符串
- `ver_rs` 替换版本字符串中的分隔符
- `ver_test` 比较两个版本

有关常见用途或 [深入了解](https://dev.gentoo.org/~mgorny/articles/the-ultimate-guide-to-eapi-7.html#version-manipulation-and-comparison-commands)的示例，请参见[版本和名称格式问题](./variables.md)。

**新函数 `eqawarn`**

`EAPI=7`已添加`eqawarn`帮助程序。此功能是为了提醒开发人员不推荐使用的功能。它不再包含在`eutils` eclass 中。

**新函数 `dostrip`**

`EAPI=7`已添加`dostrip`帮助程序。此功能控制是否剥离二进制文件。 `dostrip -x [file]`将排除二进制文件。相反，当与 RESTRICT=strip 结合使用时，`dostrip [file]`选择要剥离的二进制文件。

**`die` 和 `assert` 变化**

- 这些命令现在可以在子 shell 中安全使用，并且就像在主进程中被调用一样起作用。

**`nonfatal` 变化**

- `nonfatal` 现在适用于 Shell 函数和子流程。

**`domo` 行为改变**

- `domo`（对于本地化）现在忽略 `into` 指令。这遵循类似的命令，比如 `doinfo` 和 `doman`。

**`econf` 变化**

- 分别指定`CBUILD`和`CTARGET`的交叉编译选项`--build`和`--target`选项已添加，并且对所有 EAPI 具有追溯作用。另外，如果构建支持`--with-sysroot`，则将传递正确的值，以使常规编译和交叉编译成功。
