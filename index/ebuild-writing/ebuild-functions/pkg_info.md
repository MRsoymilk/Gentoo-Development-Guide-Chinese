# pkg_info

|          |                      |
| :------- | :------------------- |
| **函数** | `pkg_info`           |
| **目的** | 显示有关软件包的信息 |
| **沙盒** | ?                    |
| **权限** | root                 |
| **要求** | ebuild               |

## 默认 `pkg_info`

```bash
pkg_info()
{
	return
}
```

## `pkg_info`样例

```bash
pkg_info() {
	"${ROOT}"/usr/bin/mythfrontend --version
}
```

## `pkg_info`注意事项

软件包管理器显示有关软件包的信息时，将调用此阶段。

对于未安装的软件包，软件包管理器还可以调用`pkg_info`函数。 Ebuild 书写应注意，依赖项可能不可用。
