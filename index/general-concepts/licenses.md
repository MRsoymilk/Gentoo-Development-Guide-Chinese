# 许可

所有 ebuild 必须指定`LICENSE`（请注意美国英语拼写）变量。此变量中列出的许可证名称必须与存储库`licenses/`目录中现有的文件匹配。

此变量的值应包括与软件包安装的文件有关的所有许可证。如果软件包的某些部分仅是有条件地安装，或者它们的许可证取决于 USE 标志组合，则可以在`LICENSE`中使用 USE 条件：

```bash
LICENSE="LGPL-2.1+ tools? ( GPL-2+ )"
```

如果软件包源包含未安装的其他文件，则不应列出其许可证。但是，如果在构建时使用了这些文件，则许可证不得施加任何可能阻止用户构建软件的限制。

如果应用程序是多许可证（可以使用多个许可证中的任何一个），则使用以下语法：

```bash
LICENSE="|| ( foo bar )"
```

## 许可证规定的限制

非免费许可证可能会施加其他限制，这些限制需要在 ebuild 中声明。为了正确识别此类限制，有必要分析相关许可证并根据上游软件包中包含的文件确定适用的条款。请注意，上游可能对多个产品使用相同的许可证，但某些限制不适用于所讨论的 ebuild。

如果软件包的许可证未明确允许重新分发在中找到的 distfile `SRC_URI`，则相应的 ebuild 必须必须 `RESTRICT=mirror` 防止将受影响的文件复制到 Gentoo 镜像中。在某些情况下，许可证仅允许重新分发未修改的原始存档-在这种情况下，`SRC_URI` 不得包含已修改或重新打包的上游存档，并且所有更改都必须通过在适当的 ebuild 阶段进行修补来应用。

如果许可证不允许分发使用 ebuild 构建的 Gentoo 二进制软件包，无论是否进行了源代码修改，它都必须具有 `RESTRICT=bindist`。如果根据重新分配的成本设置了限制（例如，许可证禁止出售产品），情况也是如此。

某些 EULA 可能还要求用户手动获取 distfile，在这种情况下 `RESTRICT=fetch` 是必需的。请注意，这 `RESTRICT=fetch` 意味着 `RESTRICT=mirror`。

## 确定正确的许可证

<div class="alert alert-warning">
<b>警告</b>：请勿信任GitHub或使用自动化工具猜测许可证的其他站点指示的许可证！所提供的信息可能不完整和/或不正确。例如， <a href="https://developer.github.com/v3/licenses/#get-the-contents-of-a-repositorys-license">GitHub许可证API</a>只能返回给定存储库的单个许可证。
</div>

要确定的正确值 `LICENSE`，你需要跟踪所有已安装文件的许可证。通常，输出文件（编译的可执行文件，生成的文件）的许可证由相关输入文件的许可证隐含。

查找许可证信息时，应考虑以下因素：

1. `COPYING*`以及 `LICENSE*`与软件包一起分发的文件
2. 文档中的明确声明
3. 源文件和数据文件中的明确许可通知

后一种（更具体的）选项优先于前一种。特别是，`COPYING*`文件通常作为适用许可证的硬拷贝包含在内，但许可证的确切应用及其版本在其他地方指定。

如果软件包中没有任何许可，则应与作者联系以进行声明。强烈建议不要添加没有明确许可声明的软件包。如果已经存在，则应该具有 `all-rights-reserved` 许可证，并且 `RESTRICT="bindist mirror"`。

## 检测上游许可证问题

请注意上游许可问题，并向上游报告。你可以向 Gentoo 许可证团队寻求指导。通常，最好等待上游解决此问题并发布新版本。不要添加似乎包含许可条款的违规软件包！

常见的许可证问题包括但不限于：

1. 包括没有适当版权声明的第三方代码。实际上，所有许可（特别是类似公共领域的许可）都需要注明出处，有些则需要逐字复制原始版权声明。

2. 合并不兼容的许可证。当你将使用不同许可证的多个文件组合到单个可执行文件中时，这些许可证需要兼容。例如，不可能将专有代码与 Copyleft 许可证（例如 GPL）结合使用。组合使用 GPL-2（仅）和 GPL-3 代码也是不正确的。

3. 动态链接不兼容的可执行文件。可以说，某些许可证还对可执行文件和共享库之间的动态链接施加了限制。例如，通常你不能将 GPL 可执行文件与 OpenSSL 链接。相同的限制不适用于 LGPL，并且某些项目正在为其 GPL 使用添加特定的链接例外。
4. 有关项目的许可证信息有误或不完整。上游可能指示项目使用了错误的有效许可证（例如，在 README 中。例如，上游可能指示该项目被许可为 GPL-2 +，而某些源代码文件则使用了 GPL-3 +许可证。

## GPL-x 和 GPL-x +

FSF 许可证（GPL，LGPL，AGPL，FDL）有两个变体：“vN only”和“vN or later”变体。在 Gentoo 中，后一种变体的许可证通过在前一种变体的各自许可证符号后附加一个加号（+）来表示 （例如`GPL-2+`和`GPL-2`）。

确定正确的变体通常需要在代码中查找版权声明。例如，以下版权声明表示 `GPL-2+`许可：

```bash
 * This program is free software; you can redistribute it and/or
 * modify it under the terms of the GNU General Public License as
 * published by the Free Software Foundation; either version 2 of
 * the License, or (at your option) any later version.
```

## 添加新许可证

如果软件包的许可证不在 tree 中，则必须在提交软件包之前添加许可证。添加许可证时，请尽可能使用纯文本文件（UTF-8 编码）。有些许可证是 PDF 文件而不是纯文本——仅在法律上需要这样做的情况下才应这样做。最后，你需要检查是否应将许可证添加到`$PORTDIR/profiles/license_groups` 中列出的许可证组中。有关更多信息，请参阅 [GLEP 23](https://www.gentoo.org/glep/glep-0023.html)。

通常在添加新许可证之前无需邮寄到 `gentoo-dev` 列表或 `licenses@gentoo.org`。仅当许可证被认为是“可疑的”或不确定其任何部分的含义时，才应这样做。

添加新许可证时，应检查 [SPDX 许可证列表](https://spdx.org/licenses/)中是否存在已建立的名称。但是，Gentoo 并不将软件包数据交换视为权威标准，因此有时我们可能会偏离它，例如，如果其“短标识符”过长。同样，我们通常不重命名现有的标识符。

## 进一步阅读

建议使用以下资源作为版权和许可的更多信息的来源：

- [David A. Wheeler, The Free-Libre / Open Source Software (FLOSS) License Slide](https://dwheeler.com/essays/floss-license-slide.html)
- [Richard Stallman, License Compatibility and Relicensing](https://www.gnu.org/licenses/license-compatibility.en.html)
