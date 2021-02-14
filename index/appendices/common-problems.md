# 常见问题

本页提供有关如何处理各种常见错误和质量检查通知的建议。

## 处理质量检查通知

`ebuild.sh` 可移植部分包括对常见问题的大量检查。这些可能会导致显示“质量检查通知”消息。你不能依赖于始终生成这些消息的 portage —— 它们不能替代开发人员的适当测试和质量检查。

<div class="alert alert-note">
<b>注意</b>：一些eclass以相同的格式输出消息。这些不在这里。
</div>

### 质量检查通知：USE 标志 foo 不在 IUSE 中

除“特殊”标志（架构标志和`USE_EXPAND`变量）外，软件包使用的所有`USE`标志都必须包含在`IUSE`中。请参阅[IUSE](../ebuild-writing/variables.md)和[USE 标志](../general-concepts/use-flags.md)。

### 质量检查通知：全局范围内的 foo

当在全局范围内不适当地使用各种工具时，会出现此消息。请记住，不应在外部运行任何外部代码。根据使用的工具，有多种选择：

**`sed`，`awk`，`grep`，`egrep`，`cut` 等**

> 通常，在全局作用域中使用以上任何一种方法时，通常是操纵版本或程序名称字符串。应当避免使用纯 `bash` 构造方法。这里经常使用`eapi7-ver`。请参阅[版本和名称格式问题](./../ebuild-writing/variables.md)，`man eapi7-ver.eclass`以及 [Bash 变量操作](../tools-reference/bash-standard-shell.md)。

**`has_version`， `best_version`**

> 全局调用这两个键都表明存在严重问题。你**不得**使元数据根据系统相关信息而有所不同 —— 请参阅 [Portage Cache](./../general-concepts/the-portage-cache.md)。你应该重写 ebuild 以正确使用依赖项。

**`python`，`perl` 等**

> Ebuild 是 `bash` 脚本。将你不知道如何在 bash 上执行的任何操作转移到另一种语言上是不可接受的 —— 如果没有其他选择，因为在生成 ebuild 时并非所有用户都将拥有完整的系统。

### 质量检查通知：foo 是 setXid，可动态链接并使用惰性绑定

出于安全原因，动态链接的 setXid 应用程序在链接时不应使用惰性绑定。如果显示此消息，则有两种选择：

- 链接时，修改软件包的 Makefile（或等效文件）以使用`-Wl,-z,now`。此解决方案是首选。
- 使用`append-ldflags`（请参阅[添加其他标志](./../ebuild-writing/ebuild-phase-functions/src_compile/configuring-build-environment.md)）立即添加`-Wl,-z,now`。这将影响*所有*已安装的二进制文件，而不仅仅是 setXid 二进制文件。

### 质量检查通知：ECLASS foo 被非法继承

所有的 eclass 继承必须是无条件的，或纯粹基于静态机器无关的标准基础（`PN` 和 `PV` 在这里是最常见的）。请参阅 [Portage 缓存](./../general-concepts/the-portage-cache.md)。

在实际上没有发生任何非法继承的情况下，你可能会看到此警告。最常见的：

- 取消卸载使用不记录继承的旧版本软件包安装的软件包时。
- 在具有过时的缓存的覆盖中使用 eclass 时。请参见[Overlay 和 Eclass](./../general-concepts/overlay.md)。
- 使用过时的 portage 缓存时。

如果你在本地看到此通知， 则应手动检查 [Portage Cache](./../general-concepts/the-portage-cache.md) 中描述的规则。如果你在`rsync`后使用纯 `emerge sync`设置时看到此通知，则可能是真的出问题了。

<div class="alert alert-note">
<b>待办事项</b>：来自vapier：TEXTREL的...包含文本重定位的二进制文件...参见'prepstrip'以获取完整描述不安全文件...基本上是setid可写的文件其他用户我已经为portage HEAD添加了以下QA检查以移植（不知道何时发布）：不安全的RUNPATHs ...在其中编码了RUNPATH的二进制文件，位于+t目录中可执行堆栈...堆栈标记有+x的二进制文件...例如在amd64上会溢出。
</div>

### 处理 `repoman` 消息

<div class="alert alert-note">
<b>待办事项</b>：写。
</div>

### 处理访问冲突

Portage 在构建过程的某些阶段使用沙盒。这样可以防止包装意外在“安全”的位置外书写。有关详细信息，请参见[沙盒](./../general-concepts/sandbox.md)。

如果软件包违反了沙箱操作，则会出现类似以下的错误（假设沙盒已启用并在测试系统上正常工作，对于开发人员来说，情况总是如此）：

```bash
    --------------------------- ACCESS VIOLATION SUMMARY ---------------------------
    LOG FILE = "/tmp/sandbox-app-misc_-_breakme-1-31742.log"

    open_wr:   /big-fat-red-error
    --------------------------------------------------------------------------------
```

`open_wr` 意味着软件包试图打开以写入不在允许写入区域内的文件。在这种情况下，相关文件为 `/big-fat-red-error`。

存在针对读取限制区域的其他错误消息 —— 这些错误消息很少在 ebuild 中使用。

访问冲突最常见于安装阶段。有时，可以通过欺骗构建系统使用更安全的位置来解决沙箱违规问题。有关讨论，请参见`src_install`和[安装目标](./../general-concepts/install-destinations.md)。
