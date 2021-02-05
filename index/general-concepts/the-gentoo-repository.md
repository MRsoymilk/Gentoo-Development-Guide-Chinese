# Gentoo 存储库

Gentoo 存储库的基本布局如下：

- 类别，例如`app-editors`，`sys-apps`
  - 类别元数据，例如`app-admin/metadata.xml` —— 软件包目录，例如`app-editors/vim` —— 软件包元数据，例如`app-editors/vim/metadata.xml` —— 软件包 Manifest，例如`app-editors/vim/Manifest`
  - ebuild，例如 `app-editors/vim/vim-6.3.068.ebuild`，`app-editors/vim/vim-7.0_alpha20050326.ebuild` —— 软件包文件目录，例如 `app-editors/vim/files`
    - 小补丁和其他杂项文件，例如 `app-editors/vim/files/vim-completion`
- eclass 目录，`eclass/`
- 许可证目录，`licenses/`
- 元数据目录，`metadata/`。列出的大多数内容都没有直接保存在主 git tree 中，而是作为[从 Git 到 RSYNC](./git-to-rsync.md) 流程的一部分自动生成或包含在其他存储库中。
  - 各种控制文件，例如 `layout.conf`，`timestamp` 和 `projects.xml`。主 git tree 中仅存在 `layout.conf`。
  - `metadata/dtd/`和`metadata/xml-schema/`中的 XML 验证文件。
  - `metadata/glsa/`中的安全公告以及`metadata/news/`中的新闻项
- 个人文件 目录，`profiles/`
  - 各种控制和文档文件，例如`categories`，`info_pkgs`，`info_vars`，`package.mask`，`use.desc`
  - 更新目录，`profiles/updates/`
    - 更新文件，例如 `profile/updates/1Q-2005`
  - 主要的 profile 子目录
- 脚本目录，scripts/

## tree 中属于什么？

**不属于**tree 的事物：

- 大补丁
- 非文本文件
- 天线宝宝的照片
- 名称中包含`[A-Za-z0-9 ._ +-]`以外字符的文件
- 名称以点，连字符或加号开头的文件

distfile 的命名规则较为宽松，但出于互操作性，其文件名仅限于可打印的 ASCII 范围（不包括 SPACE），即 `U+0021` 至 `U+007E`（另请参阅 [GLEP 31](https://www.gentoo.org/glep/glep-0031.html#suitable-characters-for-file-and-directory-names)）。也应避免在 Bash 或 `SRC_URI` 中具有特殊含义的任何字符。如有必要，可以使用`->`语法[重命名](./../ebuild-writing/variables.md)上游文件。

在软件方面，通常应满足以下所有条件，以便将软件包包含在 tree 中：

**积极合作上游**

> 如果软件包在上游没有开发或维护，则很难解决问题。如果软件包没有上游活动组件，则将软件包添加到 tree 中的开发人员必须确保他们能够解决可能出现的任何问题。
> 有时，上游可能有一个不希望将其软件包包含在 tree 中的原因。这应该得到尊重。

**合理稳定**

> 超实验性的东西不要放在 tree 上。如果必须提交它们，请考虑使用 `package.mask`，直到一切稳定下来，或者最好将它们用作 overlay ebuild。

**合理有用**

> 不必因为某些用户要求而在 tree 中包括“Joe 的 1337 XMMS Skinz Collection”或“ Hans 的超酷超快文件系统”。只要实际可能有用的东西。

**正确打包**

> 如果某些内容仅以实时 CVS 或易懂的自动打包格式提供，请在上游可以提供不错的源代码包之前将其打包在内。同样，避免使用没有适当构建系统（在相关情况下）的东西——它们维护起来非常棘手。

**允许打补丁和分发**

> 如果我们自己不能修补软件包，那么最终我们将完全依赖上游提供支持。这可能是有问题的，特别是如果上游修复速度很慢的话。我们不希望遇到无法稳定关键软件包的情况，因为我们仍在等待闭源供应商采取行动。
> 同样，不能制作自己镜像和分发压缩包使得我们完全依赖上游镜像。经验表明，这些文件通常极其不可靠，文件会随机更改，移动或消失。

**可工作版本**

> 如果您没有*可用的*ebuild，请不要包含它。

**可移植**

> 如果软件不可移植，通常是因为软件编写不正确。请记住，尽管 x86 *现在*占据了市场多数，但是一旦 x86-64 流行起来，它在不久的将来可能不会出现。

**合理的安全记录**

> 不要包含具有糟糕安全记录的软件。每个漏洞对于很多人（安全团队，架构团队和软件包维护者）来说都是很多工作。
