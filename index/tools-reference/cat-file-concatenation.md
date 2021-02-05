# `cat` —— 文件串联

`cat`命令可用于连接两个或多个文件的内容。用法是`cat firstfile secondfile ...`。

## 滥用 `cat`

如果你发现自己要在 ebuild 中使用`cat`，请停止并重新考虑。因为这几乎总是没有必要的。

格式为`cat somefile | somecommand`的所有用法都是愚蠢的，应避免使用。形式`somecommand < somefile`执行相同的操作，并且不涉及额外的复制和管道。使用许多标准实用程序下`somecommand somefile`形式也可以使用。

也不需要使用`foo=$(cat somefile)`将文件内容放入变量`foo`中。命令`foo=$(<somefile)`同样有效，不需要派生。同样，`cat file | xargs cmd`和`xargs cmd < file`可以替换为`cmd $(<file)`。

最后，`cat foo > bar`在 foo 是单个文件的情况下，通常可以用`cp -f foo bar`代替。

## here 文档

另一方面，`cat`它对于所谓的“here”文档非常有用，例如 `sendmail-8.13.3ebuild` 中的以下示例 ：

```bash
src_install() {
	# ...
	cat <<- EOF > "${D}/etc/mail/trusted-users" || die
		# trusted-users - users that can send mail as others without a warning
		# apache, mailman, majordomo, uucp are good candidates
	EOF
	# ...
}
```

在此示例中，`src_install`中使用`cat`创建`${D}/etc/mail/trusted-users`文件。具体来说，新文件将包含`cat`行和 ebuild 中带有`EOF`的行。

有趣的连字符`<<-`是一种>=bash-2.05 语法，它告诉`cat`剥离前导制表符（请注意，当您要复制示例时，必须用制表符替换前导空格），因此，如 Azarah 简而言之，“我们不会让 ebuilds 从空格效果上看”。对于上述示例中的小文件，此处的一组文档比`${FILESDIR}`中浮动的等效文件束更为优雅，并且更易于维护。如果出于某种原因需要保留前导空格，则只需使用`<<`而不是`<<-`即可获得所需的效果。
