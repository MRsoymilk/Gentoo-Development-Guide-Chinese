# 虚拟

虚拟只是属于`virtual`类别的软件包。他们使用其依赖项字符串指定虚拟的提供程序，并且不安装任何文件。由于它们是常规的 ebuild，因此可以有多个虚拟版本（当某个软件包可以由某些版本的其他版本提供，而不是其他版本的软件包时，这会很有帮助——有关此示例，请参见 perl 虚拟版本）。另一个区别（除了不安装任何文件外）是虚拟未定义`HOMEPAGE`和`LICENSE`变量。由于它不安装任何文件，因此它实际上没有许可证。

在添加新的虚拟之前，应在`gentoo-dev`上进行讨论。

一个虚拟的例子：

```bash
EAPI=7

DESCRIPTION="Virtual for C++ tr1 <type_traits>"
SLOT="0"
KEYWORDS="alpha amd64 arm hppa ia64 ~mips ppc ppc64 ~s390 sparc x86 ~x86-fbsd"

RDEPEND="|| ( >=sys-devel/gcc-4.1 dev-libs/boost )"
```

看起来很熟...对吗？它看起来就像普通的 ebuild。

<div class="alert alert-note">
<b>注意</b>： Gentoo存储库中禁止使用所谓的老式或<code><pre>PROVIDE</pre></code>类型虚拟。
</div>

## 虚拟软件包中的关键字

由于虚拟软件包不安装任何文件，因此它们不遵循常规的架构测试程序。相反，开发人员可以立即将虚拟的`KEYWORDS`设置为其提供者的`KEYWORDS`联合。特别是，如果为稳定包创建了新的虚拟，则该虚拟将直接提交给稳定。

例如，如果你有两个软件包：`KEYWORDS="amd64 ~x86"`的`dev-libs/liblinux` 和 `KEYWORDS="~amd64-fbsd ~x86-fbsd"`的`dev-libs/libbsd` ，生成的虚拟将具有：

```bash
KEYWORDS="amd64 ~x86 ~amd64-fbsd ~x86-fbsd"

RDEPEND="|| ( dev-libs/liblinux dev-libs/libbsd )"
```

## 虚拟与子 slot

<div class="alert alert-warning">
<b>警告</b>：仅当虚拟提供软件包括彼此兼容的ABI版本（并使用匹配的SONAME）和/或废弃了不兼容的提供程序时，本节才适用。
</div>

像常规软件包一样，虚拟可以定义用于触发其反向依赖关系重建的子 slot。为此，将为提供商的每个子 slot 创建一个新的虚拟版本，其中每个版本都包含对特定子 slot 的依赖关系。

例如，用于提供 ABI 兼容 `libfoo.so.1` 库的不同软件包的虚拟程序可能如下所示：

```bash
EAPI=6

DESCRIPTION="Virtual for libfoo.so.1"
SLOT="0/1"

RDEPEND="
    || (
        dev-libs/libfoo:0/1
        dev-libs/libfoo-alternate:0/1
        dev-libs/bigpack:0/libfoo-1+libbar-0
        dev-libs/bigpack:0/libfoo-1+libbar-1
    )
"
```

当其中一个提供程序被淘汰而取而代之的是另一个破坏 ABI 兼容性而其余部分又保持 API 兼容的提供程序时，也可以使用虚拟。在这种情况下，将创建多个虚拟版本，每个版本指定一个提供程序和一个唯一的子 slot。

例如，如果将 `dev-libs/libfoo`（`libfoo.so.0`）替换为 `dev-libs/newfoo`（`libfoo.so.1`），`virtual/libfoo-0.ebuild` 则将包含：

```bash
EAPI=6

DESCRIPTION="Virtual for libfoo.so.0"
SLOT="0/0"
RDEPEND="dev-libs/libfoo:0/0"
```

而 `virtual/libfoo-1.ebuild` 将包含：

```bash
EAPI=6

DESCRIPTION="Virtual for libfoo.so.1"
SLOT="0/1"
RDEPEND="dev-libs/newfoo:0/1"
```

<div class="alert alert-note">
<b>注意</b>： 在这种情况下软件包管理器自然会尽可能地升级到<code><pre>dev-libs/newfoo</pre></code>。因此，如果需要在两个提供商之间进行干净的选择，则此解决方案不可行。
</div>
