# Introduce

原网址：[Gentoo Development Guide](https://devmanual.gentoo.org/)

中文网址：[Gentoo Development Guide Chinese](https://blog.soymilk.fun/Gentoo-Development-Guide-Chinese/)

## Info

Version 0.0.0 已经完成，格式大致能看（有些格式做了一些小修改），内容大致能看，页面链接待完善。

TODO:

- [ ] 完善格式
- [ ] 完善链接
- [ ] 完善内容
- [ ] Version 0.1.0

## Problem

原:
[Mirrors](https://devmanual.gentoo.org/general-concepts/mirrors/index.html)

you _must_ manually mirror the files

译:
[镜像](index/general-concepts/mirrors.md)

则*必须*修改相应镜像文件

---

原:
[Keywording and Stabilization](https://devmanual.gentoo.org/keywording/index.html)

If you maintain an architecture-independent package (data files, icons, pure Perl,...) then you may request that your package be stabilized on all arches at once. To do this — when you are filing the stabilization bug — please add the keyword `ALLARCHES` in addition to `STABLEREQ` and CC the arches that you would like to stabilize.

译:
[关键字和稳定化](index/keywording-and-stabilization.md)

如果你维护体系结构无关的软件包（数据文件，图标，纯 Perl 等），那么可以要求将你的软件包立即稳定在所有架构上。为此，当你提交稳定性错误时，除了`STABLEREQ`和`CC`外，还请添加关键字`ALLARCHES`，你想要稳定的架构也要添加。

---

原:
[Configuring XEmacs for UTF-8](https://devmanual.gentoo.org/appendices/editor-configuration/xemacs/index.html)

- `set-default-buffer-file-coding-system (C-x RET F)` sets the default for unsaved buffers and ASCII-looking files that have no obvious character set markers for autodetection.
- `universal-coding-system-argument (C-x RET c)` lets you set the coding system for the next command, so issuing this from a dired window immediately before opening a problematic file is a tactic that can be of use when autodetection gets things pathologically wrong.

译:
[配置 XEmacs UTF-8](index/appendices/editor-configuration/configuring-xemacs-for-utf-8.md)

- `set-default-buffer-file-coding-system (C-x RET F)` 为没有显式设置字符集编码的未保存缓冲区和 ASCII 外观文件设置默认值。
- `universal-coding-system-argument (C-x RET c)` 能让你为下一个命令设置编码系统，因此可以在错误出现之前，从新的窗口发送此命令后再打开问题文件，避免错误出现。

---

原:

[Projects](https://devmanual.gentoo.org/general-concepts/projects/index.html)

Developers should remember to add themselves to the alias by editing `/var/mail/alias/misc/<alias name> `on dev.gentoo.org.

译:

[项目](index/general-concepts/projects.md)

开发人员需要在 dev.gentoo.org 上编辑`/var/mail/alias/misc/<alias name>`将自己添加到该项目中。
