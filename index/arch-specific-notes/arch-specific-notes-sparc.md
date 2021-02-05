# 特定架构说明 —— SPARC

SPARC 端口使用 `sparc` 关键字。它着重于 `sun4u` 硬件（带有 `v9` CPU 的 Sun UltraSparc 系统）。`v8` 处理器和 2.4 内核仍然可以与 Gentoo 一起使用，但是 Gentoo/SPARC 团队不再支持它们。

## SPARC 内核和 用户方 ABI

`v9` 系统使用纯 64 位内核和纯 32 位用户区。如果内核不提供有效的 32 位兼容接口，则在使用低级软件时可能会导致可移植性问题。

`v8` 系统使用纯 32 位内核和纯 32 位用户区。

上面所有的 SPARC 系统都是大端指令，但是`v9`体系结构也利用小端指令来访问 PCI 总线上的数据。

## 其他 SPARC 关键字要求

<div class="alert alert-note">
<b>注意</b>： 本节是对<a href="../keywording-and-stabilization.md">升级关键字设置</a>中准则的补充 。它讨论 了 SPARC 体系结构的<i>其他</i>要求。
</div>

要在软件包中添加`~sparc`关键字，通常必须包含以下附加项：

- 该软件包必须已经在`v9`系统上由架构团队成员（或获得架构团队许可的人员）进行了测试。

通常，期望为 SPARC 设置关键字的任何人都应使用`sparc@`别名。

## SPARC 指令集和性能说明

有三种基本的 SPARC 指令集标准。

- `v7` 是在非常旧的硬件中使用的原始指令集。Gentoo 不会交付 `v7` 有能力的平台，但是理论上足够疯狂的人可以在 `v7` 机器上运行 Gentoo 。
- `v8` 是对的扩展，`v7` 增加了对硬件整数乘法和除法的支持。Gentoo sparc32（`sun4m`）阶段的上一次可用是 2006.1 版本。
- `v9` 新增了 64 位支持和大量的性能增强函数。Gentoo sparc64（`sun4u`）阶段是 `v9`。

此外，各个 CPU 的实现略有不同 —— 例如，在调度某些指令时，HyperSparc CPU 的要求有所放松。这些是相对较小的差异。

如果没有任何`-mcpu`参数调用`gcc`，它将生成`v7`代码。根据应用程序的不同，在 UltraSparc 上运行时，它的速度可能比`v9`代码慢 5 倍 —— 大量使用整数乘法和除法的密码和图形应用程序受到的打击尤其严重。因此，[不过滤变量](./../general-concepts/user-environment.md)中的注释在 SPARC 上尤其重要。

## 与 SPARC 团队联系

可通过以下方式与 SPARC 团队联系：

- 通过 Bugzilla 将错误分配给`sparc@`
- 通过邮件发送到`sparc@`邮件别名
- 通过邮件发送到`gentoo-sparc`邮件列表
- 通过 Freenode 上的`#gentoo-sparc`IRC 频道
