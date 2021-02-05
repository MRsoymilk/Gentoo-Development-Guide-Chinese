# 独立代码

在某些体系结构上，必须使用-fPIC 构建共享库。在 x86 和其他操作系统上，共享库可能没有-fPIC。这可能是浪费的，并可能导致性能下降。

如果遇到未使用-fPIC 构建共享库的软件包，请修补 Makefile 以仅使用-fPIC 构建共享库。有关 PIC 的更多信息，请访问[PIC 内部](https://wiki.gentoo.org/wiki/Project:Hardened/Position_Independent_Code_internals) Wiki 页面。如果不确定，请在公共开发者论坛（例如 gentoo-dev 邮件列表或`#gentoo-dev irc` 频道）中寻求帮助。
