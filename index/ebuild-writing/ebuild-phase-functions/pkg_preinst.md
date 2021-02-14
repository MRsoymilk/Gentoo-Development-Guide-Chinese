# pkg_preinst

|          |                                  |
| :------- | :------------------------------- |
| **功能** | `pkg_preinst`                    |
| **目的** | 在 image 安装到`${ROOT}`之前调用 |
| **沙盒** | 不具备                           |
| **权限** | root                             |
| **调用** | ebuild, binary                   |

## 默认`pkg_preinst`

```bash
pkg_preinst()
{
	return
}
```

## `pkg_preinst`样例

```bash
pkg_preinst() {
	enewgroup foo
	enewuser foo -1 /bin/false /dev/null foo
}
```

## 常见的`pkg_preinst`任务

在 `pkg_preinst` 中通常要做一些事情：

- 添加用户和组。但是，由于可以在 `src_compile` 之后调用 `pkg_preinst`，因此 `pkg_setup` 是更适合用户创建的位置 —— 请参阅[用户和组](./../users-and-groups.md)。
- 修改特定系统的安装 image。使用此功能，即使从二进制文件安装，也可以进行系统特定的自定义。最有用的示例可能是智能配置文件更新——`pkg_preinst` 可以检查`${ROOT}/etc/`中的配置文件，并在`${D}/etc/`中创建智能更新的版本（请参阅[安装目标](./../../general-concepts/install-destinations.md)），而不是总是尝试安装默认配置文件（请记住[配置文件保护](./../../general-concepts/configuration-file-protection.md)）。
