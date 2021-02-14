# pkg_nofetch

|          |                                  |
| :------- | :------------------------------- |
| **函数** | `pkg_nofetch`                    |
| **目的** | 告诉用户如何处理获取受限的软件包 |
| **沙盒** | 已启用                           |
| **权限** | root                             |
| **调用** | ebuild                           |

## 默认 `pkg_nofetch`

```bash
pkg_nofetch() {
	[ -z "${SRC_URI}" ] && return

	echo "!!! The following are listed in SRC_URI for ${PN}:"
	for MYFILE in `echo ${SRC_URI}`; do
		echo "!!!   $MYFILE"
	done
	return
}
```

## `pkg_nofetch`样例

```bash
pkg_nofetch() {
	einfo "Please download"
	einfo "  - ${P}-main.tar.bz2"
	einfo "  - ${P}-extras.tar.bz2"
	einfo "from ${HOMEPAGE} and place them in your DISTDIR directory."
}
```

<div class="alert alert-note">
<b>注意</b>： 该<code><pre>DISTDIR</pre></code>变量在<code><pre>pkg_*</pre></code>阶段中无效，因此不得引用。
</div>

## `pkg_nofetch`注意事项

仅当设置了 `RESTRICT="fetch"`（请参阅[限制自动镜像](./../../general-concepts/mirrors.md)）的 程序软件包且`distfiles`目录中尚未提供`SRC_URI`中列出的一个或多个组件时，才触发此功能。
