# 特定架构说明 —— PPC

Gentoo PowerPC 端口使用`ppc`关键字并保持与所有 32 位 PowerPC 处理器的兼容性。它还可用于在 64 位 PowerPC 系统上的 32 位用户环境安装。

## 常见问题

尽管 PowerPC 处理器可以以小端模式运行，但是 Linux 内核以大端模式在 PowerPC 处理器上运行。由于这个事实，PowerPC 的一个常见问题是处理只考虑了小端字节（`x86`/`amd64`）的代码。这些错误可能很难找到，但是通常是在从磁盘加载数据（例如直接写入磁盘的结构）或位操作期间发现的。

默认情况下，gcc 的 PowerPC 端口使用无符号字符，这与`x86`上的不同。如果你使用的代码假定该 `char` 类型是已签名的，则可以传递`-fsigned-char` 给 `GCC` 来解决该问题。

## Altivec

`G4`和`G5`处理器支持 Altivec（Apple 的 VMX SIMD 指令名称）。您可以通过将`-mabi = altivec -maltivec`传递给`GCC`来启用对指令集的支持。请注意，传递`-mcpu=`选项可以启用 altivec 而不传递上面的标志。

有时，出现的另一个问题是 Apple 使用不同的符号表示向量(x)而不是{x}。使用下面的代码定义向量是解决此问题的首选方法：

```bash
#ifdef CONFIG_APPLE
#define AVV(x...) (x)
#else
#define AVV(x...) {x}
#endif
```

## 与 PowerPC 团队联系

通过以下方式可以联系 PowerPC 团队：

- 通过 freenode 上的`#gentoo-ppc` IRC 频道
- 通过邮件发送给`ppc@gentoo.org`
- 通过邮件发送到`gentoo-ppc-dev@gentoo.org`邮件列表
- 通过 Bugzilla 将漏洞分配给 `ppc@gentoo.org`

## 其他资源

- [Gentoo PPC 常见问题](https://wiki.gentoo.org/wiki/PPC/FAQ)
- [Gentoo PPC 论坛](https://forums.gentoo.org/viewforum-f-24.html)
