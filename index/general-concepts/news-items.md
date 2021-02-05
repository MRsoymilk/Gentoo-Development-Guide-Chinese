# 新闻项目

Gentoo 新闻项目的创建是为了提供一种通信媒介，以通过 rsync tree 将关键消息推送给用户。新闻项目的原始提案在 [GLEP 42：重要新闻报道](https://www.gentoo.org/glep/glep-0042.html)中进行了概述。

可以使用 eselect 命令读取新闻项目。可在 news.eselect 手册页上找到有关使用此工具读取或删除新闻项的更多信息。

## 添加新闻项

- 选择适当的格式文件名： `YYYY-MM-DD-shortname.lang.txt`，即日期，然后在最多 20 个字符的一个很短的名称，只包括 `a-z`， `0-9`，`+`（加）， `-`（连字符）和`_`（下划线）。接下来 `lang` 是 [IETF 语言标记](https://tools.ietf.org/rfc/bcp/bcp47.txt)。除非要翻译新闻项目，否则英语始终使用`en`。最后，文件名以`txt`扩展名结尾。
- 编写新闻项，这类似于 RFC 兼容的邮件。有关允许的内容的详细信息可以在相应的 GLEP 部分中找到。**注意**：根据安装的软件包或激活的配置文件，可能会出现异常。
- 在提交前 72 小时将新闻项发送到 gentoo-dev 邮件列表和 Gentoo PR 团队（pr@gentoo.org）。
- 等待更正或反馈，你的新闻不是很重要，应该将其撤消（可能）。
- 在 gentoo-news 存储库中为你的项目创建目录结构（`git+ssh://git@git.gentoo.org/data/gentoo-news.git`）: `YYYY-MM-DD-shortname/`。
- 将新闻文件添加到该目录。
- 提交更改并将其推送到 gentoo-news 存储库。
