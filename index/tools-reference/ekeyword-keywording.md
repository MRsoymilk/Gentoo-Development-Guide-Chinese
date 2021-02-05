# `ekeyword` —— 关键字处理

`ekeyword`工具应用于操作`KEYWORDS`变量。使用此工具将确保你获得正确的关键字顺序——手动编辑可能会搞砸。用法是*ekeyword changes ebuilds*。有效更改的格式为`sparc`（标记为稳定），`~sparc`（标记为不稳定），`-sparc`（标记为不可用）和`^sparc`（移除 sparc 关键字）。特殊的`all`关键字在进行版本变更时很有用 —— `ekeyword ~all foo-1.23.ebuild` 会将当前存在的所有关键字都变为不稳定。有关详细信息，请参见 ekeyword-1。
