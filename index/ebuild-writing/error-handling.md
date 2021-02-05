# 错误处理

## 错误处理的重要性

合适的错误处理很重要，因为：

- Portage 尝试将损坏或不完整的软件包安装到实时文件系统*之前*，必须检测到错误。如果没有发现构建失败，一个有效的软件包将卸载。
- 当收到错误报告时，如果你确切地知道是哪个调用导致了错误，而不是仅仅知道例如`src_compile`某个地方发生了故障，那么找出问题出在哪里就容易得多 。
- 良好的错误处理和通知可以帮助减少软件包收到的错误报告的数量。

## `die` 函数

`die` 函数应用于指示致命错误并中止构建。其参数应该是要显示的消息。

尽管 `die` 没有参数即可使用，但应始终提供一条简短消息以简化错误识别。当一个函数可能在多个地方失败时，这一点尤其重要。

Ebuild 助手会在失败时自动结束。一些 eclass 提供的函数将在失败时自动结束，而其他函数则不会。如有疑问，开发人员应参考 [eclass 参考](./../eclass-reference.md)。

有时，事先显示其他错误信息会很有用。使用 `eerror` 执行此操作。请参阅[消息](./messages.md)。

使用`|| die`多多益善。

## `die` 和子 shell

<div class="alert alert-warning">
<b>警告</b>： 除非你使用的是EAPI = 7及更高版本，否则<code><pre>die</pre></code>将无法在子Shell中工作。
</div>

以下代码将无法正常工作，因为 `die`在子 shell 中：

```bash
[[ -f foorc ]] && ( update_foorc || die "Couldn't update foorc!" )
```

重写它的正确方法是使用一个 `if` 块：

```bash
if [[ -f foorc ]] ; then
	update_foorc || die "Couldn't update foorc!"
fi
```

使用管道时，会引入一个子 shell，因此以下内容是不安全的：

```bash
cat list | while read file ; do epatch ${file} ; done
```

使用输入重定向（请参阅[cat 的滥用](./../tools-reference/README.md)）可以避免此问题：

```bash
while read file ; do epatch ${file} ; done < list
```

## `assert` 函数与`$PIPESTATUS`

使用管道时，简单的条件和对`$?`的测试不会正确检测链中最后一条命令以外的任何其他内容中发生的错误。为了解决这个问题，bash 提供了`$PIPESTATUS`变量，而 portage 提供了`assert`函数来检查该变量。

```bash
bunzip2 "${DISTDIR}/${VIM_RUNTIME_SNAP}" | tar xf
assert
```

如果您需要`$PIPESTATUS`的详细信息，请参见 bash 手册页。在大多数情况下，`assert`就足够了。

## `nonfatal` 命令

```bash
src_test() {
	if ! nonfatal emake check ; then
		local a
		eerror "Tests failed. Looking for files to add to your bug report..."
		while IFS='' read -r -d $'\0' a ; do
			eerror "    ${a}"
		done < <(find "${S}" -type f -name '*.log' -print0)
		die "Make check failed"
	fi
}
```
