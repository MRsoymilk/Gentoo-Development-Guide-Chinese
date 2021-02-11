# 配置构建环境

有时有必要在运行之前操纵用户构建环境的某些方面`./configure`。该`flag-o-matic` eclass 的是操作的最佳选择`CFLAG`S，`LDFLAGS` 以及诸如此类。

<div class="alert alert-note">
<b>注意</b>： 除非另有说明，否则在<code><span>CFLAGS</span></code>上运行的任何函数在<code><span>CXXFLAGS</span></code>上也能运行。
</div>

Ebuild 不能简单地忽略用户`CFLAGS`，`CXXFLAGS`或`LDFLAGS` —— 请参阅 [不过滤变量](./../../../general-concepts/user-environment.md)。

## 标志过滤准则

如果软件包破坏了合理的范围 `CFLAGS`，则最好在收到错误报告时过滤出有问题的标志。合理的 `CFLAGS` 是 `-march=`，`-mcpu=`，`-mtune=`（取决于弓）`-O2`，`-Os` 和`-fomit-frame-pointer`。请注意， `-Os` 通常应将其替换为`-O2` 而不是完全剥离。这个`-fstack-protector` 旗帜可能也应该在这个组中，尽管我们坚强的团队声称这个旗帜永远不会破坏任何东西。

该`-pipe` 标志不会影响生成的代码，因此在这里并没有实际意义，但是在全局范围内进行设置是明智的。

如果一个软件包被其他（疯狂）破坏 `CFLAGS`，则可以通过 **WONTFIX** 来关闭该错误，这建议用户选择更明智的 global ，这是完全可以的 `CFLAGS`。同样，如果你怀疑错误是由 insane 引起的 `CFLAGS`，则 **INVALID** 解决方案是合适的。

以下所有条件均假定 ebuild `inherit flag-o-matic` 在正确的位置有一行。

## 简单标志剥离

该 `filter-flags` 函数可用于从中删除特定标志 `{C,CPP,CXX,CCAS,F,FC,LD}FLAGS`。可以提供多个参数。每个都是要删除的标志的名称。

```bash
	# -fomit-frame-pointer leads to nasty broken code on sparc thanks to a
	# rather icky asm function
	use sparc && filter-flags -fomit-frame-pointer
```

有一个 `filter-ldflags` 函数与等效 `LDFLAGS`。

如果已知某个软件包非常 `CFLAGS` 敏感，则该 `strip-flags` 函数将删除大多数标志，仅保留最少的保守标志集。

```bash
	# Our app hates screwy flags
	strip-flags
```

## 标志替换

要将标志替换为其他标志，请使用 `replace-flags`。最常用的是用`-O2` 取代`-Os` （或用`-O2` 取代`-O3`，由你决定）。

```bash
	# Seems to have issues with -Os, switch to -O2
	replace-flags -Os -O2
```

还有一个名为`replace-cpu-flags` 的特殊函数用来更换 CPU（ `-mtune`，`-mcpu`，`-march`） 指定的标志。最后一个参数是要使用的标志。先前的参数是要替换的标志。

```bash
	# Can't use ultrasparc or ultrasparc3 code, drop to v9
	replace-cpu-flags ultrasparc ultrasparc3 v9
```

## 添加额外标志

有时有必要添加额外的 `CFLAGS` 或 `LDFLAGS`。`append-flags` 和 `append-ldflags` 函数都可以在这里使用。

```bash
	# If we're using selinux, we need to add a -D
	use selinux && append-flags "-DWITH_SELINUX"

	# Secure linking needed, since we're setuid root
	append-ldflags -Wl,-z,now
```

完整参考，请参见 [flag-o-matic.eclass](./../../../eclass-reference/flag-o-matic.eclass.md)。
