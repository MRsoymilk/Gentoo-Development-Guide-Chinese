# 特定架构说明 —— MIPS

MIPS 端口使用`mips`关键字。它着重于常用硬件 —— 主要是 SGI，Cobalt，Cavium Octeon 和 MIPS Creator CI20 系统 —— 尽管还支持各种嵌入式和专用板。

`mips`关键字涵盖了范围广泛的体系结构，CPU 和硬件，从小型嵌入式设备到具有数十个 CPU 的服务器类套件。

<div class="alert alert-note">
<b>注意</b>： 术语：ABI代表“应用程序二进制接口”。它涉及诸如调用约定（调用函数时使用哪些寄存器来传递参数）和数据类型大小之类的问题。ISA代表“指令集体系结构”，指的是可用指令以及给定CPU的寄存器数量和类型。
</div>

## MIPS ABI

`o32` ABI 是 SGI 的一项出色发明，当时还不错，但后来却显得有些短视和效率低下。 `n32` ABI 通过伪装为 32 位，而实际上为 64 位来纠正该问题。 `n64`是另一个 64 位 ABI，这次不假装为 32 位，因此它很大，很胖但非常强大。

尽管大多数硬件都不支持这两种选择，但所有这些 ABI 都可以是大和小端，因为 MIPS CPU 兼有两种。

所有这些 ABI 在各种应用程序领域中都很流行。它们实际上都无法正常工作。

## MIPS ISA

最常见的 MIPS ISA 是 mips2，mips3，mips4，mips32 和 mips64。如果遇到需要了解两者之间差异的情况，请与 MIPS 团队联系。

## `CFLAGS` 在 MIPS 上不下降

因为 `CFLAGS` 有时用于指定 ISA 和 ABI 信息，所以软件包必须遵守此设置至关重要。请参阅 [不过滤变量](./../general-concepts/user-environment.md)。

## 其他 MIPS 关键字要求

<div class="alert alert-note">
<b>注意</b>： 本节是<a href="./../keywording-and-stabilization.md">关键字和稳定化</a>指南中的补充内容，它讨论了MIPS体系结构的<i>其他</i>要求。
</div>

为了使软件包中`~mips`添加关键字，通常必须包含以下附加项：

- 该软件包应在大小端系统上运行，在纯 32 位和纯 64 位系统上以及在具有不同内核和用户级 ABI 的系统上都可以运行。

通常，期望为 MIPS 做关键字的任何人都应该在`mips@`别名上。

MIPS 当前不使用稳定的关键字，因此请勿向其提交稳定的请求。

## 联系 MIPS 团队

可以通过以下方式联系 MIPS 团队：

- 通过 Bugzilla 将错误分配给 `mips@`
- 通过邮件发送到`mips@`邮件别名
- 通过邮件发送到`gentoo-mips`邮件列表
- 通过 Freenode 上的`#gentoo-mips`IRC 频道
