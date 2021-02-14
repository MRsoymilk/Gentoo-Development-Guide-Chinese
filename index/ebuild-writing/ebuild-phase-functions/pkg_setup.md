# pkg_setup

|          |                      |
| :------- | :------------------- |
| **函数** | `pkg_setup`          |
| **目的** | 预构建环境配置和检查 |
| **沙盒** | 未启用               |
| **权限** | root                 |
| **调用** | ebuild, binary       |

## 默认 pkg_setup

```bash
pkg_setup()
{
	return
}
```

## pkg_setup 样例

```bash
pkg_setup() {
	# We need to know which GUI we're building in several
	# different places, so work it out here.
	if use gtk ; then
		if use gtk2 ; then
			export mypkg_gui="gtk2"
		else
			export mypkg_gui="gtk1"
		fi
	elif use motif then
		export mypkg_gui="motif"
	else
		export mypkg_gui="athena"
	fi
}
```
