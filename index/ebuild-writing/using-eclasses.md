# 使用 Eclass

eclass 是软件包之间共享的函数或函数的集合（库）。有关 eclass 可以做什么，它们如何工作以及如何编写的完整信息，请参阅[Eclass 书写指南](./../eclass-writing-guide.md)；有关各种常用 eclass 的文档，请参阅[Eclass 参考](./../eclass-reference.md)。本节仅说明如何使用已编写的 eclass。

## `inherit`函数

要使用 eclass，必须“继承”它。这是通过`ebuild.sh`提供的继承函数完成的。`inherit`必须在任何函数*之前*位于 ebuild 的顶部。有条件的继承是非法的（除非继承条件是高速缓存且恒定，否则请参见[Portage Cache](./../general-concepts/the-portage-cache.md)）。

继承 eclass 后，可以正常使用其提供的函数。这是一个`foomatic-0.1-r2.ebuild`的 ebuild 示例，其中使用了三个 eclass：

```bash
# Copyright 1999-2020 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2

EAPI=7

inherit autotools bash-completion-r1 flag-o-matic

DESCRIPTION="Tool for foo"
HOMEPAGE="https://foomatic.sf.net"
SRC_URI="mirror://sourceforge/${PN}/${P}.tar.gz"

LICENSE="GPL-2"
SLOT="0"
KEYWORDS="alpha ~amd64 ~x86 ~x86-fbsd"

RDEPEND="sys-libs/ncurses:0=
	>=sys-libs/readline:0="
DEPEND="${RDEPEND}"

src_prepare() {
	eapply "${FILESDIR}/${P}-gentoo.patch"
	eapply_user
	eautoreconf
}

src_configure() {
	econf --sysconfdir="${EPREFIX}"/etc/devtodo
}

src_compile() {
	replace-flags -O? -O1
	default
}

src_install() {
	default
	dobashcomp "${FILESDIR}/${PN}.bash-completion" ${PN}
}

```

请注意 `inherit` 紧跟标题之后。

要获取 `eautoreconf` 函数，需要使用 `autotools` eclass（请参见[autotools.eclass](./../eclass-reference/autotools.eclass.md)）。替换标志需要`flag-o-matic` eclass（请参阅[flag-o-matic.eclass](./../eclass-reference/flag-o-matic.eclass.md)），`bash-completion-r1` eclass（[bash-completion-r1.eclass](./../eclass-reference/bash-completion-r1.eclass.md)）用于通过 `dobashcomp` 处理 bash 补全文件。
