# Portage 缓存

Portage 对大多数顶级变量（`DEPEND`，`DESCRIPTION`， `SRC_URI`等）使用高速缓存。此缓存可能在不同计算机上生成，因此这些变量必须是静态的，或者仅使用不变的“版本/名称”变量（`P`，`PN`，`PV`，`PR`，`PVR`和`PF`）生成。

因此，以下操作将无效：

```bash
# DO NOT DO THIS!
if ! has_version "x11-libs/gtk+" ; then
	DEPEND="${DEPEND}
		gtk?  ( >=x11-libs/gtk+-2 )
		!gtk? ( =x11-libs/gtk+-1.2* )"
fi
```

但是，这是合法的，因为`eapi7-ver.eclass`可以在`PV`上运行， 并且`PV`和`PN`都是静态的：

```bash
inherit eapi7-ver

if ver_test -ge 7.0 ; then
	IUSE="${IUSE} tcltk mzscheme"
	DEPEND="${DEPEND}
		tcltk?    ( dev-lang/tcl )
		mzscheme? ( dev-lisp/mzscheme )"
	RDEPEND="${RDEPEND}
		tcltk?    ( dev-lang/tcl )
		mzscheme? ( dev-lisp/mzscheme )"

	if [[ "${MY_PN}" != "vim-core" ]] ; then
		RDEPEND="${RDEPEND} !<app-vim/align-30-r1"
	fi
fi
```

## 条件继承

因为 eclass 修改了各种缓存的变量，所以条件继承是不允许的，除非始终在每个系统上获得相同的结果。例如，基于`USE`标志的继承是非法的，但完全基于`PN`的继承是允许的。

作为合法且可能有用的条件继承的示例，某些 eclass 或 ebuild 可以这样使用：

```bash
if [[ ${PV} == 9999 ]]; then
	inherit git-r3
	EGIT_REPO_URI="https://anongit.gentoo.org/git/proj/devmanual.git"
else
	SRC_URI="https://dev.gentoo.org/~ulm/distfiles/${P}.tar.xz"
	KEYWORDS="~amd64 ~arm ~hppa ~ppc ~ppc64 ~sparc ~x86"
fi
```

这允许将相同的 eclass（或相同的 ebuild“模板”）用于常规软件包和实时软件包。
