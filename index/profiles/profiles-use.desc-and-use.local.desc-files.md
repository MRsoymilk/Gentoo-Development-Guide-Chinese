# 个人文件 `use.desc` 和 `use.local.desc` 文件

`use.desc` 文件包含所有全局未扩展 `USE` 标志的列表以及简短说明。未经 `gentoo-dev` 列表讨论，不得修改此文件。

`metadata.xml` 每个 ebuild 类别上 的文件都包含一个本地 `USE` 标志列表 ，以及该类别中软件包的简短描述。有关 `metadata.xml` 文件的更多信息，请参见 [此处](./../ebuild-writing/miscellaneous-file/package-and-category-metadata.xml.md)。

<div class="alert alert-note">
<b>注意</b>： <code><pre>use.local.desc</pre></code>曾经是所有本地useflags的默认位置。但是，此文件现在是所有<code><pre>metadata.xml</pre></code>文件的自动生成的聚合，因此你绝不能直接对其进行编辑。
</div>

允许使用相同名称的本地`USE`标志使用少量软件包。如果数量开始大量增加，则可能需要将该标志变为全局标志——请参阅[本地和全局 USE 标志](./../general-concepts/use-flags.md)。

所有非扩展标志都必须在这些文件之一中列出。

扩展标志 在`desc/${prefix}.desc`中列出，其中`${prefix}`是要以小写形式扩展的环境变量的名称。仅列出标志的后缀而不是列出完整的标志，即环境变量的可能值。
