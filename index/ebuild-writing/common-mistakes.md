# 常见错误

本节包含开发人员在编写 ebuild 的常见错误。

## 常见的 Ebuild 写作错误

### 无效使用`static` USE 标志

`static`USE 标志仅应用于使二进制文件使用静态链接，而不是动态链接。它不应用于使库安装静态库。而是使用`static-libs` USE 标志。

### 无效使用 `ROOT`

`ROOT`的使用仅旨在影响软件包的安装方式（即装入`ROOT`），而构建和编译不应依赖于`ROOT`。因此，不允许在`src_*`函数中使用`ROOT`。

另请参见[ROOT](./variables.md)。

### 引用可以压缩的文档文件的完整路径

当向用户输出要在哪里查找类似 INSTALL 之类文件的文件时，请不要指定完整路径，因为`PORTAGE_COMPRESS`会起作用。该文件可以使用 gzip，bzip2 或其他随机压缩工具进行压缩。因此，不要这样做：

```bash
elog "They are listed in /usr/share/doc/${PF}/INSTALL.gz"
```

类似做法：

```bash
elog "They are listed in the INSTALL file in /usr/share/doc/${PF}"
```

### 构建精简日志

编写 ebuild 时，应始终检查生成日志，因为生成系统可能会忽略 CC/CXX/LD/CFLAGS/LDFLAGS 或类似的内容，或者默认情况下会添加不需要的标志。为了进行分析并获得完整的信息，以防万一有人报告你的软件包的错误，**构建日志必须始终是冗长的**。

根据构建系统，有几种方法可以修复非详细的构建日志：

对于`cmake`基于构建的构建系统，ebuild 调用 cmake-utils_src_compile 即可，默认情况下，该 cmake-utils_src_compile 会选择 cmake-utils.eclass 变量'CMAKE_VERBOSE = 1'。如果出于某种原因直接调用 emake，则可以执行'emake VERBOSE = 1'（请注意，cmake-utils_src_compile 也会接受传递给 make 的参数）。

对于`autotools`基于基础的构建系统，你可以将'--disable-silent-rules'传递给 econf，或使用 EAPI 5（该参数会自动传递）。'emake V = 1'也应该起作用。

对于自定义的 Makefile，你通常必须编写一个补丁。尝试向上游添加诸如'V = 1'之类的选项以启用完整详细信息。

<div class="alert alert-note">
<b>注意</b>：万一遇到受影响的软件包，而该软件包使用的portage或eclasss无法控制的构建系统，则应提交一个错误（最好是带补丁程序）并阻止其跟踪程序错误＃429308。高于ebuild级别的解决方案是首选。
</div>

### -Werror 编译器标志未删除

“-Werror”是一个标志，它将所有警告变为错误，因此如果遇到任何警告，将中止编译。

**基本原理**

不建议将该标志用于发行版，在构建日志中遇到此标志时，应始终将其禁用，因为在许多情况下，该标志无缘无故地会中断，例如：

- 有关 GCC/GLIBC 版本变更的新警告，开发人员在编码时尚未意识到
- 一些 autoconf 检查将严重失败
- 库已弃用的 API 警告，尽管该 API 仍在工作/受支持
- 在鲜为人知的架构上，我们可能会比在普通架构上得到不同/更多的警告
- 随机破坏取决于开发人员在哪个发行版/体系结构/库版本/内核/用户区上测试“-Werror”

关闭“-Werror”，我们仍然会看到警告，但是没有理由说它们会导致编译失败。还要注意，portage 已经发出了有关 gcc 警告的质量检查通知，这些警告可能会导致运行时中断。

**怎么修**

要修复受影响的构建系统，你应该尝试以下方法：

- 从构建系统中删除编译器标志，例如*Makefile.am*或*configure.ac*甚至提供一个开关（对于可能基于“--disable-werror”的基于自动工具的构建系统，这对于向上游发送修补程序很有用）
- 使用 a*ppend-flags-Wno-error*（需要 flag-o-matic.eclass）; 为此，必须遵守环境标志并将其放置在构建系统标志之后；此方法不是首选方法，因为它将同时禁用所有“-Werror = specific-warning”标志，请参阅下一节

始终检查构建日志中是否确实已删除。

**特定-Werror=...标志**

GCC 可以将任何特定的警告变成错误。例如，特定的-Werror 标志将是“-Werror = implicit-function-declaration”，并且只会影响有关隐式函数声明的警告。保留这些内容是最安全的，因为将它们固定在此问题上，并且不应造成随机的构建时间中断。此外，我们可以期望上游这样做是为了避免已知的运行时错误，而不仅仅是测试其构建。但是，你应该自己检查指定的警告，或者不确定是否要询问其他开发人员。

**例外情况**

从 configure.ac 中删除“-Werror”会在极少数情况下（其中配置阶段依赖于退出代码）导致中断。请参阅 [app-emulation/open-vm-tools bug](https://sourceforge.net/p/open-vm-tools/tracker/81/)。但是即使如此，我们仍将其从生成的 Makefile 中删除。

### 标头缺失/无效/损坏

提交 ebuild 时，标头必须与 [skel.ebuild](https://gitweb.gentoo.org/repo/gentoo.git/tree/skel.ebuild)中的头标*完全*相同。

前两行必须如下所示：

```bash
# Copyright 1999-2020 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2
```

<div class="alert alert-note">
<b>注意</b>： 标头以前包含带有CVS <code><pre>$Id$</pre></code> 或<code><pre>$Header$</pre></code>关键字的第三行。根据<a href="https://bugs.gentoo.org/611234">Gentoo理事会于2017年2月28日</a>的决定，该行在转换为Git后被取消，并且<i>不得</i>再添加。
</div>

### 重新定义 P，PV，PN，PF

你永远不要重新定义这些变量。始终使用 MY_P，MY_PN，MY_PV，P0 等。有关详细信息，请参见在移植中使用的其他 ebuild。大多数 ebuild 使用 bash“参数扩展”。请阅读 bash 的手册页以了解“参数扩展”的工作方式。

顺便说一句，如果你找到重新定义它的软件包，请不要复制它。提交有关该 ebuild 的错误。

### 在 SRC_URI 和 S 中包含版本号

你应该尝试不要在 SRC_URI 和 S 中包含版本号。始终尝试使用${PV}或${P}。它使维护 ebuild 更加容易。如果版本号与压缩包/源不一致，则使用 MY_P。一个示例 dev-python/pyopenal 获取一个名为 PyOpenAL 的压缩包，因此我们将其重新定义为：

```bash
MY_P=${P/pyopenal/PyOpenAL}
SRC_URI="http://download.gna.org/pyopenal/${MY_P}.tar.gz"
S=${WORKDIR}/${MY_P}
```

### DEPEND 存在语法错误

用户提交的 DEPEND 和 RDEPEND 字段有很多错误。在编写依赖项时，需要注意以下几点。

- *始终*包含 CATEGORY。 例如，使用`>=x11-libs/gtk+-2`而不是`>=gtk+-2`。
- _不要为>=依赖项添加星号（\*）_。 例如，应为`>=x11-libs/gtk+-2`而不是 `>=x11-libs/gtk+-2*`。
- \*特定于 GTK。对于 GTK+1 应用程序始终使用=x11-libs/gtk+-1.2\*\*。
- 永远不要依赖于元软件包。 因此，不要依赖于 gnome-base/gnome，而总是依赖于像 libgnome 这样的特定库。
- 每行一个依赖项。 不要将多个依赖项放在同一行上。它使阅读变得难看并且难以遵循。

### DEPEND 不完整

这是另一个非常常见的错误。ebuild 提交者提交“可以正常工作”的 ebuild，而不检查依赖项是否正确。以下是有关如何找到正确的依赖项的一些技巧。

- _查看 configure.in 或 configure.ac_。 在此处查找软件包的检查。需要注意的是 pkg-config 检查或检查特定版本的 AM\_ \*函数。
- _查看包含.spec 的文件_。 一个很好的依赖关系指示是查看包含的.spec 文件以获取相关的 dep。但是，不要相信它们是最终的依赖关系完整列表。
- _查看应用程序/库网站_。 检查应用程序网站，了解他们建议的可能依赖关系。
- _阅读软件包的 README 和 INSTALL_。 它们通常还包含有关构建和安装软件包的有用信息。
- _记住非二进制依赖性，例如 pkg-config，doc 生成程序等_。 通常，构建过程需要一些依赖性，例如 intltool，libtool，pkg-config，doxygen，scrollkeeper，gtk-doc 等。 。

### 无效许可

用户提交 ebuild 时犯的另一个常见错误是提供了无效的许可证。例如，`GPL`不是有效的许可证。您需要指定`GPL-1`或`GPL-2`。与`LGPL`相同。确保你在`LICENSE`字段中使用的许可证在`licenses`目录中存在。作为提示，请检查源压缩包中的`COPYING`以获取许可证。如果软件包未指定使用`GPL-1`或`GPL-2`，则很有可能使用`GPL-2`。

如果您提交的软件包的许可证是唯一的，而不是在`licenses/`中，则必须在单独的文件中提交新的许可证。

### 关键字中未经测试的架构

请不要在关键字中添加其他架构，除非已经在该架构上对 ebuild 进行了测试。同样，所有新的 ebuild 都应为`~x86`或任何体系结构。确保在更改版本时，稳定的关键字都标记为`~`。

### 缺少 SLOT

确保 ebuild 中具有 SLOT 变量。如果你不打算使用它，请不要将其删除。使用：

```bash
SLOT ="0"
```

### 说明和首页错误

请检查`HOMEPAGE`变量是否正确，如果他们想了解有关该软件包的更多信息，请引导用户进入正确的页面。并确保说明适中。好的描述将用一句话描述软件包的主要功能。如果没有该软件包的主页，请将`HOMEPAGE`变量设置为`https://wiki.gentoo.org/wiki/No_homepage`。

### 错误使用空格而不是 TABS

重新格式化 ebuild 的行并不是一件有趣的事情，因为提交者没有遵循使用 TABS 而不是空格的指导。因此，*请*使用制表符！

### 尾随空格

我经常对此感到有罪。请记住在您的 ebuild 上运行 repoman，以便它可以告诉您行尾或空行处是否有尾随空格。

### 添加冗余 S=${WORKDIR}/${P}

如果`S=${WORKDIR}/${P}`，则不应将其添加到 ebuild 中。这已经暗示了，仅当它不是`${WORKDIR}/${P`}时，才应添加它。

### 缺少文档

如果您的软件包包含文档，请确保使用`dodoc`或在`/usr/share/doc/${PF}`中进行安装。记住在运行`dodoc/doins`时检查错误。

如果软件包文档很大或需要构建其他依赖项，则应使用`doc` USE 标志将其设置为可选。如果文档很小，并且不需要其他依赖项（例如`README`文件），请无条件安装。

### 屏蔽不支持/损坏的 USE 标志

例外情况是，软件包可能具有不受支持的 USE 标志（破损的 USE 标志）（使用*vanilla 或 custom-cflags*可能会发生）。然后，至少在 ebuild 处在稳定分支时，必须对该 ebuild 屏蔽 USE 标志（通常在*profile/base/package.use.mask*中）。

## 常见的 Ebuild 提交错误

### 介绍

请按照[添加新的 Ebuild](./../ebuild-maintenance/adding-a-new-ebuild.md)教程正确提交 ebuild。

### 压缩包的 ebuild

请不要将 ebuild 或补丁作为压缩包附加。审查时避免了额外的操作。

### 内联 Ebuild

不要将 ebuild 剪切并粘贴到 bugzilla 的注释字段中。

### 没有关于软件包是什么的描述

请让我们知道软件包是什么，如果有的话，请使用应用程序的主页填写 URL。

### 软件包更新，但不说明已更改的内容

如果您提交软件包更新，请确保说明您对 ebuild 进行了哪些更改。例如，如果软件包引入了新功能/选项，而您使用了 USE 标志，则将其列出在您的错误中。不要让我们去寻找它。

提交差异而不是整个 ebuild 的差异是明智的。生成它的最佳方法是：

```bash
$ diff -u some-package-0.1.0.ebuild some-package-0.2.0.ebuild > ~/some-package-0.2.0.diff
```

### 提交版本变更的未更改 ebuild

如果要以可移植的方式提交软件包的新版本，请确保现有的 ebuild 可以正常工作，并确保所做的更改已包含在新的 ebuild 中（例如添加的文档）。如果对先前版本的 ebuild 不需要进行任何更改，然后不要附加 ebuild。只需在错误报告中这样说，即您复制了 ebuild 并验证了该软件包可以正常工作并安装。
