# rpm 源

如果软件包以.rpm 文件形式提供，则应：

```bash
inherit rpm
```

如果在解压缩阶段不需要执行任何操作，那么将完成 `rpm.eclass` 导出的默认操作 `src_unpack` ，该默认设置将解压缩 RPM 文件。

如果确实需要调用其他解压缩函数，则可以 `src_unpack `使用以下方式进行覆盖：

```bash
src_unpack () {
	rpm_src_unpack ${A}
	cd "${S}"

	use ssl && epatch "${FILESDIR}/${PV}/${P}-ssl.patch"
}
```

<div class="alert alert-note">
<b>注意</b>： <code><pre>${A}</pre></code>可以包含非rpm文件，因为rpm eclass会为非 RPM格式的文​​件调用常规<code><pre>unpack</pre></code>函数。
RPM处理示例
</div>

这是一个基于 SuSE 9.2 的 fetchmail 源 RPM 的 ebuild 片段。ebuild 片段足够完整，可以使用`ebuild unpack` 命令。ebuild 将从 OSU SuSE 镜像下载源文件，解压缩文件并应用随附的补丁程序。文件名应为 `suse-fetchmail-6.2.5.54.1.ebuild`。

```bash
# Copyright 1999-2020 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2

EAPI="6"

inherit eapi7-ver rpm

MY_PV=$(ver_rs 3 '-')
MY_P=fetchmail-${MY_PV}

SRC_URI="https://suse.osuosl.org/suse/i386/9.2/suse/src/${MY_P}.src.rpm"
DESCRIPTION="SuSE 9.2 Fetchmail Source Package"
HOMEPAGE="https://www.suse.com"

LICENSE="GPL-2 public-domain"
SLOT="0"
KEYWORDS="-*"

RESTRICT="mirror"

# Need to test if the file can be unpacked with rpmoffset and cpio
# If it can't then set:

#DEPEND="app-arch/rpm"

# To force the use of rpmoffset and cpio instead of rpm2cpio from
# app-arch/rpm, then set the following:

#USE_RPMOFFSET_ONLY=1

S=${WORKDIR}/fetchmail-$(ver_cut 1-3)

src_unpack () {
    rpm_src_unpack ${A}
}

src_prepare () {
    for i in "${WORKDIR}"/*.patch ; do
        eapply "${i}"
    done
    eapply_user
}
```
