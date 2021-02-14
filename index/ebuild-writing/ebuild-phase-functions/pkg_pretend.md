# pkg_pretend

|          |                                        |
| :------- | :------------------------------------- |
| **函数** | `pkg_pretend`                          |
| **目的** | 在依赖性计算期间对软件包进行完整性检查 |
| **沙盒** | ?                                      |
| **权限** | root                                   |
| **调用** | ebuild, binary                         |
| **EAPI** | 4                                      |

## 默认 `pkg_pretend`

```bash
pkg_pretend()
{
	return
}

```

## `pkg_pretend`样例

```bash
pkg_pretend() {
	if use kernel_linux ; then
		if [[ -e "${ROOT}"/usr/src/linux/.config ]] ; then
			if kernel_is lt 2 6 30 ; then
				CONFIG_CHECK="FUSE_FS"
				ERROR_FUSE_FS="this is an unrealistic testcase..."
				check_extra_config
			fi
		fi
	fi
}

}
```

## `pkg_pretend`注意事项

在运行主阶段功能序列之前，可以使用`pkg_pretend`阶段进行完整性检查（这意味着该阶段在软件包管理器计算依赖性之后并在安装它们之前执行）。此阶段通常检查内核配置，并且在需要时可能`error`并`die`。

<div class="alert alert-important">
<b>重要</b>：不能保证在调用此阶段时已安装ebuild的依赖项。
</div>

<div class="alert alert-important">
<b>重要</b>：正如<code><pre>pkg_pretend</pre></code>在主阶段函数序列中未调用的那样，不能保证节省环境。
</div>
