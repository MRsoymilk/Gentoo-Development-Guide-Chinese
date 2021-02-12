# pkg_postrm

|          |                      |
| :------- | :------------------- |
| **函数** | `pkg_postrm`         |
| **目的** | 取消合并软件包后调用 |
| **沙盒** | 不具备               |
| **权限** | root                 |
| **要求** | ebuild，binary       |

## 默认 `pkg_postrm`

```bash
pkg_postrm()
{
	return
}
```

## `pkg_postrm`示例

```bash
pkg_postrm() {
	xdg_desktop_database_update
	xdg_mimeinfo_database_update
}
```

## 常见 `pkg_postrm` 任务

`pkg_postrm` 用于卸载软件包后更新符号链接，缓存文件和其他生成的内容。
