# Ebuild 快速入门指南

该页面提供了有关 ebuild 编写的非常简短的介绍。它并未尝试涵盖开发人员将遇到的许多细节或问题，而是提供了一些琐碎的示例，这些示例在尝试掌握 ebuilds 的基本思想时可能有帮助。

有关所有内容的正确介绍，请参阅[Ebuild Writing](./ebuild-writing/README.md)。[一般概念](./general-concepts/README.md)章节也将提供帮助。

请注意，此处使用的示例虽然基于真实的 tree ebuilds，但其中的部分内容已被裁剪，更改和简化。

## 第一个 Ebuild

我们将从源代码索引工具*Exuberant Ctags*的 ebuild 开始。这是一个简化的`dev-util/ctags/ctags-5.5.4.ebuild`（你可以在 main tree 中看到真实的 ebuild）。

```bash
# Copyright 1999-2020 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2

EAPI=6

DESCRIPTION="Exuberant ctags generates tags files for quick source navigation"
HOMEPAGE="https://ctags.io/ https://github.com/universal-ctags/ctags"
SRC_URI="mirror://sourceforge/ctags/${P}.tar.gz"

LICENSE="GPL-2+"
SLOT="0"
KEYWORDS="~mips ~sparc ~x86"

src_configure() {
	econf --with-posix-regex
}

src_install() {
	emake DESTDIR="${D}" install

	dodoc FAQ NEWS README
}
```

### 基本格式

如你所见，ebuild 只是`bash`在特殊环境中执行的脚本。

ebuild 的顶部是标题块。这在所有 ebuild 中都存在。

Ebuild 使用`Tab`缩进，每个`Tab`代表四个位置。请参阅[Ebuild 文件格式](./ebuild-writing/ebuild-file-format.md)。

## 信息变量

下面是一系列变量。这些变量告诉 Portage 需要的 ebuild 和软件包的各种相关信息。

`EAPI`：请参阅[EAPI 用法和说明](./ebuild-writing/eapi-usage-and-description.md)。

`DESCRIPTION`：软件包及其用途的简短描述。

`HOMEPAGE`：软件包的主页（请记住，包括 URI 格式，例如`https://`）。

`SRC_URI`：告诉 Portage 下载的源码包的地址。在这里 `mirror://sourceforge/`是一个特殊的符号，表示“任何镜像”。`${P}`是 Portage 设置的只读变量，它是软件包的名称和版本——在本例中为 `ctags-5.5.4`。

`LICENSE`：`GPL-2+`（GNU 通用公共许可证第 2 版或（由你选择）任何更新的版本）。

`SLOT`：告诉 Portage 安装该软件的版本。如果你之前从未见过 slots，请使用`"0"`或者阅读 [Slotting](./general-concepts/slotting.md)。

`KEYWORDS`：设置为在其上测试过此 ebuild 的架构。对于新编写的 ebuild，我们使用关键字`~`即使软件包似乎可以正常使用，它们也不会直接提交给稳定版。有关详细信息，请参见关键字和稳定化。

### 构建函数

`src_configure`函数：当要*configure*软件包时，Portage 将调用此函数。该 `econf` 函数是`./configure`调用的包装器。如果由于某种原因在`econf`中发生错误 ，Portage 将停止工作而不是尝试继续安装。

`src_install`函数：当要安装软件包时，Portage 会调用 该函数。这里有一点微妙之处 —— 我们必须直接安装到`${D}`变量指定的特殊位置（而不是直接安装到实时文件系统中）（Portage 对此进行了设置 —— 请参阅[安装目标](./general-concepts/install-destinations.md)和[沙盒](./general-concepts/sandbox.md)）。

<div class="alert alert-note">
<b>注意</b>： 规范的安装方法是<code><pre>emake DESTDIR="${D}" install</pre></code> 。这种方法支持任何正确编写的标准 <code><pre>Makefile</pre></code>文件。如果这会导致沙盒错误，请参阅 <a href="./ebuild-writing/ebuild-functions/src_install/README.md">src_install</a> 以了解如何进行手动安装。
</div>

`dodoc`: 将文件安装到`/usr/share/doc`相关部分的辅助函数。

Ebuild 可以定义其他函数（请参阅 `Ebuild 函数`）。在所有情况下，Portage 提供了一个合理的默认实现，该实现通常会做“正确的事情”。不需要定义 `src_unpack` 和 `src_compile` 函数，例如 —— `src_unpack` 函数用于解压缩压缩文件或为源文件打补丁，但这种情况下默认实现做了我们需要的一切。同样，默认 `src_compile` 函数将调用`make`的包装：`emake`。

<div class="alert alert-note">
<b>注意</b>： 以前，必须在每个命令之后使用<code><pre>|| die</pre></code>构造以检查错误。在 EAPI 4 后，这不再是必需的 —— 如果发生故障，Portage 提供的函数将默认处理。
</div>

## 具有依赖关系的 Ebuild

在 ctags 的示例中，我们没有告诉 Portage 任何依赖关系。碰巧，这没关系，因为 ctags 只需要一个基本的工具链即可进行编译和运行（请参阅[隐式系统依赖关系](./general-concepts/dependencies.md)以了解为什么我们不需要显式依赖它们）。但是，生活很少那么简单。

这是`app-misc/detox/detox-1.1.1.ebuild`：

```bash
# Copyright 1999-2020 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2

EAPI=5

DESCRIPTION="detox safely removes spaces and strange characters from filenames"
HOMEPAGE="http://detox.sourceforge.net/"
SRC_URI="mirror://sourceforge/${PN}/${P}.tar.bz2"

LICENSE="BSD"
SLOT="0"
KEYWORDS="~hppa ~mips sparc x86"

RDEPEND="dev-libs/popt"
DEPEND="${RDEPEND}
	sys-devel/flex
	sys-devel/bison"

src_configure() {
	econf --with-popt
}
```

同样，你可以看到 ebuild 标题块和各种参考变量。在 `SRC_URI`中，`${PN}`用于获取不带版本后缀的软件包名称（更多变量 —— 请参见[预定义的只读变量](./ebuild-writing/variables.md)）。

我们仅定义 `src_configure` 。 `src_install` 不需要定义，因为调用 `emake install`和安装通用文档文件的默认实现对于该软件包是正确的。

`DEPEND` 和 `RDEPEND` 变量决定了 Portage 构建和运行需要的软件包。`DEPEND` 变量列出了编译时依赖，`RDEPEND` 变量列出了运行时依赖。有关更多复杂的示例，请参见 [依赖关系](./general-concepts/dependencies.md)。

## 具有补丁的 Ebuild

通常，我们会使用补丁。这可以通过在 `src_prepare`函数中 使用 eapply 助手函数完成。要使用 eapply 一个必要条件是使用 EAPI7。这是 `app-misc/detox/detox-1.1.0.ebuild`：

```bash
# Copyright 1999-2020 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2

EAPI=7

DESCRIPTION="detox safely removes spaces and strange characters from filenames"
HOMEPAGE="http://detox.sourceforge.net/"
SRC_URI="mirror://sourceforge/${PN}/${P}.tar.bz2"

LICENSE="BSD"
SLOT="0"
KEYWORDS="~hppa ~mips ~sparc ~x86"

RDEPEND="dev-libs/popt"
DEPEND="${RDEPEND}
	sys-devel/flex
	sys-devel/bison"

src_prepare() {
	eapply "${FILESDIR}"/${P}-destdir.patch \
		"${FILESDIR}"/${P}-parallel_build.patch
	eapply_user
}

src_configure() {
	econf --with-popt
}
```

请注意`${FILESDIR}/${P}-destdir.patch`——指的是 `detox-1.1.0-destdir.patch`，它 位于 Gentoo 存储库`files/` 的子目录中。较大的修补程序文件必须位于开发人员的空间：`dev.gentoo.org` 而不是位于 `files/`或镜像中—请参阅[Gentoo 镜像](./general-concepts/mirrors.md)和 [使用 epatch 和 eapply 打补丁](./ebuild-writing/ebuild-functions/src_prepare/patching-with-epatch-and-eapply.md)。

当 `src_prepare` 段落被覆盖时，必须确保 `eapply_user` 被调用。

## 具有 USE 标志的 Ebuild

现在来一下 `USE` 标志。这是`dev-libs/libiconv/libiconv-1.9.2.ebuild `，`libc`没有实现的字符转换库。

```bash
# Copyright 1999-2020 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2

EAPI=5

DESCRIPTION="GNU charset conversion library for libc which doesn't implement it"
HOMEPAGE="https://www.gnu.org/software/libiconv/"
SRC_URI="ftp://ftp.gnu.org/pub/gnu/libiconv/${P}.tar.gz"

LICENSE="LGPL-2+ GPL-3+"
SLOT="0"
KEYWORDS="~amd64 ~ppc ~sparc ~x86"
IUSE="nls"

DEPEND="!sys-libs/glibc"

src_configure() {
	econf $(use_enable nls)
}
```

注意 `IUSE`变量。它列出了 ebuild 使用的所有（非特殊）使用标志。除其他外，它用于`emerge -pv`输出。

软件包的`./configure` 脚本通常采用 `--enable-nls` 或 `--disable-nls` 参数。我们使用`use_enable`实用函数来自动生成此函数（参阅[查询函数参考](./ebuild-writing/variables.md)）。

另一个更复杂的示例，这次基于 `mail-client/sylpheed/sylpheed-1.0.4.ebuild`：

```bash
# Copyright 1999-2020 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2

EAPI=7

inherit desktop

DESCRIPTION="A lightweight email client and newsreader"
HOMEPAGE="https://sylpheed.good-day.net/"
SRC_URI="mirror://sourceforge/${PN}/${P}.tar.bz2"

LICENSE="GPL-2+ LGPL-2.1+"
SLOT="0"
KEYWORDS="alpha amd64 hppa ia64 ppc ppc64 sparc x86"
IUSE="crypt imlib ipv6 ldap nls pda ssl xface"

RDEPEND="=x11-libs/gtk+-2*
	crypt? ( >=app-crypt/gpgme-0.4.5 )
	imlib? ( media-libs/imlib2 )
	ldap? ( >=net-nds/openldap-2.0.11 )
	pda? ( app-pda/jpilot )
	ssl? ( dev-libs/openssl )
	xface? ( >=media-libs/compface-1.4 )
	app-misc/mime-types
	x11-misc/shared-mime-info"
DEPEND="${RDEPEND}
	dev-util/pkgconfig
	nls? ( >=sys-devel/gettext-0.12.1 )"

src_prepare() {
	eapply "${FILESDIR}"/${PN}-namespace.diff \
		"${FILESDIR}"/${PN}-procmime.diff
	eapply_user
}

src_configure() {
	econf \
		$(use_enable nls) \
		$(use_enable ssl) \
		$(use_enable crypt gpgme) \
		$(use_enable pda jpilot) \
		$(use_enable ldap) \
		$(use_enable ipv6) \
		$(use_enable imlib) \
		$(use_enable xface compface)
}

src_install() {
	emake DESTDIR="${D}" install

	doicon sylpheed.png
	domenu sylpheed.desktop

	dodoc [A-Z][A-Z]* ChangeLog*
}
```

注意可选的依赖项。有些 `use_enable` 行使用了两个参数的版本 —— 这对 USE 标志名称与`./configure` 参数不完全匹配时有帮助。
