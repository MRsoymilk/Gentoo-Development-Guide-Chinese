# USE 标志

`USE` 标志用于控制用户可能希望选择的可选依赖项和设置。例如，`app-editors/vim` 可以有选择地构建对 `ruby` 解释器的支持，并且需要 `dev-lang/ruby` 安装它来完成此操作 —— 我们使用 ruby `USE` 标志提供此选项。在另一方面， `app-text/glark` 必定依赖 `ruby`，所以没有此 `USE` 标志。

`USE` 标志的任何组合都不应导致软件包无法构建，因为用户可以设置标志的任何组合。

软件包不应基于编译时可用的内容进行配置和链接 —— 必须覆盖任何自动检测。这通常称为依赖项是“自动的”。这很不好，因为包管理器工具无法检测到该依赖关系，并且很容易破坏它，尤其是在其他问题上。

最好通过准备构建系统补丁程序以添加自定义选项来控制相关依赖项，并为方便所有用户向上游提交此补丁程序，从而修复自动魔术性依赖项。为了避免在下游携带额外的补丁，通常可以使用特殊的构建系统选项（例如，自动工具中的缓存变量）或通过无条件地依赖于相关软件包（即强制检查始终成功）来解决自动魔术依赖项。

<div class="alert alert-note">
<b>注意</b>： USE标志的状态保存在VDB中，从那里获取它们在 <code><pre>pkg_prerm</pre></code>和<code><pre>pkg_postrm</pre></code>的值。这意味着在合并和取消合并之间设置或取消设置USE标志无效。
</div>

## 什么时候不使用 USE 标志？

尽管通常认为`USE`标志对用户有利，但是有一些避免使用它们的有效用例。编写 ebuild 时，请考虑是否为特定的条件功能添加标志，或探索以下所述的替代方案。

当软件包未进行链接时，`USE`标志的使用不应控制运行时依赖项。这样做将为软件包创建额外的配置，并重新编译磁盘上的基础文件不变。应该避免这种情况，如果需要，可以通过安装后消息将其传达给用户。

`USE`标志不得用于控制小型，非侵入式，不会引入其他构建时间依赖性或导致构建时间显着增加的安装文件。此类文件的示例包括 bash 完成文件，init.d 脚本，logrotate 配置文件，systemd 服务文件。基本原理与上述相同。而是，这些文件必须无条件安装。

对于具有多个条件程序或模块的包，可以进行类似的处理。只要导致结果产生大量`USE`标志并迫使用户花费大量时间来选择兼容的标志，并可能在选择不完全之后进行重建，请考虑减少对那些具有外部依赖性和/或较长时间的程序或模块使用标志建立时间。它们的其余部分应改为无条件构建，或由标记（例如`minimal`）控制。

您不应引入仅用于操作`CFLAGS`，`FEATURES`或用户直接配置的类似变量的 USE 标志。相反，软件包应该完全避免操纵它们，并让用户直接设置它们。常见的错误包括：

1. 使用`debug` USE 标志强制`-O0 -g`并禁用剥离。`debug`标志的正确目的是控制其他调试代码路径。用户应负责使用正确的标志和功能来保留调试信息。
2. 引入`lto`标志强制使用`-flto`。这是用户应直接在标志变量中设置的内容。
3. 使用`CPU_FLAGS_*`控制`-m*`选项。这些标志旨在控制明确要求特定 CPU 扩展的代码路径，例如单独组装。编译器生成的程序集应遵循用户的`-march`选择。

在某些极端情况下，这些规则将不适用。例如，一些上游要求用户使用特定的`CFLAGS`并拒绝针对使用其他值的构建的错误报告。在这种情况下，习惯上默认情况下会剥离标志并提供`custom-cflags`标志，以允许用户强制使用其首选标志。

## `noblah` 使用标志

避免使用`noblah`样式的`USE`标志。这些会破坏`use.mask`并为架构开发人员造成各种复杂情况。原因如下：

考虑一个名为“vplayer”的假想软件包，它可以播放视频。此软件包通过`USE`标志具有对各种声音和视频输出方法，各种视频编解码器等的可选支持。

vplayer 的可选功能之一是对'fakemedia'编解码器的支持，不幸的是，该编解码器仅以 x86 二进制文件形式提供。我们可以通过执行以下操作来解决此问题：

```bash
RDEPEND="x86? ( fakemedia? ( >=media-libs/fakemedia-1.1 ) )"
```

除非这很讨厌 —— 还要制作 AMD64 二进制文件怎么办？同样，其他架构上的用户也会看到在`emerge -pv`输出中列出的 fakemedia，即使它实际上不可用。

类似地，说 vplayer 支持通过 ALSA 编解码器作为一个选项进行输出。但是，ALSA 在 SPARC 或 Alpha 上不可用（或在编写本示例时不可用）。因此，我们可以这样做：

```bash
DEPEND="!sparc? ( !alpha? ( alsa? ( media-libs/alsa-lib ) ) )"
```

同样，这很混乱，而且 ALSA 仍然显示在`emerge -p`输出中。同样，一旦 ALSA 开始在 SPARC 上运行，则必须手动编辑执行此操作的每个 ebuild。

解决方案是`use.mask`，该文件记录在[个人文件 use.mask 文件](./../profiles/profiles-use.mask-file.md)中。每个配置文件都可以具有`use.mask`文件，该文件可用于强制禁用给定架构（或子架构或子配置文件）上的某些 USE 标志。因此，如果在每个非 x86 配置文件上都使用了`fakemedia` USE 标志，则以下内容将完全合法，并且不会破坏任何内容：

```bash
RDEPEND="fakemedia? ( >=media-libs/fakemedia-1-1 )"
```

使用非 x86 的用户在执行`emerge -pv vplayer`时会看到以下内容：

```bash
[ebuild   R   ] media-video/vplayer-1.2 alsa -blah (-fakemedia) xyz
```

要添加标志 `use.mask`，请询问相关的架构团队。

## IUSE 默认值

在`IUSE`中使用标记的名称之前添加`+`或`-`以默认将其打开或关闭。

<div class="alert alert-important">
<b>重要提示</b>：在<code><pre>IUSE</pre></code>中的标记之前添加<code><pre>-</pre></code>几乎没有用，因为它既不会覆盖用户配置（<code><pre></pre></code>make.conf），也不会覆盖配置文件默认值（<code><pre>make.defaults</pre></code>和<code><pre>package.use</pre></code>）。有关在Portage中的USE-ordering的详细信息，请参见make.conf（5）。
</div>

```bash
# Copyright 1999-2020 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2

EAPI=7

IUSE="foo +bar"
```

## 本地和全局 USE 标志

USE 标志分为本地或全局。全局 USE 标志必须满足几个条件：

- 它被许多不同的软件包使用，似乎已经同意至少 5 个。
- 它具有一般的非特定用途。

第二点很重要。如果 USE 标志对`pkg-one`的影响与它对`pkg-two`的影响大不相同，则该标志不是适合用作全局标志的候选对象。特别要注意的是，如果曾经引入过`client`和`server`USE 标志，则由于这个原因它们不能成为全局 USE 标志。

在引入新的全局 USE 标志之前，必须在 gentoo-dev 邮件列表上进行讨论。

## USE 标志说明

必须在`profiles/`目录中的`use.desc`或`metadata.xml`中描述所有 USE 标志。有关格式的说明，请参见`man portage`或这些文件中的注释。请记住要对这些文件进行排序。文件`use.local.desc`是从软件包的`metadata.xml`中的条目自动生成的，并且可以由解析 tree 的工具使用。由于`use.local.desc`是自动生成的，因此绝对不能在 tree 中进行手动编辑。有关更多信息，请参见[GLEP 56](https://www.gentoo.org/glep/glep-0056.html)。

`USE_EXPAND`标志是例外，必须在`profile/desc/`目录中进行记录。每个`USE_EXPAND`变量需要一个文件，该文件必须包含此变量可以采用的可能值的说明。请参阅这些文件中的注释以获取格式，并记住对它们进行排序。

## USE 标志冲突

有时 ebuild 的函数 USE 标志会发生冲突。检查它们并返回错误*不是*可行的解决方案。相反，你必须选择其中有冲突但更好的 USE 标志，并应警告用户使用了特定标志。

一个来自`mail-mta/msmtpebuilds`的例子。该软件包可以使用带 GnuTLS 的 SSL，带 OpenSSL 的 SSL 或完全不使用 SSL。由于 GnuTLS 比 OpenSSL 具有更多函数，因此受到青睐：

```bash
src_compile() {
	local myconf

	if use ssl && use gnutls ; then
		myconf="${myconf} --enable-ssl --with-ssl=gnutls"
	elif use ssl && ! use gnutls ; then
		myconf="${myconf} --enable-ssl --with-ssl=openssl"
	else
	myconf="${myconf} --disable-ssl"
	fi

	econf \
		# Other stuff
		${myconf}

	emake
}
```

在某些特殊情况下，上述策略会破坏反向 USE 依赖性。为了避免这种情况，ebuild 可以使用`REQUIRED_USE`指定允许的 USE 标志组合。有关其语法的说明，请参见[REQUIRED_USE](./../ebuild-writing/variables.md)部分。

例如，如果可以使用`USE="a"`或`USE="b"`或不使用两者都构建软件包`dev-libs/foo`，则首选其中一个标志将破坏依赖于`dev-libs/foo[a]`或`dev-libs/foo[b]`的软件包。因此，在这种情况下，ebuild 应该指定`REQUIRED_USE="a ? ( !b )"`。

<div class="alert alert-note">
<b>注意</b>： 为了避免强迫用户过多地管理标志， <code><pre>REQUIRED_USE</pre></code>应谨慎使用。只要有可能进行可能满足用户需求的构建，就应遵循常规策略。
</div>

## USE_EXPAND 和 ARCH USE 标志

`VIDEO_CARDS`，`INPUT_DEVICES`和`L10N`变量会自动扩展为 USE 标志。这些称为`USE_EXPAND`变量。例如，如果用户在`make.conf`中具有`L10N ="en fr"`，则 Portage 将自动设置`USE ="l10n_en l10n_fr"`。

自 Portage 2.0.51.20 起，`USE_EXPAND`列表在`profile/base/make.defaults`中设置。未经对 gentoo-dev 列表的讨论，不得对其进行修改，并且不得在任何子配置文件中对其进行修改。

当前体系结构（例如`x86`，`sparc`，`ppc-macos`）也将自动设置为 USE 标志。有关有效架构关键字的完整列表，请参见`profiles/arch.list`；有关 ​​ 格式的说明，请参见[GLEP 22](https://www.gentoo.org/glep/glep-0022.html)。

<div class="alert alert-warning">
<b>警告</b>：常见的误解是架构变量某种程度上与<code><pre>ACCEPT_KEYWORDS</pre></code>相关。但并不是。例如，在<code><pre>sparc</pre></pre>上设置<code><pre>x86</pre></pre>关键字将不会设置<code><pre>USE=x86</pre></pre>。同样，也没有<code><pre>~arch</pre></pre>USE标志，所以不要尝试<code><pre>if use ~x86</pre></pre>。
</div>
