# src_compile

| **函数** | `src_compile` |
| :------- | :------------ |
| **目的** | 生成软件包。  |
| **沙盒** | 已启用        |
| **权限** | user          |
| **要求** | ebuild        |
| **默认** | src_compile   |

## 默认`src_compile`

```bash
src_compile() {
	if [ -f Makefile ] || [ -f GNUmakefile ] || [ -f makefile ]; then
		emake || die "emake failed"
	fi
}
```

## `src_compile`样例

```bash
src_compile() {
	use sparc && filter-flags -fomit-frame-pointer
	append-ldflags -Wl,-z,now

	emake
}

```

<div class="alert alert-note">
<b>注意</b>： 你还需要继承 <a href="../../../eclass-reference/flag-o-matic.eclass.md">flag-o-matic</a> eclass才能使用该<code><pre>append-ldflags</pre></code>函数。
</div>

## `src_compile` 过程

以下小节涵盖编写 `src_compile` 函数时经常出现的不同主题 。

- [配置构建环境](./configuring-build-environment.md)
- [建立一个包裹](./building-a-package.md)
- [没有构建系统](./no-build-system.md)
