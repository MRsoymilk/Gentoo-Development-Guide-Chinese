# 初始化脚本

应该使用`doinitd`或`newinitd`函数将初始化脚本安装到`/etc/init.d`中（请参阅[安装函数参考](./../function-reference/install-functions-reference.md)）。这些脚本的任何配置（命令行参数或环境变量）都应通过`/etc/conf.d`中同名条目来处理——`doconfd`或`newconfd`可用于安装这些文件。

Gentoo 初始化系统以及如何编写初始化脚本的概述可在[编写初始化脚本部分](https://wiki.gentoo.org/wiki/Handbook:X86/Working/Initscripts#Writing_initscripts)中找到。
