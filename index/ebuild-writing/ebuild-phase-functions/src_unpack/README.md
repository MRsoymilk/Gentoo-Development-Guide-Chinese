# src_unpack

|          |                |
| :------- | :------------- |
| **函数** | `src_unpack`   |
| **目的** | 提取源软件包。 |
| **沙盒** | 已启用         |
| **权限** | user           |
| **调用** | ebuild         |

## 默认 `src_unpack`

```bash
src_unpack() {
	if [[ -n ${A} ]]; then
		unpack ${A}
	fi
}
```

## `src_unpack`样例

```bash
src_unpack() {
	unpack ${P}.tar.xz
	use foo && unpack ${P}-foo-extension.tar.xz
}
```

## 解压缩

`unpack` 函数应用于解压缩 tarball，压缩文件等。不要使用 `tar`，`gunzip`手动等。

该`${A}`变量包含所有`SRC_URI`组件，但`SRC_URI` 自身内部基于 USE 的条件语句排除的组件除外。如果多个档案需要按特定顺序解压缩，通常避免使用会更简单`${A}`。

## 已知文件格式

该 `unpack` 函数可识别以下文件格式：

- `*.tar`
- `*.gz`，`*.Z`， `*.tar.gz`，`*.tgz`，`*.tar.Z`
- `*.bz2`，`*.bz`， `*.tar.bz2`，`*.tbz2`，`*.tar.bz`，`*.tbz`
- `*.lzma`， `*.tar.lzma`
- `*.xz`，`*.tar.xz`，`*.txz`
- `*.zip`，`*.ZIP`，`*.jar`
- `*.a`， `*.deb`
- `*.7z`， `*.7Z`
- `*.rar`， `*.RAR`
- `*.LHA`，`*.LHa`，`*.lha`，`*.lzh`

在 EAPI 6 和更高版本中，文件名扩展名不区分大小写。

<div>
<b>重要说明</b>： 除非解包所需的实用程序集中在系统中，否则 ebuild 必须为其指定必要的构建时间依赖性。
</div>

## `src_unpack` 动作

以下小节涵盖编写 `src_unpack` 函数时经常出现的不同主题 。

- [版本控制系统（VCS）源](./version-control-system-sources.md)
- [RPM 来源](./rpm-sources.md)
- [其他压缩格式](./other-archive-formats.md)
