# 项目

[GLEP 39](https://www.gentoo.org/glep/glep-0039.html#specification)中定义了 Gentoo 的管理结构，称为“元结构”。在 Gentoo 中，一个项目是一组致力于在不同领域中实现共同目标的开发人员。例如，[Devmanual](https://wiki.gentoo.org/wiki/Project:Devmanual) 项目专注于维护此文档。许多其他人负责维护软件包。涵盖大范围主题的项目可以具有多个子项目，这些子项目专门研究父项目域内的特定部分，从而形成完整的项目层次结构。

项目组软件包维护者需要项目在 [metadata.xml](./../ebuild-writing/miscellaneous-file/package-and-category-metadata.xml.md) 中将其列为维护者。可以在 [api.gentoo.org](https://api.gentoo.org/metastructure/projects.xml) 或 [wiki](https://wiki.gentoo.org/wiki/Project:Gentoo) 上找到所有项目的完整列表。

## 开始新项目

根据元结构，任何开发人员都可以创建一个新项目。启动新项目涉及两个过程：

1. [通过 wiki](https://wiki.gentoo.org/wiki/Gentoo_Wiki:Developer_Central/Project_pages) 创建一个新的项目页面。
2. 将评论请求（Reques For Comments, RFC）邮件发送到 gentoo-dev 邮件列表。

RFC 不需要批准，负面评论也不会阻止开发人员创建项目。允许竞争项目在 Gentoo 中共存；目标相似项目的存在并不会阻止开发人员启动新的这类项目。

## 加入和离开项目

项目成员通过 Gentoo Wiki 上的项目页面进行管理。每个页面都有一个“项目”模板，其中列出了项目的成员。只需修改列表就足以添加或删除开发人员。请注意，不同的项目对招聘开发人员有不同的要求和流程，这可能需要在修改成员列表之前进行事先安排。

开发人员需要在 dev.gentoo.org 上编辑`/var/mail/alias/misc/<alias name>`将自己添加到该项目中。例如，Devmanual 项目的别名位于`/var/mail/alias/misc/devmanual`对应于项目的邮件地址`devmanual@gentoo.org`。
