# Ebuild 文件格式

ebuild 是在特殊环境中执行的 `bash` 脚本。文件应该是带有`.ebuild`扩展名的简单文本文件。

## 文件命名规则

ebuild 应该以`name-version.ebuild`的形式命名。

名称部分应仅包含小写的非重音字母，数字 0-9，连字符，下划线和加号字符。强烈建议不要使用大写字母，但从技术上讲这是有效的。

<div class="alert alert-note">
<b>注意</b>： 这与<a href="https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap03.html#tag_03_282">IEEE Std 1003.1-2017, section 3.282 (Portable Filename Character Set)</a>相同，除了句点字符和加号之外，其他字符使GTK+ 和朋友们保持快乐。
</div>

名称不能以连字符或加号开头，也不能以连字符结尾，后接可能误认为版本的任何内容。

版本部分更加复杂。它由一个或多个数字组成，并用句号（或句号，点或小数点）分隔（例如 `1.2.3`，`20050108`）。最终编号后面可以有一个字母（例如 `1.2b`）。该字母不应用于表示‘beta’状态——portage 将`1.2b`视为比`1.2`或`1.2a`更高的版本。

版本的后缀可以指示发行的类型。在下表中，首先将 portage 视为“最低”的版本。

| **后缀**   | **含义**           |
| :--------- | :----------------- |
| `_alpha`   | Alpha 版本（最早） |
| `_beta`    | 测试版             |
| `_pre`     | 预发行             |
| `_rc`      | 发布候选           |
| （无后缀） | 正常发行           |
| `_p`       | 补丁发布           |

这些后缀中的任何一个都可以后跟无符号整数。

这些后缀可以链接在一起，并将被迭代处理。举例说明（以下列表从最低版本到最高版本）：

- foo-1.0.0_alpha_pre
- foo-1.0.0_alpha_rc1
- foo-1.0.0_beta_pre
- foo-1.0.0_beta_p1

版本的整数部分不得超过 18 位数字。

最后，版本可能具有`-r1`形式的 Gentoo 修订号。最初的 Gentoo 版本应该没有修订后缀，第一个修订应该是`-r1`，第二个修订是`-r2`，依此类推。请参阅[Ebuild 修订](./../general-concepts/ebuild-revisions.md)。修订版本号与修补程序版本有所区别，其区别在于 Gentoo 开发人员对修订版本进行了更改，而修补程序版本是上游的新版本（快照除外，请参见下文）。

总体而言，这为我们提供了类似`libfoo-1.2.5b_pre5-r2.ebuild`的文件名 。

[EAPI 7 version commands](./variables.md)可用于操纵和提取 ebuild 版本组件。

## 快照和实时 ebuild

打包源存储库的快照时，有两种常用格式。第一个将快照视为以前版本的补丁，因此 ebuild 版本的格式为`$(last-released-version)_pYYYYMMDD`。或者，可以将快照视为即将发布版本的预发布版本，通常在预期发布但尚未发布时使用。格式为`$(upcoming-version)_preYYYYMMDD`。

所谓的*实时* ebuild（请参阅 [src_unpack 操作](./ebuild-phase-functions/src_unpack/README.md)）的策略是使用 `9999` 作为版本（或作为最后一个版本组件）。

## 二进制包

Gentoo 通常从源代码构建其软件包。例外地，可以改为提供二进制软件包（例如，如果上游不提供源代码）。这样的软件包仍应遵循正常的命名约定，并且不需要任何特殊的后缀。

如果除了基于开源的等效软件包之外还提供了二进制软件包，则为了区别起见，前者的名称应带有后缀`-bin`。示例是从源代码构建时占用大量 CPU 时间或内存等资源的软件包。

## Ebuild 标题

提交给 tree 的所有 ebuild 都应在开头有两行标题，指示版权，后跟空白行。这必须是 Gentoo 资料库顶层目录中`header.txt`内容的精确副本。

```bash
# Copyright 1999-2020 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2
```

<div class="alert alert-note">
<b>注意</b>： 标头先前包含带有CVS <code><pre>$Id$</pre></code>或<code><pre>$Header$</pre></code>关键字的第三行。根据<a href="https://bugs.gentoo.org/611234">decision of the Gentoo Council on 28 February 2017</a>决定，该行在转换为Git后被取消，并且<i>不得</i>再添加。
</div>

## 缩进和空格

ebuild 的缩进必须使用制表符完成，每个缩进级别一个制表符。每个制表符代表四个空格。制表符只能用于缩进，而不能用于字符串。

避免尾随空格：如果你的 ebuild 在提交时包含尾随或前导空格（用空格代替制表符选项以缩进），repoman 会警告你。

在可能的情况下，尽量使线条的宽度不超过 80 个位置。“位置”通常与字符相同——制表符有四个位置宽，多字节字符只有一个位置宽。

## 字符集

所有 ebuild（以及 eclass，元数据文件和 ChangeLogs）都必须使用 UTF-8 字符集。详细信息，请参见 [GLEP 31](https://www.gentoo.org/glep/glep-0031.html)。
