# src_configure

|          |                 |
| :------- | :-------------- |
| **函数** | `src_configure` |
| **目的** | 配置软件包。    |
| **沙盒** | 已启用          |
| **权限** | user            |
| **调用** | ebuild          |
| **EAPI** | 2               |

## 默认 `src_configure`

```bash
src_configure() {
	if [[ -x ${ECONF_SOURCE:-.}/configure ]] ; then
		econf
	fi
}
```

## `src_configure`样例

```bash
src_configure() {
	use sparc && filter-flags -fomit-frame-pointer
	append-ldflags -Wl,-z,now

	econf \
		$(use_enable ssl ) \
		$(use_enable perl perlinterp )
}
```

## `src_configure` 工艺流程

以下小节涵盖编写 `src_configure` 函数时经常出现的不同主题 。

- [配置软件包](./configuring-a-package.md)
