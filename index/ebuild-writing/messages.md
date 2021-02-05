# 输出信息

有时有必要为用户显示消息。这可能是错误，安装后帮助消息，安装前警告，或者只是用于通知用户发生了什么。例如，在执行任何特别长时间且无声的任务之前，显示消息是一种很好的形式（并且它还有助于减少伪造的“comping foo froze！”错误）。

<div class="alert alert-note">
<b>注意</b>： 使用这些函数中的任何一个来显示一行字符（横幅标题）都是违反政策的。这些函数提供的颜色和特殊前导字符的使用足以使消息突出。
</div>

在所有情况下，假定用户终端的宽度不超过 79 列，并且`elog`，`einfo`，`ewarn`和`eerror`函数将以其花哨的前导标记占据 4 列。

## 信息讯息

这里有许多函数可以提供帮助。 “echo” bash 内部是最简单的 —— 它只是将其参数显示为消息。

`elog`函数可用于显示一条信息消息，该信息旨在“脱颖而出”，并由 Portage 2.1 和 Paludis 0.6 或更高版本中的 elog 功能记录。在彩色终端上，提供的消息将带有绿色星号作为前缀。在早期版本中，elog 的行为类似于 einfo。

```bash
pkg_postinst() {
	elog "You will need to set up your /etc/foo/foo.conf file before"
	elog "running foo for the first time. For details, please see the"
	elog "foo.conf(5) manual page."
}
```

`einfo`函数可用于显示一条信息消息，旨在“脱颖而出”。在彩色终端上，提供的消息将带有绿色星号作为前缀。 einfo 消息转到默认情况下未记录的 INFO elog 类。

```bash
src_compile() {
	einfo "Starting a silent compile that takes hours."
	./build
}
```

## 警告讯息

`ewarn`功能类似，但显示黄色星号。这应该用于警告消息，而不是信息。

## 错误讯息

`eerror`功能显示红色星号，用于显示错误消息。几乎应始终在`die`调用后。此功能主要用于在解决之前显示其他错误详细信息。

## 质量检查警告

eclass 作者可以使用`eqawarn`函数将不推荐使用的功能通知 ebuild 作者。 eqawarn 在 EAPI = 7 之前在 eutils 中定义。默认情况下，Portage 不会记录 qa 消息类，因此用户不会因为看到自己做不到的事情而烦恼。

## 消息函数参考

有关函数的完整列表，请参见[消息函数参考](./../function-reference/message-functions-reference.md)。

## 好消息和坏消息

这是一个错误消息的示例：

```bash
i=10
while ((i--)) ; do
	ewarn "PLEASE UPDATE TO YOUR PACKAGE TO USE linux-info.eclass"
done
```

- 重复显示相同的消息是多余的。
- 大写字母过多。
- 英文不好，看起来很不专业。
- 该消息只会使最终用户感到困惑，而不会帮助他们确定是否有问题以及如何解决。

最好写成：

```bash
ewarn "The 'frozbinate' function provided by eutils.eclass is deprecated"
ewarn "in favour of frozbinate.eclass, but this package has not been"
ewarn "updated yet. If this is a package from the main tree, please check"
ewarn "https://bugs.gentoo.org/ and file a bug if there is not one already."
ewarn "If this is your own package, please read the comments in the"
ewarn "frozbinate eclass for instructions on how to convert."
```
