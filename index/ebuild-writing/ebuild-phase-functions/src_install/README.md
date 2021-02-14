# src_install

|          |                     |
| :------- | :------------------ |
| **函数** | `src_install`       |
| **目的** | 安装软件包到 `${D}` |
| **沙盒** | 已启用              |
| **权限** | root                |
| **调用** | ebuild              |

## 默认 `src_install`

对于 EAPI 4 和更高版本，默认 `src_install` 函数如下：

```bash
src_install() {
	if [[ -f Makefile ]] || [[ -f GNUmakefile ]] || [[ -f makefile ]] ; then
		emake DESTDIR="${D}" install
	fi

	if ! declare -p DOCS >/dev/null 2>&1 ; then
		local d
		for d in README* ChangeLog AUTHORS NEWS TODO CHANGES THANKS BUGS \
				FAQ CREDITS CHANGELOG ; do
			[[ -s "${d}" ]] && dodoc "${d}"
		done
	elif [[ $(declare -p DOCS) == "declare -a"* ]] ; then
		dodoc "${DOCS[@]}"
	else
		dodoc ${DOCS}
	fi
}
```

对于 EAPI 6 和更高版本，默认 `src_install` 函数如下：

```bash
src_install() {
	if [[ -f Makefile ]] || [[ -f GNUmakefile ]] || [[ -f makefile ]] ; then
		emake DESTDIR="${D}" install
	fi
	einstalldocs
}
```

## `src_install`样例

```bash
src_install() {
	emake DESTDIR="${D}" install
	dodoc README CHANGES
}
```

## 易于安装

通常，尤其是在使用自动工具的软件包中，有一个`Makefile install`目标，该目标将遵循`DESTDIR`变量来告知其安装到非 root 目录。如果可能，应使用：

```bash
	emake DESTDIR="${D}" install
```

<div class="alert alert-note">
<b>注意</b>： <code><pre>emake</pre></code>此处应用于并行化。某些安装并非旨在并行化，使用，在出现错误时使用<code><pre>emake -j1</pre></code>或者<code><pre>make</pre></code>。
</div>

通常情况下，软件包的构建系统不会安装 `README`， `ChangeLog` 等文件，因此有必要为其添加其他`dodoc`语句：

```bash
	emake DESTDIR="${D}" install
	dodoc README CHANGES
	dodoc -r doc
```

`dodoc` 支持`-r` 作为第一个参数，它允许递归安装目录。

<div class="alert alert-note">
<b>注意</b>： 没有必要使用<code><pre>dodoc</pre></code> <code><pre>COPYING</pre></code>！许可证属于<code><pre>${PORTDIR}/licenses</pre></code>。但是有时，你可能想要安装<code><pre>COPYING</pre></code>，例如，无论是否说明了如何将不同的许可证应用于应用程序的不同部分。
</div>

## 琐碎的安装

对于某些没有 `Makefile` 仅安装少量文件的软件包，使用`cp`编写手动安装是最简单的选择。例如，进行一些主题的简单安装（无需编译）：

```bash
	dodir /usr/share/foo-styles/
	cp -R "${S}/" "${D}/" || die "Install failed!"
```

或者有时是 `insinto` 和`doins`的组合（相关函数——请参阅[安装函数参考](./../../../function-reference/install-functions-reference.md)）——以下内容基于 `sys-fs/udev`安装：

```bash
src_install() {
	dobin udevinfo
	dobin udevtest
	into /
	dosbin udev
	dosbin udevd
	dosbin udevsend
	dosbin udevstart
	dosbin extras/scsi_id/scsi_id
	dosbin extras/volume_id/udev_volume_id

	exeinto /etc/udev/scripts
	doexe extras/ide-devfs.sh
	doexe extras/scsi-devfs.sh
	doexe extras/cdsymlinks.sh
	doexe extras/dvb.sh

	insinto /etc/udev
	newins "${FILESDIR}/udev.conf.post_050" udev.conf
	doins extras/cdsymlinks.conf

	# For devfs style layout
	insinto /etc/udev/rules.d/
	newins etc/udev/gentoo/udev.rules 50-udev.rules

	# scsi_id configuration
	insinto /etc
	doins extras/scsi_id/scsi_id.config

	# set up symlinks in /etc/hotplug.d/default
	dodir /etc/hotplug.d/default
	dosym ../../../sbin/udevsend /etc/hotplug.d/default/10-udev.hotplug

	# set up the /etc/dev.d directory tree
	dodir /etc/dev.d/default
	dodir /etc/dev.d/net
	exeinto /etc/dev.d/net
	doexe etc/dev.d/net/hotplug.dev

	doman *.8
	doman extras/scsi_id/scsi_id.8

	dodoc ChangeLog FAQ HOWTO-udev_for_dev README TODO
	dodoc docs/{overview,udev-OLS2003.pdf,udev_vs_devfs,RFC-dev.d,libsysfs.txt}
	dodoc docs/persistent_naming/* docs/writing_udev_rules/*

	newdoc extras/volume_id/README README_volume_id
}
```

当然，这比简单的 `Makefile` 安装要难得多。

## 其他安装

有时，会有一个不遵循`DESTDIR`的`Makefile`和要安装的大量文件。在这些情况下，最好修复`Makefile`并联系上游，向他们解释情况。

## `src_install` 流程

以下小节涵盖编写 `src_install` 函数时经常出现的不同主题 。

- [可控压缩](./controllable-compression.md)
