# `head`和`tail` —— 行提取

`head` 和 `tail` 实用程序可用于分别以仅获得一个文件的第一个或最后部分。两者都将从命令行上命名的文件中读取，或者如果没有提供文件，则从 stdin 中读取。

该 `head` 实用程序采用单个参数，`-n` 其后必须跟一个整数，该整数指示要显示的所需行数。

<div class="alert alert-warning">
<b>警告</b>：GNU <code><pre>-c</pre></code>选项的使用不可移植，应避免使用。
</div>

有关详细信息，请参阅 [IEEE Std 1003.1-2017-head](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/head.html)。请注意，GNU 系统上的 head-1 描述了许多不可移植的选项。

该 `tail` 实用程序类似，但是从文件末尾开始。该-n 参数指定要显示的行数。

要指定“最后五行”，请使用 `tail -n 5`。要指定“除前五行外的所有行”，请使用 `tail -n +6`。

```bash
# bad: drop first five lines, clumsily computing line count
tail -n $(($(wc -l in.txt | awk '{print $1}') - 5)) in.txt > out.txt

# good: let tail count lines from the beginning of the file
tail -n +6 in.txt > out.txt
```

<div class="alert alert-warning">
<b>警告</b>：<code><pre>head/tail -5</pre></code>语法不建议使用，并且不兼容POSIX。
</div>

有关完整的详细信息，请参见 [IEEE Std 1003.1-2017-tail](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/tail.html)。请注意，GNU 系统上的 tail-1 描述了许多不可移植的选项。

## `head` 或 `tail` 与 `sed` 链式操作

通常`sed`不需要链接`head`或`tail`。使用地址和提前退出可以通过一个`sed`调用完成相同的操作：

```bash
# bad: get the first five lines of input.txt with all 'foo'
# replaced with 'bar'
head -n 5 input.txt | sed -e 's/foo/bar/g' > output.txt

# good: use sed's address ranges and command groups to do
# the same thing with only one fork
sed -n -e '1,5{ s/foo/bar/g ; p }' input.txt > output.txt

# good: another way is with an extra command which exits
# on line 5
sed -n -e 's/foo/bar/gp ; 5q' input.txt > output.txt

# bad: set foo to the first line containing somestring
foo=$(sed -n -e '/somestring/p' input.txt | head -n 1 )

# good: use early exit to do the same thing in pure sed
foo=$(sed -n -e '/somestring/{ p ; q }' input.txt )

# bad: output the last line matching 'somestring'
sed -n -e '/somestring/p' input.txt | tail -n 1

# good: do this in pure sed using the hold space
sed -n -e '/somestring/h ; $ {x;p}'
```

`tail -n X` 其中 `X` 大于 1 可以用纯`sed`来完成，但比较棘手。 在这里使用链接命令可能是最简单的。

最后，要提取单个特定行，使用`sed`代替：

```bash
sed -n -e '123p'
```
