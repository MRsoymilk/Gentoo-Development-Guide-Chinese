# Gentoo DevBook XML 指南

## DevBook XML 设计目标

DevBook XML 语法轻巧但易于表达，因此易于学习，还提供了创建 Web 文档所需的所有函数。标签数量保持在最低限度 —— 刚好是那些我们需要的。这样可以轻松地将指南转换为其他格式，例如 DocBook XML/SGML 或可用于 Web 的 HTML。

目标是使*创建和转换*DevBook XML 文档变得容易。

## Devbook XML

### 基本结构

让我们开始学习 DevBook XML 语法。我们将从 DevBook XML 文档中使用的初始标签开始：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<guide self="appendices/devbook-guide/">
<chapter>
<title>Gentoo DevBook XML Guide</title>
```

在第一行，我们看到了必需的标签，该标签将其标识为 XML 文档。接下来，有一个`<guide>`标签 —— 整个文档包含在 一 `<guide> </guide>`对中。它的 `self` 属性必须指向文档从根节点开始的相对路径。在上面的示例中，路径为`appendices/devbook-guide/`。根节点本身是一个例外，而根节点本身具有`<guide root="true">`。

接下来，有一个`<chapter>`标签。每个文档必须只有一章。`<title>`用于设置整个文档的标题。

<div class="alert alert-important">
<b>重要提示</b>：<code><pre>&lt;title&gt;</pre></code>元素不能包含任何换行符。确保它在一行中，包括其开始和结束标记。
</div>

当然，所有标签都必须关闭，因此文档的结尾为：

```xml
</chapter>
</guide>
```

### 各节和小节

一旦指定了初始标签，就可以开始添加文档的结构元素了。各章分为几节；每个部分可以包含零个或多个子部分，其中可以包含零个或多个子子部分。节，小节和小节元素必须具有标题。这是一个带有一个小节的示例节，由一个段落组成：

```xml
<section>
<title>This is my section</title>
<subsection>
<title>This is subsection one of my section</title>
<body>

<p>
This is the actual text content of my subsection.
</p>

</body>
</subsection>
</section>
```

在上面，我通过向`<title>`元素添加子元素来设置节标题`<section>`。然后，我通过添加`<subsection>`元素创建了一个小节。如果你查看`<subsection>`元素的内部 ，将会看到它具有两个子元素 —— `<title>`和 `<body>`。尽管`<title>`并不是什么新鲜事物，但是`<body>` —— 它包含此特定小节的实际文本内容。我们将稍等一下`<body>`元素中允许使用的标签。

<div class="alert alert-note">
<b>注意</b>： <code><pre>&lt;chapter&gt;</pre></code>，<code><pre>&lt;section&gt;</pre></code>，<code><pre>&lt;subsection&gt;</pre></code>和<code><pre>&lt;subsubsection&gt;</pre></code>元素包含一个<code><pre>&lt;body&gt;</pre></code>和/或任何数目的下一个较低级别的部分元件。不允许跳过级别，例如，小节不能直接位于章节的下方。
</div>

### 包括子文件

该手册以 tree 状组织。每个目录包含一个文档，使用`<include href="foo/"/>` 标签可以包含多个子文档。请注意，`href` 值中的尾部斜杠是必需的。

可以使用生成目录`<contentsTree>`。通常，此标记将是其自己的节主体中的唯一元素，如以下示例所示：

```xml
<section>
<title>Contents</title>
<body>
<contentsTree/>
</body>
</section>
```

## 一个`<body>`例子

现在，该学习如何标记实际内容了。这是示例`<body>`元素的 XML 代码：

```xml
<p>
This is a paragraph. <c>/etc/passwd</c> is a file.
<uri>https://forums.gentoo.org/</uri> is my favorite website.
Type <c>ls</c> if you feel like it. I <e>really</e> want to go to sleep now.
</p>

<pre>
This is text output or code.
# this is user input
</pre>

<codesample lang="sgml">
Make HTML/XML easier to read by using selective emphasis:
&lt;foo&gt;bar&lt;/foo&gt;
</codesample>

<note>
This is a note.
</note>

<important>
This is important.
</important>

<warning>
This is a warning.
</warning>
```

现在，这是`<body>`上面元素的呈现方式：

这是一段。`/etc/passwd` 是一个文件。 [https://forums.gentoo.org/](https://forums.gentoo.org/)是我最喜欢的网站。如果你喜欢就输入`ls`。我现在*真的*很想睡觉。

```bash
This is text output or code.
# this is user input
```

```bash
Make HTML/XML easier to read by using selective emphasis:
<foo>bar</foo>
```

<div class="alert alert-note">
<b>注意</b>： 这是一个注意事项。
</div>

<div class="alert alert-important">
<b>重要</b>：这很重要。
</div>

<div class="alert alert-warning">
<b>警告</b>：这是警告。
</div>

### `<body>`标签

在上一节中，我们引入了许多新标签 —— 这是你需要知道的。`<p>`（段）， `<pre>` （预格式化的块）， `<codesample>`（代码块）， `<note>`，`<important>`和`<warning>`标签都可以包含文本的一个或多个行。除了`<figure>`， `<table>`，`<ul>`，`<ol>` 和`<dl>`元素（我们将在稍后介绍），这些都是应该立即出现一个内唯一的标签`<body>`元素。另一件事 —— 这些标签不应堆叠 —— 换句话说，不要将`<note>`元素放在元素`<p>`内。你可能会猜到，`<pre>`和`<codesample>`元素完全保留其空白，使其非常适合代码摘录：

```xml
<pre>
# uptime
16:50:47 up 164 days,  2:06,  5 users,  load average: 0.23, 0.20, 0.25
</pre>
```

### `<c>`，`<b>`和`<e>`

`<c>`元素用于标记*命令或用户输入*。可以认为`<c>`是一种提醒读者注意的内容，他们可以输入这些内容来执行某种操作。例如，本文档中显示的所有 XML 标签都包含在一个`<c>`元素中，因为它们表示用户可以键入的内容而不是路径。通过使用`<c>`元素，你将帮助读者快速识别需要键入的命令。此外，由于`<c>`元素已经与常规文本发生偏移，_因此几乎不需要在用户输入中用双引号引起来_。例如，请勿引用“`<c>`”，就像我在这句话中所做的那样。避免使用不必要的双引号使文档更易读—美观！

正如你可能已经猜到的，`<b>`它用来**加粗**一些文本。

`<e>`用于强调单词或短语；例如：我*确实*应该更频繁地使用分号。如你所见，此文本与常规段落类型有所偏离，以强调。这有助于给你的散文更多的*冲击力*！

### 代码示例和颜色编码

`<pre>`标记不支持任何语法高亮。当你需要语法高亮时，请将`<codesample>`标记与一个 `lang` 属性一起使用 —— 通常，您希望将此设置为`“ebuild”`以高亮 ebuild 代码段的语法。当前，支持以下语言：

- C
- ebuild
- make
- m4
- sgml

<div class="alert alert-note">
<b>注意</b>： 请记住，所有前导和尾随空格以及<code><pre>&lt;codesample&gt;</pre></code>块中的换行符 将出现在显示的html页面中。
</div>

样本`<codesample lang="c">`块：

```C
#include <stdio.h>

main()
{
	/* This is a comment */
	printf("Hello, world!\n");
}
```

你还可以指定 `numbering="lines"`启用行编号，如以下示例所示：

```bash
01: # Copyright 1999-2020 Gentoo Authors
02: # Distributed under the terms of the GNU General Public License v2
03:
04: EAPI=7
05:
06: DESCRIPTION="MicroGnuEmacs, a port from the BSDs"
07: HOMEPAGE="https://homepage.boetes.org/software/mg/"
08: SRC_URI="https://github.com/hboetes/${PN}/archive/${PV}.tar.gz -> ${P}.tar.gz"
09:
10: LICENSE="public-domain"
11: SLOT="0"
12: KEYWORDS="alpha amd64 arm hppa ppc ~ppc64 sparc x86"
13:
14: RDEPEND="sys-libs/ncurses:0=
15: 	>=dev-libs/libbsd-0.7.0"
16: DEPEND="${RDEPEND}"
17: BDEPEND="virtual/pkgconfig"
18:
19: src_install()  {
20: 	dobin mg
21: 	doman mg.1
22: 	dodoc README tutorial
23: }
```

### `<uri>`

`<uri>`标记用于指向 Internet 上的文件/位置。它有两种形式 —— 第一种形式可以在正文中显示实际 URI 时使用，例如指向[https://forums.gentoo.org/](https://forums.gentoo.org/)的链接。要创建此链接，我输入了`<uri>https://forums.gentoo.org/</uri>`。另一种形式是当您要将 URI 与其他文本（例如[Gentoo 论坛](https://forums.gentoo.org/)）相关联时。要创建此链接，我输入了`<uri link ="https://forums.gentoo.org/"> Gentoo Forums</uri>`。

请避免[W3C](https://www.w3.org/QA/Tips/noClickHere)建议的[单击此处综合症](https://en.wikipedia.org/wiki/Click_here)。

### 图像

这是将图形插入文档的方法 —— `<figure link="mygfx.png" short="my picture" caption="my favorite picture of all time"/>`。`link`属性指向实际的图形图像，`short`属性指定简短描述（当前用于图像的 HTML `alt`属性）和标题。不太困难:)我们还支持标准 HTML 样式的`<img src ="foo.gif"/>`标记，用于添加不带标题，边框等的图像。

### 表格

DevBook XML 支持类似于 HTML 的简化表语法。要创建表，请使用`<table>`标记。用`<tr>`标记开始一行。但是，对于插入实际的表数据，我们*不支持* HTML `<td>`标记。相反，如果要插入标头，请使用`<th>`；如果要插入普通信息块，请使用`<ti>`。您可以在可以使用`<ti>`的任何位置使用`<th>` —— 无需`<th>`元素仅出现在第一行。

此外，表标题（`<th>`）和表项目（`<ti>`）都接受`colspan`和`rowspan`属性，以将其内容跨行，跨列或跨这两者。

此外，表单元格（`<ti>`&`<th>`）可以与`align`属性右对齐，左对齐或居中。

<table class="table">
  <tbody><tr>
    <th colspan="4" style="text-align:center">这个标题跨越4栏</th>
  </tr>
  <tr>
    <th rowspan="6">该标题跨越6行</th>
    <td>条目 A1</td>
    <td>条目 A2</td>
    <td>条目 A3</td>
  </tr>
  <tr>
    <td>条目 B1</td>
    <th colspan="2" style="text-align:center" rowspan="2">块状2x2标题</th>
  </tr>
  <tr>
    <td>条目 C1</td>
  </tr>
  <tr>
    <td colspan="3">条目 D1..D3</td>
  </tr>
  <tr>
    <td rowspan="2">条目 E1..F1</td>
    <td colspan="2">条目 E2..E3</td>
  </tr>
  <tr>
    <td colspan="2">条目 F2..F3</td>
  </tr>
</tbody></table>

### 列表

要创建有序或无序的列表，只需使用 XHTML 风格 `<ol>`，`<ul>`和`<li>`标签。列表只能出现在`<body>`和`<li>`标记内，这意味着你可以在列表内包含列表。不要忘记你正在编写 XML，并且必须闭合所有标签，包括列表项，这与 HTML 不同。

支持定义列表（`<dl>`）。请注意，定义术语标签（`<dt>`）不接受任何其他块级标签，例如段落或警告。定义列表包括：

`<dl>`

> 定义列表标签包含（**D**efinition **L**ist）

`<dt>`

> 定义术语标签（**D**efinition **T**erm）

`<dd>`

> 和定义数据标签。（**D**efinition **D**ata）

从 [w3.org](https://www.w3.org/TR/REC-html40/struct/lists.html) 复制的以下示例 显示列表也可以嵌套，并且可以一起使用不同的列表类型：

**这些成分：**

- 100 克面粉
- 10 克糖
- 1 杯水
- 2 个蛋
- 椒盐

**步骤：**

- 彻底混合干成分。
- 倒入湿的成分。
- 混合 10 分钟。
- 在 300 度下烘烤一小时。

**笔记：**

> 可以通过添加葡萄干来改善配方。

### 文档内参考

DevBook XML 使使用超链接引用文档的其他部分变得非常容易。您可以创建一个链接指向其它章节，例如通过键入`<uri link="::ebuild-writing/file-format/">Ebuild File Format</uri>`，例如，即两个冒号后跟相对根节点的路径来创建指向[Ebuild 文件格式](./../ebuild-writing/ebuild-file-format.md)的链接，要引用另一章中的部分，例如[First Ebuild](./../quickstart-ebuild-guide.md)，键入`<uri link="::quickstart/#First Ebuild">First Ebuild</uri>`。

如果要将链接目标的章节（或节等）标题用作链接文本，则可以使用空的`<uri>`元素。实际上，我可以用更紧凑的形式编写上面的两个示例：`<uri link="::ebuild-writing/file-format/"/>`和<`<uri link="::quickstart/#First Ebuild"/>`分别渲染为[Ebuild 文件格式](./../ebuild-writing/ebuild-file-format.md)和[First Ebuild](./../quickstart-ebuild-guide.md)。

## 编码风格

由于所有 Gentoo 文档都是共同努力的结果，并且有很多人很可能会更改现有文档，因此需要一种编码样式。编码样式包含两个部分。第一个是关于内部编码 —— XML 标签的放置方式。第二个关于内容-如何不使读者感到困惑。

接下来介绍这两个部分。

### 内部编码样式

**新的一行**必须放置在*每个* DevBook XML 标签之后（两个都作为结束），除了： `<title>`， `<th>`，`<ti>`，`<li>`， `<dt>`，`<dd>`， `<b>`，`<c>`，`<e>`，`<d/>`， `<uri>`。

**空行**必须放置在每个`<body>`（仅开始标签）之后和每个 `<section>`之前，`<p>`，`<pre>`， `<codesample>`，`<figure>`，`<table>`，`<ul>`，`<ol>`，`<dl>`，`<note>`， `<important>`和`<warning>`（仅开始标签）。

**换行**限制在 80 个字符，`<pre>`和`<codesample>`除外。仅当没有其他选择（例如，URL 超过最大字符数）时，你才可以偏离此规则。然后，只要出现第一个空白，编辑器就必须换行。你应该尝试将`<pre>`和`<codesample>`元素呈现的内容保持在 80 列之内，以帮助控制台用户。

**缩进**不得使用，除了父 XML 标记为`<tr>`（来自`<table>`） `<ul>`，`<ol>`和的 XML 结构之外，。如果使用缩进，则每个缩进必须为两个空格。这意味着没有制表符，也没有更多的空格。此外，DevBook XML 文档中不允许使用制表符。

如果自动换行发生在`<ti>`，`<th>`， `<li>`或`<dd>`结构，缩进必须用于内容。

缩进的示例是：

```xml
<table>
<tr>
  <th>Foo</th>
  <th>Bar</th>
</tr>
<tr>
  <ti>This is an example for indentation</ti>
  <ti>
    In case text cannot be shown within an 80-character wide line, you
    must use indentation if the parent tag allows it
  </ti>
</tr>
</table>

<ul>
  <li>First option</li>
  <li>Second option</li>
</ul>
```

**开始标签**仅具有单个属性不能在行之间分割。例如，请勿在`<uri`及其`link`属性之间放置换行符。在`<uri>`标记之前换行。

**属性**与属性，“=”标记，属性值之间不能有空格。举个例子：

```xml
Wrong  :     <uri link = "https://forums.gentoo.org/">
Correct:     <uri link="https://forums.gentoo.org/">
```

## 外部编码风格

在表（`<table>`）和列表（`<ul>`， `<ol>`和`<dl>`）内部，除非使用多个句子，否则不应使用句点（“.”）。在这种情况下，每个句子都应以句号（或其他阅读标记）结尾。

每个句子，包括表格和列表中的句子，都应以大写字母开头。

```xml
<ul>
  <li>No period</li>
  <li>With period. Multiple sentences, remember?</li>
</ul>
```

尽量使用`<uri>`的 link 属性。换句话说，[Gentoo 论坛](https://forums.gentoo.org/)优于[https://forums.gentoo.org/](https://forums.gentoo.org/)。
