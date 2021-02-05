# USE 标志条件代码

通常，只有在设置（或未设置）给定 USE 标志的情况下，才必须执行特定的代码块。对于大的块，`if use foo`是最好的，或者对于反向测试，`if ! use`或`if use !foo`可以使用（前者较为常见，建议阅读以提高可读性）。对于单语句条件，`use foo && blah`（或对否定`use foo || blah`）形式通常更易读。

在较旧的代码中偶尔出现的<code><pre>if [ "`use foo`" ]</pre></code>和<code><pre>if [-n "`use foo`" ]</pre></code>形式**不得**使用。这是因为，从 portage-2.1 开始，当启用或禁用 use 标志时，'use' portage helper 不会产生任何输出，因此["`use foo`"]语句与[""]几乎完全相同。评估为假。

<div class="alert alert-note">
<b>注意</b>：<code><pre>die</pre></code> 在子 Shell 中将无法正常工作，因此应避免使用<code><pre>use foo && ( blah ; blah )</pre></code>形式的代码，而应使用适当的 if 语句。请参见 <a href="./error-handling.md">die 和 Subshel​​ls</a>。
</div>

```bash
	# USE conditional blocks...
	if use livecd ; then
		# remove some extra files for a small livecd install
		rm -fr "${vimfiles}"/{compiler,doc,ftplugin,indent}
	fi

	# Inverse USE conditional blocks...
	if ! use cscope ; then
		# the --disable-cscope configure arg doesn't quite work properly,
		# so sed it out of feature.h if we're not USEing cscope.
		sed -i -e '/# define FEAT_CSCOPE/d' src/feature.h || die "couldn't disable cscope"
	fi

	# USE conditional statements...
	use ssl && epatch "${FILESDIR}/${P}-ssl.patch"
	use sparc && filter-flags -fomit-frame-pointer

	# Inverse USE conditional statements...
	use ncurses || epatch "${FILESDIR}/${P}-no-ncurses.patch"
```

为了基于 USE 标志回显内容，通常可以使用更好的辅助函数。

保证`use`不会产生任何输出。如果需要显示输出，请使用`usev`函数；如果满足条件，它将回显 USE 标志的名称。 `useq`函数是已弃用的`use`同义词，请勿在新代码中使用它。
