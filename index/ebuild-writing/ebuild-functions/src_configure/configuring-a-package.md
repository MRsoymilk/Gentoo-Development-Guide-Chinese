# 配置软件包

许多软件包都带有自动配置生成的`./configure` 脚本，用于检查构建环境和配置对库的可选支持。`econf` 函数应在可能的地方使用——这将为 Gentoo 环境提供正确的构建和路径规范。

通常，configure 脚本会尝试根据已安装的软件包自动启用对可选组件的支持。这一定是不允许发生的。例如，如果用户安装了 Perl 但具有 `USE="-perl"`，则具有可选 Perl 支持的软件包不得链接到 Perl。通常可以使用`--enable-`和`--disable`或`--with-`和`--without-`开关来覆盖这种自动检测函数（但请注意，这些检测并不总是有效的——请确保已对它们进行了正确的测试！）。

`use_enable` 和 `use_with` 实用函数应该在适当情况下可用于产生这些开关。

```bash
src_configure() {
	# We have optional perl, python and ruby support
	econf \
		$(use_enable perl ) \
		$(use_enable python ) \
		$(use_enable ruby )
}

src_configure() {
	# Our package optional IPv6 support which uses --with rather than
	# --enable / --disable

	econf $(use_with ipv6 )
}
```

有时，软件包的选项名称选择将与 `USE` 标志的名称或大小写将不完全匹配。带有 `X` 标志的情况经常如此。对于这些情况，有两种参数形式：

```bash
src_configure() {
	# Our package has optional perl, python and ruby support
	econf \
		$(use_enable perl perlinterp ) \
		$(use_enable python pythoninterp ) \
		$(use_enable ruby rubyinterp )

	# ...
}

src_configure() {
	econf $(use_with X x11 )
}
```

要检查未设置的 `USE` 标志， 可以使用`use_enable !flag`表格。

## `econf` 选项

`econf` 设计用于与 GNU 自动配置生成的配置脚本一起使用。它首先将下面列出的默认选项传递到配置脚本，然后将任何其他参数传递给 `econf`。

- `--prefix="${EPREFIX}"/usr`
- `--mandir="${EPREFIX}"/usr/share/man`
- `--infodir="${EPREFIX}"/usr/share/info`
- `--datadir="${EPREFIX}"/usr/share`
- `--sysconfdir="${EPREFIX}"/etc`
- `--localstatedir="${EPREFIX}"/var/lib`
- `--build="${CBUILD}"`（仅在`CBUILD`非空时通过）
- `--host="${CHOST}"`
- `--target="${CTARGET}"`（仅在`CTARGET`非空时通过）
- `--libdir`根据配置文件中`LIBDIR_${ABI}`变量的值设置。
- `--disable-dependency-tracking`
- `--disable-silent-rules`

在 EAPI 6 和更高版本中，还传递了以下选项：

- `--docdir="${EPREFIX}"/usr/share/doc/${PF}`
- `--htmldir="${EPREFIX}"/usr/share/doc/${PF}/html`

在 EAPI 7 和更高版本中，还传递了以下选项：

- `--with-sysroot="${ESYSROOT:-/}"`
