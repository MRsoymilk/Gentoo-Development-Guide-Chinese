# pkg_prerm

|          |                        |
| :------- | :--------------------- |
| **函数** | `pkg_prerm`            |
| **目的** | 取消合并软件包之前调用 |
| **沙盒** | 不具备                 |
| **权限** | root                   |
| **调用** | ebuild，binary         |

## 默认 `pkg_prerm`

```bash
pkg_prerm()
{
	return
}
```

## `pkg_prerm`样例

```bash
pkg_prerm() {
	# clean up temp files
	[[ -d "${ROOT}/var/tmp/foo" ]] && rm -rf "${ROOT}/var/tmp/foo"
}
```

## 常见 `pkg_prerm` 任务

`pkg_prerm` 用于清除所有文件，这些文件会阻止干净的合并。
