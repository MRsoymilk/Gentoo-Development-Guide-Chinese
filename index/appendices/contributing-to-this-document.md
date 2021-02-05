# 对本文档做贡献

非常欢迎你为本文档做出贡献。无论你是发现拼写错误还是撰写了全新的文章，最好的联系方式都是通过 Bugzilla 的[bugoo.gentoo.org](https://bugs.gentoo.org/) 进行联系。

编辑保留根据自己的意愿修改提交内容的权利。当然，任何实质性更改都将首先与提交者讨论 —— 除非明确要求，否则将不讨论较小的错字更正和格式修复。

本文档是根据 [知识共享署名-相同方式共享 4.0 国际许可](https://creativecommons.org/licenses/by-sa/4.0/)许可的。如果这是一个问题，请不要提交任何东西。

本文档是使用 DevBook XML 构建系统生成的。你可以从 Git 下载系统快照以及相关的 XML 文件，请参阅 [以下部分](https://devmanual.gentoo.org/appendices/contributing/index.html#where-to-find-the-sources)。如果你只想使用纯文本，那也很好 —— 格式设置可以很容易地由其他人（即我们）完成。

## 在哪里找到资源

目前，资源托管在 [git.gentoo.org](https://gitweb.gentoo.org/proj/devmanual.git) 上。对于当前的 Gentoo 开发人员，访问位于 `git+ssh://git@git.gentoo.org/proj/devmanual.git`。非开发人员可以通过克隆存储库 `git clone git://anongit.gentoo.org/proj/devmanual.git`。源码也托管在 [GitHub](https://github.com/gentoo/devmanual) 上， 供那些希望使用拉取请求提交补丁的人使用。

要构建开发手册，只需在存储库的顶层目录中运行`make`即可。在整个文档的某些图中，你需要 `xsltproc`（`dev-libs/libxslt`）进行 `XML` 到 `HTML` 的转换，`xmllint`（`dev-libs/libxml2`）进行验证，`rsvg-convert`（`gnome-base/librsvg`）进行 SVG 到 PNG 转换。

要检查文档的 XML 是否有效，请在顶级目录中运行`make validate`，该目录将使用`xmllint`验证所有 XML 文件。

## DevBook XML 快速入门

DevBook XML 很大程度上基于 GuideXML，并且许多标签是相似的，即使不相同。布局上的主要区别在于，使用层次结构 tree 系统可以使大规模出版物的生产和管理更加容易。在开始之前，你确实应该首先深入了解 [DevBook XML 指南](./gentoo-devbook-xml-guide.md)。

## 与 GuideXML 的区别

**缩进**

> 在需要时缩进 —— 你不应缩进任何节流元素，例如，`<subsection>`。而缩进表，列表和定义列表。 *不要*在普通段落块中缩进文本。

**代码样本**

> 如果`<pre>`不需要高亮显示语法，则可以使用普通的 GuideXML 标记。当你需要语法高亮显示时，请使用`<codesample>`标记以及一个 `lang` 属性 —— 通常你希望将其设置为`ebuild`语法高亮显示 ebuild 代码段。

**层次结构**

> 整个文档被组织成一棵 tree。每个目录可以包含一个文档。每个文档可以使用该`<include>`标志继承多个子文档 。你**必须**确保 `self` 每个文档中的标签都正确指向根节点上文档的相对路径，以便 tree 遍历算法正常工作。

## 风格准则

- 本文档使用英式英语。欢迎使用其他英语提交，但可能会更正其拼写。
- 应该使用第三人称形式而不是第一形式。
- 这不是正式文件。写作风格旨在专业但可读。
- 当使用句子中的连字符作为标点符号时 —— 例如这样 `——` 请使用空格，后跟`<d/>`标签。构建系统将自动将其转换为适当的 Unicode 长破折号。
