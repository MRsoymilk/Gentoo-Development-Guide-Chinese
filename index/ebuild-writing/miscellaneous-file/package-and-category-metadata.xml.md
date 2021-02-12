# 软件包和类别文件`metadata.xml`

`metadata.xml`文件用于指定有关软件包或类别的其他数据。
句法

元数据文件遵循 [GLEP 68](https://www.gentoo.org/glep/glep-0068.html) 中定义的语法 。字符集**必须**是 [GLEP 31](https://www.gentoo.org/glep/glep-0031.html) 指定的 UTF-8 。

关于缩进，没有严格的规则来控制样式和宽度。但是，需要遵循以下最低限度规则：

- 缩进应该是一致的，即空格或制表符，但不能两者都有。
- 改动属于其他开发人员的 metadata.xml 文件时，请保留现有样式。

下表总结了可能出现在 metadata.xml 中的标签：

|                         |                                                                                                                                                                                                                                  |
| :---------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **标签**                | **描述**                                                                                                                                                                                                                         |
| `<catmetadata>`         | `metadata.xml` 类别文件的根元素。它没有属性。它包含许多 `<longdescription>`标签，每个标签用于不同的语言。                                                                                                                        |
| `<pkgmetadata>`         | `metadata.xml`软件包文件的根元素。它没有属性。下面的子标签被允许： `<maintainer>`， `<longdescription>`， `<stabilize-allarches>`， `<slots>`， `<use>`，和 `<upstream>`。尽管所有子标签都是可选的，但`<upstream>`最多出现一次。 |
| `<maintainer>`          | 该标签指定负责维护软件包的人员和/或项目。`type` 必须指定该属性，该属性可以是`"person"`或`"project"`。有一个必需的子标签： `<email>`。它具有两个可选的子标签： `<name>`和 `<description>`。                                       |
| `<email>`               | 包含维护者的邮件地址。它是必需的。                                                                                                                                                                                               |
| `<name>`                | 包含带有维护者名称的自由文本。它是可选的。                                                                                                                                                                                       |
| `<description>`         | description 标签包含对维护的描述，或者例如，有兴趣的人可以接管维护的说明。它是可选的。                                                                                                                                           |
| `<longdescription> `    | 该标签包含类别或软件包的描述。对于软件包它用于在 ebuilds 自身中增加[DESCRIPTION](./../variables.md) 字段。此标签具有两个可选的子标签：`<pkg>`和`<cat>`。                                                                                         |
| `<stabilize-allarches>` | 表示独立于体系结构的软件包。如果 ebuild 仅在一种体系结构上进行了测试，则可以将其标记为对相关体系结构稳定。此标签必须包含空内容。                                                                                                 |
| `<slots>`               | 此标签描述软件包的 [slot](./../../general-concepts/slotting.md)。它具有两个可选的子标签：`<slot>`和`<subslots>`。                                                                                                                                                    |
| `<slot>`                | 它包含有关特定 slot 的信息。该 `name` 属性是必填属性，用于指定 slot 的名称。                                                                                                                                                     |
| `<subslots>`            | 描述整个软件包的子 slot 的含义。该标签最多出现一次。                                                                                                                                                                             |
| `<use>`                 | 此标签包含 [USE 标志](./../variables.md)的描述 。此标签是可选的，如果指定，则具有一个必需的子标签： `<flag>`。                                                                                                                                    |
| `<flag>`                | 此标签包含有关命名 USE 标志如何影响此软件包的描述。如果`<use>`指定了标签，则为必需。它还要求在 `name` 属性中命名 USE 标志。此标签具有两个可选的子标签：`<pkg>`和 `<cat>`。                                                       |
| `<upstream>`            | 该标签包含有关上游开发人员/项目的信息。它支持多个可选子标签：`<maintainer>`，`<changelog>`，`<doc>`，`<bugs-to>`和`<remote-id>`。                                                                                                |
| `<maintainer>`          | 提供有关上游维护者的信息。它需要指定`<name>`子标签，并支持可选的`<email>`子标签和可选的 `status` 属性。请注意，`不得`为上游维护者指定该 `type` 属性。                                                                            |
| `<name>`                | 上游维护者的名称应包含带有上游名称的文本块。对于上游维护者，此元素是必需的，并且必须出现一次。                                                                                                                                   |
| `<email>`               | 上游维护者的邮件地址。只能出现一次。                                                                                                                                                                                             |
| `<changelog>`           | 应该包含一个 URL，可以在其中找到上游变更日志的位置。该 URL 必须是版本无关的，并且必须指向仅在相应软件包的新发行版上更新的变更日志。（这也意味着仅在 vcs 快照的情况下，可以链接到自动更新的变更日志）。最多出现一次。             |
| `<doc>`                 | 应该包含一个 URL，可以在其中找到上游文档的位置。该链接不得指向任何第三方文档，并且必须与版本无关。如果文档提供了多种语言，则可以使用 `lang` 属性，该属性遵循与`<longdescription>`相同的规则。                                    |
| `<bugs-to>`             | 应包含可以提交错误的位置，URL 或以`mailto:`开头的邮件地址 。最多出现一次。                                                                                                                                                       |
| `<remote-id>`           | 应该指定一种类型的软件包识别跟踪器以及与该软件包相对应的识别。`remote-id` 应该使索引诸如 `Freshmeat ID` 或 `CPAN` 名称之类的信息更加容易。它具有一个必填属性 type，用于标识上游源的类型。                                        |
| `<pkg>`                 | 该标签包含有效的合格软件包名称。                                                                                                                                                                                                 |
| `<cat>`                 | 此标签包含有效的类别名称，如中所定义 `profiles/categories`。                                                                                                                                                                     |

这些标签还可以使用一些属性：

| **属性** | **标签**                                                                | **描述**                                                                                                                                                                                                                                                                                                                                        |
| :------- | :---------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| lang     | `<description>`，`<longdescription>`， `<slots>`，`<use>`，`<doc>`      | 在每种情况下都需要描述，*至少*必须有英文描述。如果给出了另一种语言的附加描述，则此属性用于指定所使用的语言。格式是 [ISO-639-1](https://www.w3.org/WAI/ER/IG/ert/iso639.htm#2letter) 规范定义的两个字符的语言代码。                                                                                                                                                                                 |
| restrict | `<maintainer>`，`<longdescription>`， `<stabilize-allarches>`，`<flag>` | 限制属性允许将某些标签的应用限制到软件包的某些版本。使用此属性时，必须存在具有或不具有 `restrict` 属性的其他标签，以覆盖软件包的所有版本。没有`restrict`属性的标签用作默认标签。限制属性的格式是 EAPI = 0 软件包依赖项规范的格式（即，不允许使用 USE 条件或 slot 依赖项）。由于`<`和`>`是 XML 中的特殊字符，因此必须使用转义字符`&lt`和`&gt;`。 |
| name     | `<flag>`， `<slot>`                                                     | 当`<flag>`标签上需要此属性时，它仅包含 USE 标志的名称。对于`<slot>`标签，它指定要应用的 [slot 名称](./../../general-concepts/slotting.md)。slot 名称`*` 允许单个`<slot>`标签与软件包的所有 slot 匹配，在这种情况下，可能不会出现其他 slot 标签。                                                                                                                                    |
| status   | `<maintainer>`                                                          | 上游维护者元素具有状态属性，该属性是`"active"`或`"inactive"`之一。此属性不是必需的。缺失值应为 `"unknown"`。请注意，该属性仅允许用于`<upstream>`元素的`<maintainer>`子标签！                                                                                                                                                                    |
| type     | `<remote-id>`                                                           | 标识上游源类型的字符串。有效字符串列表保存在 [metadata.dtd](https://www.gentoo.org/dtd/metadata.dtd) 中。开发人员应在使用新类型值之前通过邮件发送 gentoo-dev 邮件列表。                                                                                                                                                                                                                |
| type     | `<maintainer>`                                                          | 定义软件包维护者的类型。只有两个有效值：`"person"`和`"project"`。后者表示一个正式的 [Gentoo 项目](./../../general-concepts/projects.md)。                                                                                                                                                                                                                                           |

##软件包的元数据

所有软件包**必须**包含 `metadata.xml` 文件，该文件提供有关软件包描述，维护者，本地 USE 标志，上游等信息。

为了方便开发人员，Gentoo tree 中提供了一个骨架文件，名称为 [skel.metadata.xml](https://gitweb.gentoo.org/repo/gentoo.git/tree/skel.metadata.xml)。也可以使用 `app-portage/metagen` 工具创建元数据文件。

软件包元数据文件的提交由`repoman`处理，作为常规软件包维护的一部分。

除非另有说明，否则按照 [GLEP 67](https://www.gentoo.org/glep/glep-0067.html#bug-assignment) 的规定，元数据中首先列出的维护者将是该软件包的错误的处理者。

##软件包元数据示例

在以下各节中，提供了 `metadata.xml` 的各种示例。这些示例基于实际的软件包元数据文件，以使内容尽可能真实。但是，它们可能不完全包含这些文件，因此应作为假设示例。

#### 维护者的项目

对于第一个示例，展示了一个由单个项目维护的软件包。它是软件包`app-office/libreoffice`的`metadata.xml`的简化版本。软件包维护者由邮件地址`office@gentoo.org`标识，其名称为`Gentoo Office Project`，该名称在可选的`<name>`子标签中指定。它还提供了详细的软件包说明。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE pkgmetadata SYSTEM "http://www.gentoo.org/dtd/metadata.dtd">
<pkgmetadata>
  <maintainer type="project">
    <email>office@gentoo.org</email>
    <name>Gentoo Office Project</name>
  </maintainer>
  <longdescription>
    LibreOffice is the successor of OpenOffice.org. This ebuild
    allows you to compile it yourself. Unfortunately this compilation can
    take up to a day depending on the speed of your computer. It will
    however make a snappier LibreOffice than the binary version.
  </longdescription>
</pkgmetadata>
```

邮件地址`office@gentoo.org`对应于[projects.xml](https://api.gentoo.org/metastructure/projects.xml)中定义的`Gentoo Office Project`。该文件列出了 Gentoo 中的所有项目，并且是从 Gentoo Wiki 上[可用的项目](https://wiki.gentoo.org/wiki/Project:Gentoo)列表生成的 ：

```xml
<project>
  <email>office@gentoo.org</email>
  <name>Gentoo Office Project</name>
  <url>https://wiki.gentoo.org/wiki/Project:Office</url>
  <description>
    The Office project manages the office implementations
    and related packages in Gentoo.
  </description>
  <member>
    <email>dilfridge@gentoo.org</email>
    <name>Andreas K. Hüttel</name>
    <role>member</role>
  </member>
  <member>
    <email>scarabeus@gentoo.org</email>
    <name>Tomáš Chvátal</name>
    <role>member</role>
  </member>
</project>
```

#### 本地 USE 标志说明

第二个示例在`sys-apps/portage`的`metadata.xml`之后形成。它列出了多个维护者，其中一个是开发人员，另一个是项目。它说明了如何指定本地 USE 标志描述，并且还包含一个上游元素。还应该指出在`<bugs-to>`标签中使用`mailto:`前缀，因此使用邮件地址而非 URL。相反，在`<email>`标签中指定邮件地址不需要此类前缀。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE pkgmetadata SYSTEM "http://www.gentoo.org/dtd/metadata.dtd">
<pkgmetadata>
  <upstream>
    <bugs-to>mailto:dev-portage@gentoo.org</bugs-to>
    <changelog>https://gitweb.gentoo.org/proj/portage.git/plain/RELEASE-NOTES</changelog>
    <doc>https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Portage</doc>
  </upstream>
  <maintainer type="person">
    <email>larry@gentoo.org</email>
    <name>Larry the Cow</name>
  </maintainer>
  <maintainer type="project">
    <email>dev-portage@gentoo.org</email>
  </maintainer>
  <use>
    <flag name="epydoc">Build html API documentation with epydoc.</flag>
    <flag name="ipc">Use inter-process communication between portage and running ebuilds.</flag>
    <flag name="pypy2_0">Use pypy-c2.0 as Python interpreter.</flag>
    <flag name="python2">Use python2 as Python interpreter.</flag>
    <flag name="python3">Use python3 as Python interpreter.</flag>
    <flag name="xattr">
      Preserve extended attributes (filesystem-stored metadata) when
      installing files. Usually only required for hardened systems.
    </flag>
  </use>
</pkgmetadata>
```

#### 划分维护关系

本示例使用属性`restrict`根据软件包版本划分维护者。根据`sys-boot/grub`的`metadata.xml`描述，版本 2 及更高版本的 ebuild 由floppym@gentoo.org维护，而早期版本则由 Gentoo 基本系统项目维护。注意在`restrict`中使用“`&gt;`” 而非“>” 。

该示例还在 USE 标志说明中使用了`<pkg>`标签。`<pkg>`中不允许使用 slot 依赖性说明符，因此，采用`<pkg>sys-boot/grub</pkg>:2`而非`<pkg>sys-boot/grub :2</pkg>`表示。

最后，说明上游描述中的`<remote-id>`标签。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE pkgmetadata SYSTEM "http://www.gentoo.org/dtd/metadata.dtd">
<pkgmetadata>
  <maintainer restrict="&gt;=sys-boot/grub-2" type="person">
    <email>floppym@gentoo.org</email>
  </maintainer>
  <maintainer type="project">
    <email>base-system@gentoo.org</email>
    <name>Gentoo Base System</name>
  </maintainer>
  <use>
    <flag name="device-mapper">
      Enable support for device-mapper from <pkg>sys-fs/lvm2</pkg>
    </flag>
    <flag name="efiemu">
      Build and install the efiemu runtimes
    </flag>
    <flag name="fonts">Build and install fonts for the gfxterm module</flag>
    <flag name="mount">
      Build and install the grub-mount utility
    </flag>
    <flag name="libzfs">
      Enable support for <pkg>sys-fs/zfs</pkg>
    </flag>
    <flag name="multislot">
      Allow concurrent installation of <pkg>sys-boot/grub</pkg>:0 and
      <pkg>sys-boot/grub</pkg>:2 by renaming all programs.
    </flag>
    <flag name="themes">Build and install GRUB themes (starfield)</flag>
    <flag name="truetype">
      Build and install grub-mkfont conversion utility
    </flag>
  </use>
  <upstream>
    <remote-id type="sourceforge">dejavu</remote-id>
  </upstream>
</pkgmetadata>
```

#### slot 和子 slot

本示例重点是通过检查`media-libs/libpng`的元数据来演示如何指定 slot 和子 slot。根据软件包的性质，slot 可能有多种原因。对于这个特定的软件包可以看出 slot 用于提供具有不同二进制兼容性的库的不同版本，并且建议开发人员针对 slot 0 进行构建。此外，根据`<subslots>`标签中指定的描述，具有相同子 slot 的该软件包的不同版本提供了相同的应用程序二进制接口（ABI）。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE pkgmetadata SYSTEM "http://www.gentoo.org/dtd/metadata.dtd">
<pkgmetadata>
  <maintainer type="project">
    <email>base-system@gentoo.org</email>
    <name>Gentoo Base System</name>
  </maintainer>
  <use>
    <flag name="apng">support unofficial APNG (Animated PNG) spec</flag>
  </use>
  <upstream>
    <remote-id type="cpe">cpe:/a:libpng:libpng</remote-id>
    <remote-id type="sourceforge">apng</remote-id>
  </upstream>
  <slots>
    <slot name="0">
      For building against. This is the only slot
      that provides headers and command line tools.
    </slot>
    <slot name="1.2">
      For binary compatibility, provides libpng12.so.0 only.
    </slot>
    <slot name="1.5">
      For binary compatibility, provides libpng15.so.15 only.
    </slot>
    <subslots>Reflect ABI compatibility for libpng.so.</subslots>
  </slots>
</pkgmetadata>
```

## 维护者需要

维护人员需要的或孤立的没有维护人员对此负责的软件包。根据 [GLEP 67](https://www.gentoo.org/glep/glep-0067.html#case-of-maintainer-needed-packages)，这些软件包的不得在`<pkgme​​tadata>`下的任何`<maintainer>`子标签中包含 `metadata.xml`。对于这种情况的严格测试将是：

```bash
[[ $(xmllint --xpath "count(/pkgmetadata/maintainer)" metadata.xml) -eq 0 ]]
```

按照约定，包含`maintainer-needed`字符串的注释行需要插入。与软件包有关的其他标签可以出现在元数据中。这些软件包的错误必须分配给`maintenanceer-needed@gentoo.org`。质量检查团队会定期生成[孤立的软件包列表](https://qa-reports.gentoo.org/output/maintainer-needed.html)以及它们相应的错误，作为质量检查报告的一部分。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE pkgmetadata SYSTEM "http://www.gentoo.org/dtd/metadata.dtd">
<pkgmetadata>
  <!-- maintainer-needed -->
  <upstream>
    <maintainer status="active">
      <email>rasmus@alum.mit.edu</email>
      <name>Matt Rasmussen</name>
    </maintainer>
    <doc lang="en">http://keepnote.org/manual/</doc>
    <bugs-to>https://code.google.com/p/keepnote/issues/list</bugs-to>
  </upstream>
  <longdescription lang="en">
    KeepNote is a note taking application. With KeepNote, you can
    store your class notes, TODO lists, research notes, journal entries,
    paper outlines, etc in a simple notebook hierarchy with rich-text
    formatting, images, and more. Using full-text search, you can
    retrieve any note for later reference.
  </longdescription>
</pkgmetadata>
```

## 类别元数据

对于类别，请 `metadata.xml` 指定详细说明（使用英语，也可以使用其他语言）。一个典型的例子：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE catmetadata SYSTEM "http://www.gentoo.org/dtd/metadata.dtd">
<catmetadata>
  <longdescription lang="en">
    The app-vim category contains plugins and syntax file
    packages for the Vim text editor.
  </longdescription>
  <longdescription lang="de">
    Die Kategorie app-vim enthält Plugins und Syntax-Pakete
    für den Vim Texteditor.
  </longdescription>
</catmetadata>
```

创建新类别时，`必须创建`英语的`metadata.xml`文件以及`<longdescription>`。翻译工作由任何感兴趣的开发人员解决。

当前提交类别 `metadata.xml` 文件的正确方法是：

```bash
xmllint --noout --valid metadata.xml
git add metadata.xml
git commit --gpg-sign
```

提交消息的格式应正确。提交示例如下所示：

```bash
commit db359439bcd52f5a7f20d2332ab62feb16657504
Author: Alexis Ballier <aballier@gentoo.org>
Date:   Tue Sep 22 10:47:49 2015 +0200

  dev-ros: Add metadata.xml for the category.
```
