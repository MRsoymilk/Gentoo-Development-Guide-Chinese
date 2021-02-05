# `find` —— 查找文件

`find` 实用程序可用于在与一组给定条件匹配的文件组上搜索并执行命令。基本用法是 `find path rules`。

为了便于移植，请**始终**指定路径。`find` 如果没有提供路径，请不要依赖默认值到当前工作目录。

有用的规则包括：

| **规则**            | **效果**                                                                                        | **POSIX?**      |
| :------------------ | :---------------------------------------------------------------------------------------------- | :-------------- |
| `-name "blah"`      | 仅找到名为`blah`的文件。该`*`和`?`可以使用通配符，但应以引号包裹`-name 'blah*'`。               | 是              |
| `\! -name "blah"`   | 仅查找不为`blah`的文件 。                                                                       | 是              |
| `-type f`           | 仅查找常规文件，而不查找目录。                                                                  | 是              |
| `-type d`           | 仅查找目录。                                                                                    | 是              |
| `-type l`           | 仅查找符号链接。                                                                                | 是              |
| `-user foo`         | 仅查找属于 user 的`foo`文件 。最好使用命名用户，而不是 UID。root 是在某些系统上可能不是 UID 0。 | 是              |
| `-group foo`        | 仅查找属于组`foo` 的文件 。最好使用组命名而不是 GID。                                           | 是              |
| `-maxdepth 3`       | 子目录最大查找深度为 3 级。 `-maxdepth 1` 将忽略指定路径的所有子目录。                          | 否，GNU 和 BSD  |
| `-mindepth 2`       | 发生匹配之前，请忽略前 2 个目录级别。 `-mindepth 1` 将处理除命令行参数以外的所有文件。          | 否，GNU 和 BSD  |
| `-exec foo '{}'`    | 执行命令。`{}`替换为当前匹配文件的名称。请参见下面的示例。                                      | 是              |
| `-execdir foo '{}'` | 与`-exec`相同，但从匹配的基础中运行命令。                                                       | 否，GNU 和 BSD  |
| `-delete`           | 删除匹配项。                                                                                    | 否，GNU 和 FBSD |
| `-print0`           | 在输出中用 NUL 字符而不是换行符分隔文件名。这对于防止文件名中可能包含换行符的文件很有用。       | 否，GNU 和 FBSD |

<div class="alert alert-note">
<b>注意</b>： EAPI&gt;=5假定使用GNU find，因此在ebuild上下文中为那些EAPI使用GNU扩展是安全的。
</div>

默认情况下，`find` 会将匹配文件列表回显到以换行符分隔的标准输出。可以将其与 `for` 循环结合使用，以存储少量文件，且文件名中没有奇怪的字符和空格：

```bash
for f in $(find "${S}" -type f) ; do
	einfo "Doing unholy things to ${f}"
done
```

上面的循环可能会导致名称中包含特殊字符的文件出现问题。 更好的方法是在`while`循环中使用`-print0`选项运行`find`（注意传递给`while`和`read`的更改定界符选项）：

```bash
while IFS="" read -d $'\0' -r f ; do
	einfo "Calling down holy vengance upon ${f}"
done < <(find "${S}" -type f -print0)
```

或者，你可以使用`-exec` 参数。小心转义，以确保 `bash` 不会吞噬特殊字符：

```bash
find "${S}" -name '*.data' -exec mv '{}' "${S}/data/" \;
```

当`-exec` 以`;`字符（需要转义或引号）结尾时，则会为每个匹配项分别构建命令行。如果以`+`字符结尾，则通过在每个末尾附加每个选定的文件名来构建命令行。

如果要运行的命令接受多个参数（例如`doins`），并且在这种情况下效率更高，则以下语法很有用：

```bash
find "${S}" -name '*.so*' -exec doexe '{}' +
```

查找还支持否定匹配：

```bash
find "${S}/bundled-libs" \! -name 'libbass.so' -delete
```

这将删除“ bundled-libs”文件夹中的所有文件，但“ libbass.so”除外。确保你始终转义该!字符，以便外壳程序不会对其进行解释。

<div class="alert alert-warning">
<b>警告</b>：由于<a href="https://www.gnu.org/software/findutils/manual/html_mono/find.html#Race-Conditions-with-_002dexec">竞争条件</a>，参数<code><pre>-exec</pre></code>存在安全性问题（对于<code><pre>find . -print0 | xargs ...</pre></code>构造也是如此）。在ebuild上下文中这应该不是问题，因为目录通常不能由随机用户写入。但是，你应该考虑使用<code><pre>-execdir</pre></code>替代，该方法从匹配的目录内运行命令（请注意，<code><pre>-execdir</pre></code> 不是POSIX，请参见表）。
</div>

有关更多详细信息和示例，请参见 [IEEE Std 1003.1-2017-find](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/find.html)。
