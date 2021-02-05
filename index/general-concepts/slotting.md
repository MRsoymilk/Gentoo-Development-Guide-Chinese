# Slotting

软件包可以支持同时安装多个版本。这对于可能已经更改版本之间的接口库有用——例如，`gtk+`包可以安装`2.24 `和 `3.6`两个版本。此函数称为 slotting。

大多数包装都不需要 slot。这些软件包`SLOT="0"` 在 ebuild 中指定。这纯粹是一个约定；软件包管理器 `0` 与其他 slot 值没有 任何区别。

<div class="alert alert-note">
<b>注意</b>：<code><pre>SLOT</pre></code>是必填变量，不能为空。
</div>

每个`SLOT`值，Portage 最多允许一个软件包安装实例。例如，假设我们有以下内容：

- `foo-1.1` 与 `SLOT="1"`
- `foo-1.2` 与 `SLOT="1"`
- `foo-2.0` 与 `SLOT="2"`
- `foo-2.1` 与 `SLOT="2"`

这样，用户可能会并行安装`foo-1.2`和`foo-2.0`，而没有`foo-1.1`和`foo-1.2`。请注意，用户完全有可能安装了`foo-2.0`，而根本没有`foo-1.x`。

要查看 `DEPEND` 特定 slot 中的软件包，请参阅 [SLOT 依赖](./dependencies.md)。

## 子 slot

有时，一个软件包会安装一个库来更改版本之间的接口，但是同时安装其中一些版本是不希望的或不便的。在`EAPI=5`或更高版本中，可以通过使用子 slot 来处理这种情况，该子 slot 与常规 slot 之间用`/`字符分隔，如 `SLOT ="slot / subslot"`中所示。当运行时依赖项的子 slot 更改时，可以[请求自动重建软件包](./dependencies.md)。

例如，假设软件包`foo` 安装了一个库，其 soname 对于不同的版本是不同的。将 soname 版本用作子 slot 名称是合理的：

- `foo-1.1`安装`libfoo.so.5`——`SLOT="1/5"`
- `foo-1.2`安装`libfoo.so.6`——`SLOT="1/6"`
- `foo-2.0`安装`libfoo-2.so.0`——`SLOT="2/0"`
- `foo-2.1`安装`libfoo-2.so.1`——`SLOT="2/1"`

然后，安装了链接到`libfoo-2`（或`libfoo`）的二进制文件的其他 ebuild 可以在安装的`foo:2`或`foo:1`更改子 slot 时（例如，当用户从`foo-2.0`升级时）请求自动重建到`foo-2.1。`

如果 ebuild 没有显式声明子 slot，则默认情况下将常规 slot 用作该子 slot 的值。

<div class="alert alert-note">
<b>注意</b>：首次在库ebuild中使用子slot时必须小心。添加子slot将触发已使用子slot相关性的所有软件包的重建（例如，在<code><pre>media-libs/libpng</pre></code>且软件包foo中从SLOT=“0”切换到SLOT =“0/14”取决于<code><pre>libpng:0=</pre></code>）。因此，最好在现有库接口更改时开始使用库中的子<code><pre>slot</pre></code>。
</div>

## Slot 名称

当前版本的 portage 接受以字母数字字符或*开头的 slot 和子 slot 名称，可包含字母数字和`*`，`-`、`.`和`+`字符。
