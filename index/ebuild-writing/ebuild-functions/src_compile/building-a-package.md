# 构建一个软件包

`emake` 函数应用于调用 `make`。这将确保用户的`MAKEOPTS`正确使用。 `emake` 函数传递提供的任何参数，因此可以用于创建非默认目标（`emake extras`）。有时候，你可能会遇到一个非常古怪的非自动工具 `Makefile`文件，`emake`失败，但这很少见。

应使用`MAKEOPTS`中各种`-j`设置对构建进行测试， 以确保构建正确并行化。如果软件包*不能*完全并行，则应打补丁。

如果*确实*不能修补， 则应使用`emake -j1`。但是，在执行此操作时，请记住，特别是对于许多非 x86 用户而言，这严重损害了构建时间。 在某些 MIPS 和 SPARC 系统上，强制使用 `-j1` 可能会使构建时间从几分钟增加到一个小时。

## 修复编译器使用

有时，一个软件包会尝试使用一个奇怪的编译器，或者需要被告知要使用哪个编译器。在这些情况下，应使用 [toolchain-funcs.eclass](./../../../eclass-reference/toolchain-funcs.eclass.md) 中的 `tc-getCC()`函数 。还可以使用其他类似函数——在`man toolchain-funcs.eclass`中进行了记录 。

<div class="alert alert-note">
<b>注意</b>：为此目的使用变量<cdoe><pre>${CC}</pre></code>是不正确的。
</div>

有时，软件包不会使用用户的`${CFLAGS}`或`${LDFLAGS}`。必须解决此问题，因为有时这些变量用于指定关键的应用二进制接口选项。在这些情况下，应将构建脚本修改（例如，使用`sed`）以正确使用`${CFLAGS}`或`${LDFLAGS}`。

```bash
inherit flag-o-matic toolchain-funcs

src_compile() {
	# -Os not happy
	replace-flags -Os -O2

	# We have a weird build.sh to work with which ignores our
	# compiler preferences. yay!
	sed -i -e "s:cc -O2:$(tc-getCC) ${CFLAGS} ${LDFLAGS}:" build.sh \
		|| die "sed fix failed. Uh-oh..."
	./build.sh || die "Build failed!"
}
```

<div class="alert alert-note">
<b>注意</b>：当<code><pre>sed</pre></code>
与<code><pre>CFLAGS</pre></code>
或<code><pre>LDFLAGS</pre></code>一起使用时
，使用逗号或作为分隔符斜杠不安全。推荐的字符是冒号。
</div>

Portage 执行质量检查，以验证是否遵守 LDFLAGS。仅当 `LDFLAGS` 变量包含`-Wl,--hash-style=gnu`时，才启用此质量检查。（此标志只能在使用 `sys-libs/glibc`的系统上，MIPS CPU 的机器除外。）
