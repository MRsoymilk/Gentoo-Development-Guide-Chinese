# 变量

在 ebuild 中必须设置许多特殊变量，可以选择指定更多特殊变量。在整个 ebuild 中也使用了一些预定义的变量。

## 预定义的只读变量

为你定义了以下变量。你不得尝试设置它们。编写 ebuild 时，开发人员不得依赖于软件包管理器的这些变量的特定值。

| **变量**              | **目的**                                                                                                                                                                                                    |
| :-------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `P`                   | 软件包名称和版本（不包括修订版，如果有的话），例如 `vim-6.3`。                                                                                                                                              |
| `PN`                  | 包名称，例如 `vim`。                                                                                                                                                                                        |
| `PV`                  | 软件包版本（不包括修订版，如果有的话），例如 `6.3`。它应反映上游版本控制方案。                                                                                                                              |
| `PR`                  | 软件包修订版，若不存在修订版， 则为`r0`。                                                                                                                                                                   |
| `PVR`                 | 包版本和修订（如果有的话），例如 `6.3`，`6.3-r1`。                                                                                                                                                          |
| `PF`                  | 完整的软件包名称，`${PN}-${PVR}`例如 `vim-6.3-r1`。                                                                                                                                                         |
| `A`                   | 软件包的所有源文件（不包括由于 `USE` 标志而无法使用的文件）。                                                                                                                                               |
| `CATEGORY`            | 包的类别，例如 `app-editors`。                                                                                                                                                                              |
| `FILESDIR`            | ebuild `files/`目录的路径，通常用于小补丁和文件。例如： `"${PORTDIR}/${CATEGORY}/${PN}/files"`。                                                                                                           |
| `WORKDIR`             | ebuild 的根生成目录的路径。例如： `"${PORTAGE_BUILDDIR}/work"`。                                                                                                                                           |
| `T`                   | ebuild 可以使用的临时目录的路径。例如：`"${PORTAGE_BUILDDIR}/temp"`。                                                                                                                                      |
| `D`                   | 临时安装目录的路径。例如： `"${PORTAGE_BUILDDIR}/image"`。                                                                                                                                                 |
| `HOME`                | 临时目录的路径，供 ebuild 调用的可能读取或修改主目录的任何程序使用。例如： `"${PORTAGE_BUILDDIR}/homedir"`。                                                                                               |
| `ROOT`                | 软件包将合并到的根目录的绝对路径。仅在 `pkg_*`阶段中允许。见 [ROOT](./variables.md)。                                                                                                                                     |
| `DISTDIR`             | 包含目录的路径，该目录存储了为该软件包获取的所有文件。                                                                                                                                                      |
| `EPREFIX`             | 偏移安装的标准化偏移前缀路径。更多信息请参见 [Gentoo 前缀技术文档](https://wiki.gentoo.org/wiki/Project:Prefix/Technical_Documentation)。                                                                                                                                      |
| `ED`                  | `${D%/}${EPREFIX}/`的简写。                                                                                                                                                                                 |
| `EROOT`               | `${ROOT%/}${EPREFIX}/`的简写。                                                                                                                                                                              |
| `SYSROOT`             | （EAPI = 7）根目录的绝对路径，包含`DEPEND`满足的构建依赖项                                                                                                                                                  |
| `ESYSROOT`            | （EAPI = 7）`${SYSROOT%/}${EPREFIX}/`的简写。                                                                                                                                                               |
| `BROOT`               | （EAPI = 7）根目录的绝对路径，其中包含 `BDEPEND`，通常是可执行的构建工具所满足的构建依赖关系。                                                                                                              |
| `MERGE_TYPE`          | 被合并的软件包的类型。可能的值包括：`source`（如果从源代码构建和安装软件包），`binary`（如果安装先前从 ebuild 生成的二进制软件包），`buildonly`（仅构建而不安装二进制软件包）。                             |
| `REPLACING_VERSIONS`  | 由于此安装而被替换（卸载或覆盖）的所有软件包（`PVR`）的空格分隔列表。它是一个列表，而不是一个可选值，用于处理病理情况，例如安装`foo-2:2`来替换`foo-2:1`和`foo-3:2`。在`pkg_preinst`和`pkg_postinst`中可用。 |
| `REPLACED_BY_VERSION` | 如果要在安装过程中将其卸载，则此软件包的单个版本（`PVR`）将替换此 ebuild 提供的版本。否则为空字符串，即是否要卸载而不替换。在`pkg_prerm`和`pkg_postrm`中可用。                                              |

## Ebuild 定义的变量

每个 ebuild 可能会或必须定义以下变量。

| **变量**       | **目的**                                                                                                                                                                                                                                                                                                                                     |
| :------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `EAPI`         | EAPI。请参阅 [EAPI 用法和说明](./eapi-usage-and-description.md)。                                                                                                                                                                                                                                                                                                           |
| `DESCRIPTION`  | 软件包目的的简短描述（不超过 80 个字符）。必选。                                                                                                                                                                                                                                                                                             |
| `HOMEPAGE`     | 软件包的主页。强制（虚拟除外），接受多个值。在某些情况下，为了方便起见，习惯上在主页之外还提供了软件包跟踪器和/或代码托管链接（例如，Python 软件包的 PyPI 链接，当无法在主页上轻松找到代码时的 GitHub 链接）。如果没有该软件包的主页，请将其设置为`https://wiki.gentoo.org/wiki/No_homepage`。永远不要在字符串中引用变量名；仅包含原始文本。 |
| `SRC_URI`      | 包的源 URI 列表。可以包含`USE`条件部分，请参阅[SRC_URI](./variables.md)。                                                                                                                                                                                                                                                                                  |
| `LICENSE`      | 软件包的许可证，与`licenses/`中的文件完全对应（包括大小写）。强制性（虚拟除外）。请参阅[LICENSE](./variables.md)。                                                                                                                                                                                                                                            |
| `SLOT`         | 包的 `SLOT`。必选，参见[slot](./variables.md)。                                                                                                                                                                                                                                                                                                            |
| `KEYWORDS`     | 请参阅[KEYWORDS](./variables.md)以及 [关键字和稳定化](./../keywording-and-stabilization.md)。                                                                                                                                                                                                                                                                                                    |
| `IUSE`         | ebuild 中使用的所有`USE`标志（不包括架构标志，但包括`USE_EXPAND`标志）的列表。请参阅[IUSE](./variables.md)。                                                                                                                                                                                                                                               |
| `REQUIRED_USE` | `USE`标志的配置必须对此声明有效的声明列表。请参阅[REQUIRED_USE](./variables.md)。                                                                                                                                                                                                                                                                          |
| `PROPERTIES`   | 空格分隔的属性列表，支持条件语法。 `interactive` 是目前唯一的有效值。                                                                                                                                                                                                                                                                        |
| `RESTRICT`     | 用空格分隔的要限制的运输函数列表。有效值是 `fetch`，`mirror`，`strip`，`test` 和 `userpriv`。详细信息请参见 man 5 ebuild。                                                                                                                                                                                                                   |
| `DEPEND`       | 软件包的生成依赖项列表。请参阅依赖项。从 EAPI-7 开始，仅适用于 CHOST。                                                                                                                                                                                                                                                                       |
| `BDEPEND`      | （EAPI = 7）软件包的 CBUILD 构建依赖项列表。请参阅 [依赖项](./../general-concepts/dependencies.md)。                                                                                                                                                                                                                                                                              |
| `RDEPEND`      | 软件包的运行时依赖项列表。请参阅 [依赖项](./../general-concepts/dependencies.md)。                                                                                                                                                                                                                                                                                                |
| `PDEPEND`      | 合并软件包后要安装的软件包的列表（如果可能）。仅当`RDEPEND`会引起循环依赖性时才使用此选项。请参阅[依赖项](./../general-concepts/dependencies.md)。                                                                                                                                                                                                                                |
| `S`            | 临时构建目录的路径，由`src_compile`和`src_install`使用。默认值：`"$ {WORKDIR}/${P}"`。如果 Ebuilds 与默认值相同，则**不应**为其提供值。                                                                                                                                                                                                      |
| `DOCS`         | 要使用`dodoc`安装的默认`src_install`函数的文档文件列表，以空格或空格分隔。如果未定义，则使用合理的默认列表。请参阅[默认 src_install 函数](./ebuild-phase-functions/src_install/README.md)。                                                                                                                                                                                                |
| `HTML_DOCS`    | 用于以递归方式安装`einstalldocs`函数的文档文件或目录的数组或空格分隔的列表。（需要 [EAPI>=6](./eapi-usage-and-description.md)。）                                                                                                                                                                                                                                           |

## SLOT

如果不需要 slot，请使用 `SLOT="0"`。千万**不能**使用 `SLOT=""`，因为变量不能为空。

有关此变量的更多信息，请参阅[slot](./../general-concepts/slotting.md)。

## 关键字

在`KEYWORDS`中唯一包含`*`的有效构造是`-*`。不要使用`KEYWORDS ="*"`或`KEYWORDS ="~*"`。

有关如何处理此变量的信息，请参见[关键字和稳定化](./../keywording-and-stabilization.md)。

## SRC_URI

### 有条件的来源

基于 USE 标志的条件源文件允许使用`flag?()`语法。这通常对特定于架构的代码或二进制包很有用 —— 在 ppc 上下载 sparc 二进制文件会浪费带宽。

```bash
SRC_URI="https://example.com/files/${P}-core.tar.bz2
	x86?   ( https://example.com/files/${P}/${P}-sse-asm.tar.bz2 )
	ppc?   ( https://example.com/files/${P}/${P}-vmx-asm.tar.bz2 )
	sparc? ( https://example.com/files/${P}/${P}-vis-asm.tar.bz2 )
	doc?   ( https://example.com/files/${P}/${P}-docs.tar.bz2 )"
```

### 重命名源

有时，上游 URI 使用通用名称，这些通用名称在创建单个镜像时很容易与其他软件包冲突。使用`->`语法，您可以在从上游获取文件时重命名该文件，以存储在镜像和本地 distdir 中。

在这里，我们从上游下载一个文件，其名称类似于`1.0.tar.gz`，并使用更好的名称（如`package-1.0.tar.gz`）进行保存/镜像。与往常一样，所有标记（包括运算符和输出文件名）都必须用空格分隔。

```bash
SRC_URI="https://example.com/files/${PV}.tar.gz -> ${P}.tar.gz"
```

## 第三方镜像

如果`SRC_URI`中的项目在多个第三方镜像上可用，并且同一组镜像在多个 ebuild 中共享，那么您不必在每个 ebuild 中重复列出每个镜像。 `::gentoo`存储库中的`profiles/thirdpartymirrors`文件包含已命名的镜像组，其格式由软件包管理器规范（PMS）指定，可通过`mirror://`伪协议进行访问。

可能会定义一组“示例”镜像，

```bash
example https://download.example.com https://mirror1.example.org/example
```

之后可以通过 `mirror://` URI 进行引用：

```bash
SRC_URI="mirror://example/${PN}/${P}.tar.gz"
```

有两种`thirdpartymirrors`的有效用法 ：

1. 为镜像或获取限制的软件包提供多个下载位置，
2. 处理通过镜像网络分发其 distfile 的上游，而无需主要下载位置或保护服务。

在任何其他情况下，必须改用主要位置。 distfiles 将被[镜像到 Gentoo 基础架构](./../general-concepts/mirrors.md)上。在这种情况下，使用第三方镜像列表的好处不会超过维护它的负担。

## IUSE

请注意，`IUSE`变量是累积性的，因此无需指定仅在 ebuild 的 IUSE 中继承的 eclass 中使用的 USE 标志。

<div class="alert alert-note">
<b>注意</b>：如果IUSE变量为空，则无需在其中将其分配。
</div>

架构 USE 标记（`sparc`，`mips`，`x86-fbsd` 等）不应该被列出。

## 许可证

可以指定多个`LICENSE`条目，这些条目仅在设置了特定`USE`标志时才适用。格式与`DEPEND`相同。有关详细信息，请参见[GLEP 23](https://www.gentoo.org/glep/glep-0023.html)。

## ROOT

`ROOT`背后的思想是，可以用`ROOT=/somewhere`构建一个系统，然后将其 chroot 或 tar 压缩 `/somewhere` 为系统映像。它的设计不允许用户运行`/somewhere/usr/bin/foo`。

Ebuild 只能在`pkg_*`阶段引用`ROOT`。由于合并二进制包时`ROOT`可能不同，因此无法在`src_*`阶段正确使用它。例如，可以使用`ROOT=/`构建二进制包，然后使用`ROOT=/somewhere`将其安装到系统上。

构建软件包时，不应使用`ROOT`来满足对库，标头文件等的依赖。相反，应使用`/`指定构建系统上的文件。

在交叉编译环境中，ebuild 不得调用`ROOT`内的任何二进制文件，因为它们可能无法在生成系统上执行。

以下是一个 ebuild 的示例，如果仍然存在旧的和过时的配置文件，则在`pkg_postinst()`中使用`ROOT`来有条件地打印错误消息：

```bash
pkg_postinst() {
	if [[ -e "${ROOT}/etc/oldconfig" ]]; then
		ewarn "You still have the obsolete config file "
		ewarn "    ${ROOT}/etc/oldconfig."
		ewarn "Please migrate your settings to ${ROOT}/etc/newconfig"
		ewarn "and remove ${ROOT}/etc/oldconfig."
	fi
}
```

## 版本和名称格式问题

通常上游的 tarball 版本或命名格式不太符合 Gentoo 约定。在名称中使用大写字母或在版本中使用下划线和连字符的方式上的差异特别常见。例如，Gentoo 所谓的`foo-1.2.3b`可能期望使用名为`Foo-1.2-3b.tar.bz2`的压缩包。最好不要使用`MY_PN`，`MY_PV`和`MY_P`变量，并使用它们来定义上游命名，而不是对包字符串进行硬编码。 EAPI = 7 启用了一组新功能 ver_cut，ver_rs 和 ver_test。这些已使用`eapi7-ver` eclass 移植到较旧的 EAPI 中。重新定义版本的简便方法是使用以下功能，除非您确定自己知道自己在做什么，否则应使用以下功能：

```bash
inherit eapi7-ver
MY_PN="Foo"
# Replace the second period separator in PV with -
MY_PV=$(ver_rs 2 '-')
MY_P="${MY_PN}-${MY_PV}"
```

该代码的优点是，即使上游切换到`Foo-1.3-4.5.tar.bz2`之类的格式，它也可以继续工作（是的，这确实发生了）。

也可以使用 bash 替换来达到相同的效果（这是 eapi7-ver 在内部的工作方式），但这很复杂，容易出错并且难以阅读。

一些 ebuild 利用`sed`，`awk`和/或`cut`来执行此操作。这*不得*对任何新代码执行此操作，而且尽可能使用 eapi7-ver 或 bash 替换。不建议使用全局范围的非 bash 代码。

ver\_ 函数用于从版本字符串中提取特定组件。有关更多文档和示例，请参见`man eapi7-ver.eclass`和 eclass 源代码。功能的简要概述如下。

| 函数                     | 目的                                         |
| :----------------------- | :------------------------------------------- |
| `ver*rs [range] ' '`    | 获取重要的版本组件，但不包括'.'，'-'和'\_'。 |
| `ver_cut 1 `             | 获取主要版本。                               |
| `ver_cut [range] `       | 从版本字符串中提取一系列子部分。             |
| `ver_cut 2- `            | 在主要版本之后获取所有内容。                 |
| `ver_rs [range] [char] ` | 替换特定的版本分隔符。                       |
| `ver_rs 1- [char]`       | 更换所有版本分隔符。                         |
| `ver_rs [range] '' `     | 删除版本分隔符。                             |
| `ver_rs 1- '' `          | 删除所有版本分隔符。                         |

## 变量中的尾部斜杠

在 EAPI 7 中，以下变量永远不会以斜杠结尾：`D`，`ED`，`ROOT`，`EROOT`。相反，在 EAPI 7 之前的 EAPI 中，这些变量需要以斜杠结尾。在使用 EAPI 7 之前的 EAPI 时，鼓励开发人员使用 bash 后缀删除作为尾部斜杠，并在加入路径时显式添加`/`。例如：`${D%/}/`，`${ED%/}/`，`${ROOT%/}/`，`${EROOT%/}/`。

## 在 ebuild 中使用常量值变量

变量在 ebuild 中具有重要价值，可以避免不必要的重复并简化维护。但是，应谨慎使用对常量的引用，因为过量使用它们会损害可读性并增加维护负担（例如，在重命名软件包时）。特别是，应避免使用其值与预期字符串没有直接相关性的变量。

有益的常量值变量引用的示例包括：

- 在`SRC_URI`和`S`中使用`${PV}`及其派生变量（`${P}`，`${MY_P}`…）来避免必须为每个版本改变而更新这些变量（假设`${PV}`用于指示上游版本）；
- 在`--docdir`路径中使用`${PF}` —— 这是规范的 Gentoo 路径，始终需要与`${PF}`匹配。

错误的常量值变量引用的示例是：

- 在`HOMEPAGE`，`EGIT_REPO_URI`或`SRC_URI`的域中使用`${PN}` —— 破坏了编辑器和终端机中的 URL 解析，并使复制链接供外部使用（例如，通过 gitweb 进行审核时）变得困难，
- 在`SRC_URI`中使用`${HOMEPAGE}` —— 在更改主页或添加其他条目时会中断，
- 在`src_install()`中过度使用`${PN}`（或其他不必要的辅助变量）—— 损害了可读性，无济于事，并在需要重命名软件包时造成很多麻烦。

## REQUIRED_USE

`REQUIRED_USE`变量包含一个声明列表，这些声明必须由 USE 标志的配置来满足才能对该 ebuild 有效。为了匹配，必须启用终端元素中的 USE 标志（如果它带有感叹号前缀，则必须禁用）。

本质上，`REQUIRED_USE`是`DEPEND`样式语法的类似物。例如，要声明禁止某些组合，即“如果设置了 foo，则必须取消设置 bar”：

```bash
REQUIRED_USE="foo? ( !bar )"
```

要声明“如果设置了 foo，则必须激活 bar，baz 和 quux 中的至少一个”：

```bash
REQUIRED_USE="foo? ( || ( bar baz quux ) )"
```

要声明“必须正确设置 foo，bar 或 baz 之一，但不能设置多个”：

```bash
REQUIRED_USE="^^ ( foo bar baz )"
```

请注意，最后一个关系是“异或”（XOR）的关系。虽然可以通过通常的 DEPEND 语法形成 XOR，但已为这种情况添加了特定的^^运算符。

最后，声明“必须设置 foo，bar 或 baz 中的至少一个”：

```bash
REQUIRED_USE="|| ( foo bar baz )"
```

<div>
<b>重要提示</b>：有关何时使用（以及何时不使用）<code><pre> REQUIRED_USE </pre></code>的信息，请参见<a href="../general-concepts/use-flags.md">冲突 USE 标志</a>。
</div>

## EAPI 5

EAPI 5 添加了另一种情况，以简化冲突的 USE 标志。

要声明“必须设置为零，或者必须设置 foo，bar 或 baz 之一，但不能设置多个”：

```bash
REQUIRED_USE="?? ( foo bar baz )"
```

在先前的 EAPI 中，这将与以下内容相同：

```bash
REQUIRED_USE="foo? ( !bar !baz ) bar? ( !foo !baz ) baz? ( !foo !bar )"
```
