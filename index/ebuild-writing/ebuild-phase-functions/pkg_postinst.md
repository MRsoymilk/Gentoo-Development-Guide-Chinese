# pkg_postinst

|          |                              |
| :------- | :--------------------------- |
| **函数** | `pkg_postinst`               |
| **目的** | image 安装到 `${ROOT}`后调用 |
| **沙盒** | 不具备                       |
| **权限** | root                         |
| **调用** | ebuild，binary               |

## 默认 `pkg_postinst`

```bash
pkg_postinst()
{
	return
}
```

## `pkg_postinst`样例

```bash
pkg_postinst() {
	if $OLD_FLUXBOX_VERSION ; then
		ewarn "You must restart fluxbox before using the [include] /directory/"
		ewarn "feature if you are upgrading from an older fluxbox!"
		ewarn " "
	fi
	elog "If you experience font problems, or if fluxbox takes a very"
	elog "long time to start up, please try the 'disablexmb' USE flag."
	elog "If that fails, please report bugs upstream."
}
```

## 常见 `pkg_postinst` 任务

`pkg_postinst`的最常见用法是显示安装后的参考消息或警告。如果要显示选择性升级消息，则应在较早的阶段对`has_version`进行调用，例如`pkg_preinst`，并将结果存储在全局变量中。
