|          |                                          |
| :------- | :--------------------------------------- |
| **函数** | `src_prepare`                            |
| **目的** | 准备源软件包并进行任何必要的修补或修复。 |
| **沙盒** | 已启用                                   |
| **权限** | user                                     |
| **要求** | ebuild                                   |

## 默认 `src_prepare`

在 EAPI 6 之前，默认实现什么都不做：

```bash
src_prepare() {
	true
}
```

从 EAPI 6 开始，`src_prepare` 函数增加了新的默认实现：

```bash
src_prepare() {
	if [[ $(declare -p PATCHES 2>/dev/null) == "declare -a"* ]]; then
		[[ -n ${PATCHES[@]} ]] && eapply "${PATCHES[@]}"
	else
		[[ -n ${PATCHES} ]] && eapply ${PATCHES}
	fi
	eapply_user
}
```

<div class="alert alert-note">
<b>注意</b>： 使用EAPI 6，你必须调用<code><pre>eapply_user</pre></code>或<code><pre>default</pre></code>定义 <code><pre>src_prepare</pre></code>！
</div>

## `src_prepare`样例

```bash
src_prepare() {
	eapply "${FILESDIR}/${PV}/${P}-fix-bogosity.patch"
	eapply "${FILESDIR}/${PV}/${P}-pam.patch"

	eapply_user

	sed -i -e 's/"ispell"/"aspell"/' src/defaults.h || die "Sed failed!"
}
```

## `src_prepare` 流程

以下小节涵盖编写 `src_prepare` 函数时经常出现的不同主题。

- [使用 epatch 和 eapply 修复](./patching-with-epatch-and-eapply.md)
- [自动包装](./autopackage.md)
