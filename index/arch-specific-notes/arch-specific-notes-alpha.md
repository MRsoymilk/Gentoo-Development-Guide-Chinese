# 架构具体说明 —— Alpha

Alpha 端口使用`alpha`关键字。它着重于 HP（以前称为 Compaq（以前称为 DEC））硬件。涵盖了从`ev4`（称为 21064）到`ev7z`（称为 21364a）。

## Alpha 内核和用户区应用二进制接口

所有 Alpha 系统都使用纯 64 位内核和纯 64 位用户区。

所有的 Alpha 系统支持大端字节（big endian）和小端字节（little endian）—— 但是 Linux 仅使用小端字节。

## 其他 Alpha 关键字要求

通常，期望为 Alpha 做关键字的任何人都应该使用`alpha@`别名。但是，如果维护人员在访问 Alpha 硬件时使用关键字为其软件包添加密码，则 Alpha 团队很高兴，尽管该团队希望对此有所了解。

## Alpha 指令集和性能说明

有六个基本的 Alpha 指令集标准：

- `ev4`或`ev45`。 `ev4`是 Alpha 系列中的第一个 Alpha 处理器。它具有一个整数管道和一个浮点管道。 `ev45`是修改后的`ev4`，具有双倍的数据和指令缓存（分别为 D-Cache 和 I-Cache）；它还具有部门优化功能。
- `ev5`是`ev45`的演变。管道数量增加了一倍，浮点管道以 9 个阶段而不是 10 个阶段运行。`ev5`支持 3 个缓存级别。
- `ev56`添加了 BWX 扩展名，以 8 位或 16 位量子的方式加载/存储数据。
- `pca56`添加了一套新的 MVI（运动视频指令），旨在加速视频和音频计算。
- `ev6`支持`pca56`和新的 FIX 集和支持的所有扩展，FIX 用于在整数和浮点寄存器之间以及平方根之间移动数据。
- `ev67`是`ev6`的改进，此外还支持新的版本。 CIX 添加了用于计数和查找位的指令。

当没有`-mcpu`选项传递给`gcc`时，将默认使用构建编译器的处理器。

除非你对 Alpha 架构有深入的了解，否则**应**始终使用`-mieee`标志，因此有关[不过滤变量](./../general-concepts/user-environment.md)的注释对于 Alpha 确实很重要。

## 关于 Alpha 和 PIC 的注意事项

一般的[独立代码](./../general-concepts/position-independent-code.md)政策也适用于 Alpha。实际上，如果你尝试链接 PIC 和非 PIC 代码，则 Alpha 系统会显示警告。通常，这会导致编译中止出现错误。

## 联系 Alpha 团队

可以与 Alpha 团队联系：

- 通过 Bugzilla 将漏洞分配给 `alpha@`
- 通过邮件发送到`alpha@`邮件别名
- 通过邮件发送到`gentoo-alpha`邮件列表
- 通过 Freenode 上的`#gentoo-alpha`IRC 频道

## 其他资源

- [Gentoo Linux Alpha 开发项目](https://wiki.gentoo.org/wiki/Project:Alpha)
- [Gentoo / Alpha 常见问题](https://wiki.gentoo.org/wiki/Alpha/FAQ)
- [Alpha 移植指南](https://wiki.gentoo.org/wiki/Project:Alpha/Porting_guide)
- [Gentoo 关于替代架构论坛](https://forums.gentoo.org/viewforum-f-32.html)
