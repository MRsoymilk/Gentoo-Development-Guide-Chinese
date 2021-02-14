# bash —— 标准 Shell

使用 ebuild 时，对`bash`编程的透彻理解至关重要。

## bash 条件

### 基本选择

基本的条件运算符是`if` 语句：

```bash
if something ; then
	do_stuff
fi
```

### 多项选择

可以使用`else`和`elif`进行多项选择 ：

```bash
if something ; then
	do_stuff
elif something_else ; then
	do_other_stuff
elif full_moon ; then
	howl
else
	turn_into_a_newt
fi
```

<div class="alert alert-warning">
<b>警告</b>：你<b>必须</b>在每个块中至少指定一个语句。下面将<b>不</b>工作：
</div>

```bash
if some_stuff ; then
	# A statement is required here. a blank or a comment
	# isn't enough!
else
	einfo "Not some stuff"
fi

```

如果你确实不想重组该块，则可以单独使用一个冒号（`:`）作为空语句。

```bash
if some_stuff ; then
	# Do nothing
	:
else
	einfo "Not some stuff"
fi

```

### 选择测试

要进行比较或文件属性测试，`[ ]`或`[[ ]]`需要块。

```bash
# is $foo zero length?
if [[ -z "${foo}" ]] ; then
	die "Please set foo"
fi

# is $foo equal to "moo"?
if [[ "${foo}" == "moo" ]] ; then
	einfo "Hello Larry"
fi

# does "${ROOT}/etc/deleteme" exist?
if [[ -f "${ROOT}/etc/deleteme" ]] ; then
	einfo "Please delete ${ROOT}/etc/readme manually!"
fi

```

## bash 中的单括号与双括号

<div>
<b>重要提示：</b>该<code><pre>[[ ]]</pre></code>形式通常比<code><pre>[ ]</pre></code>所有新代码都更安全，应该在所有新代码中使用。
</div>

这是因为`[[]]`是 bash 语法的构造，而`[]`是恰巧是内部实现的程序 —— 因此，前者可以使用更简洁的语法。为简单起见，请考虑：

```bash
bash$ [ -n $foo ] && [ -z $foo ] && echo "huh?"
huh?
bash$ [[ -n $foo ]] && [[ -z $foo ]] && echo "huh?"
bash$
```

## `bash`字符串比较

字符串比较的一般形式是`string1 operator string2`。以下是可用的：

| **操作**      | **目的**                                                      |
| :------------ | :------------------------------------------------------------ |
| `==`(或者`=`) | 字符串相同                                                    |
| `!=`          | 字符串不同                                                    |
| `<`           | 字符串字法比较（之前）                                        |
| `>`           | 字符串字法比较（之后）                                        |
| `=~`          | 字符串正则表达式匹配（**仅 bash 3**，当前在 ebuild 中不允许） |

### `bash`字符串测试

字符串测试的一般形式是`-operator "string"`。以下是可用的：

| **操作**      | **目的**       |
| :------------ | :------------- |
| `-z "string"` | 字符串长度为零 |
| `-n "string"` | 字符串长度非零 |

<div class="alert alert-note">
<b>注意</b>： 要检查是否设置了变量而不是空白，请使用<code><pre>-n "${BLAH}"</pre></code>而不是<code><pre>-n $BLAH</pre></code>。如果未设置变量，则后者在某些情况下会引起问题。
</div>

### `bash`中的整数比较

整数比较的一般形式是 `int1 -operator int2`。以下是可用的：

| **操作** | **目的**       |
| :------- | :------------- |
| `-eq`    | 整数相等       |
| `-ne`    | 整数不等式     |
| `-lt`    | 整数小于       |
| `-le`    | 小于或等于整数 |
| `-gt`    | 整数大于       |
| `-ge`    | 大于或等于整数 |

### `bash`文件测试

文件测试的一般形式是`-operator "filename"`。以下是可用的（来自 `man bash`）：

| **操作**  | **目的**                                        |
| :-------- | :---------------------------------------------- |
| `-a file` | 存在（`-e` 替代）                               |
| `-b file` | 存在并且是块特殊文件                            |
| `-c file` | 存在并且是字符特殊文件                          |
| `-d file` | 存在并且是目录                                  |
| `-e file` | 存在                                            |
| `-f file` | 存在并且是常规文件                              |
| `-g file` | 存在且为组标识(set-group-id)                    |
| `-h file` | 存在并且是符号链接                              |
| `-k file` | 存在并且其粘性位已设置                          |
| `-p file` | 存在并且是命名管道（FIFO）                      |
| `-r file` | 存在且可读                                      |
| `-s file` | 存在且大小大于零                                |
| `-t fd`   | 描述符 fd 已打开并指向终端                      |
| `-u file` | 存在并且其设置用户标识位(set-user-id bit)已设置 |
| `-w file` | 存在且可写                                      |
| `-x file` | 存在并且可执行                                  |
| `-O file` | 存在并由有效用户 id 拥有                        |
| `-G file` | 存在并由有效组 id 拥有                          |
| `-L file` | 存在并且是符号链接                              |
| `-S file` | 存在并且是一个套接字                            |
| `-N file` | 存在，自上次阅读以来已被修改                    |

### `bash`文件比较

文件比较的一般形式是`"file1" -operator "file2"`。以下是可用的（来自 `man bash`）：

| **操作**          | **目的**                                                              |
| :---------------- | :-------------------------------------------------------------------- |
| `file1 -nt file2` | file1 比 file2 更新（根据修改日期），或者 file1 存在而 file2 不存在。 |
| `file1 -ot file2` | file1 早于 file2，或者如果 file2 存在而 file1 不存在。                |
| `file1 -ef file2` | file1 和 file2 引用相同的设备和 inode 编号。                          |

### `bash`布尔代数

有一些可用于布尔代数的构造（“和”，“或”和“非”）。这些在块外使用`[[ ]]`。对于运算符优先级，请使用 `( )`。

| 构造                                   | 影响                 |
| :------------------------------------- | :------------------- |
| <code>first &#124;&#124; second</code> | 第一*或*第二（短路） |
| `first && second`                      | 第一*和*第二（短路） |
| `!condition`                           | *非*条件             |

<div class="alert alert-note">
<b>注意</b>： 这些有时也可以在<code><pre>[[ ]]</pre></code>构造内部使用，并且<code><pre>!</pre></code>在测试之前使用是很常见的。<code><pre>[[ ! -f foo ]] && bar</pre></code> 很好。但是，也有意外 —— <code><pre>[[ -f foo && bar ]]</pre></code> 将<b>无法</b>正常工作，因为命令不能在<code><pre>[[ ]]</pre></code>块内部运行。
</div>

在`[ ]`块内部，可以使用几种`-test` 样式布尔运算符。应避免使用`[[ ]]`和上述操作符。

### Bash 迭代结构

`bash`内部有一些简单的迭代结构。其中最有用的是 `for` 循环。这可用于对多个项目执行相同的任务。

```bash
for myvar in "the first" "the second" "and the third" ; do
	einfo "This is ${myvar}"
done
```

`for` 循环的第二种形式可用于将事件重复给定次数。

```bash
for (( i = 1 ; i <= 10 ; i++ )) ; do
	einfo "i is ${i}"
done
```

还有一个 `while` 循环，尽管这在 ebuild 中通常没有用。

```bash
while hungry ; do
	eat_cookies
done
```

这最常用于遍历文件中的行：

```bash
while read myline ; do
	einfo "It says ${myline}"
done < some_file
```

请参阅 [die 和 Subshel​​ls](./../ebuild-writing/error-handling.md) 以获取有关为什么 `while read < file` 要在`cat file | while read`上使用的解释。

## Bash 变量操作

`bash`中有许多特殊的`${}`构造，它们可以根据变量来操纵或返回信息。可以使用这些方法代替对`sed`和类似的高代价外部调用（如果在全局范围内则是非法的）。

## `bash` 字符串长度

`${#somevar}`构造可用于获取字符串变量的长度。

```bash
somevar="Hello World"
echo "${somevar} is ${#somevar} characters long"
```

## `bash` 可变默认值

如果变量未设置或长度为零，则有多种使用默认值的方法。如果设置了`${var-value}`构造，它将扩展为`${var}`的值，并且不为 null，否则为`value`。 `${var-value}`构造类似，但是仅检查是否已设置变量。

`${var=value}`和`${var=value}`形式还将为`var`赋值（如果`var`未设置（也已设置，但`:=`形式为 null））。

`${var:?message}`形式将向 stderr 显示`message`，如果`var`未设置或为 null，则退出。通常不应该在 ebuilds 中使用它，因为它不使用`die`机制。也有一个`${var?message}`形式。

如果设置了`var`且不为 null，则`${var:+value}`格式扩展为`value`，否则扩展为空白字符串。有一个`${var+value}`形式。

## `bash` 子串提取

`${var:offset}`和`${var:offset:length}`构造可用于获取子字符串。字符串是零索引的。`offset`和`length`都是算术表达式。

第一个有具正偏移量的形式返回一个子字符串，该子字符串从偏移量处的字符开始，一直到字符串的结尾。如果`offset`为负，则相对于字符串的*末尾*获取偏移量。

<div class="alert alert-note">
<b>注意</b>：对于不会在这里讨论的原因，任何负值必须是一个表达式，其结果为负值，而不是简单地为负值。最好的方法是使用<code><pre>${var:0-1}</pre></code>。<code><pre${var:-1}></pre></code>将<b>无法</b>正常工作。
</div>

第二种形式返回从偏移量开始的`${var}`值的第一个`length`字符。如果`offset`为负，则从字符串的*末尾*开始获取`offset`。`长度`参数*不得*小于零。同样，负`offset`必须作为表达式给出。

## `bash`命令替换

`$(command)`构造可用于运行命令并将输出（`stdout`）捕获为字符串。

<div class="alert alert-note">
<b>注意</b>：<code><pre>`command`</pre></code>构造也可以这样做，但是为了清楚，易于阅读和嵌套的目的，应避免使用<code><pre>$(command)</pre></code>。
</div>

```bash
myconf="$(use_enable acl) $(use_enable nls) --with-tlib=ncurses"
```

### `bash` 字符串替换

有三种基本的字符串替换形式：`${var#pattern}`， `${var%pattern}`和`${var/pattern/replacement}`。前两个分别用于删除字符串开头和结尾的内容。第三个用于替换具有不同内容的匹配项。

`${var＃pattern}`形式将在删除的`var`值的开头返回`pattern`最短匹配的`var`。如果无法匹配，`var`将被赋值。要在开始处删除*最长*的匹配项，请改用`${var##pattern}`。

`${var%pattern}`和`${var%%pattern}`形式差不多，但分别删除`var`*结束*的最短和最长的匹配。

<div class="alert alert-note">
<b>注意</b>：此处有时会使用术语<i>贪婪（greedy）</i>和<i>非贪婪（non-greedy）</i>（<code><pre>%</pre></code>和<code><pre>#</pre></code>是非贪婪形式）。可以说这是不正确的，但是这些术语非常接近。
</div>

`${var/pattern/replacement}`构造扩展为`var`的值，其中模式的第一个匹配项被替换。要替换*所有*匹配项，可以使用`${var//pattern/replacement}`。

<div class="alert alert-note">
<b>注意</b>：<code><pre>man bash</pre></code>错误地描述了将要匹配的内容。在所有可能的最左边匹配中，将采取最长匹配。是的，确实是最长的，即使涉及偏爱以后的小组或以后的分支机构也是如此。这是<b>不</b>像 <code><pre>perl</pre></code>或<code><pre>sed</pre></code>。有关详细信息 ，请参见<a href="https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap09.html#tag_09_01">IEEE Std 1003.1-2017, section 9.1</a>。
</div>

为了仅在`pattern`出现在`var`值的开头时进行匹配，该模式应以`#`字符开头。要仅在末尾匹配，模式应以`%`开头。

如果`replacement`为 null，则删除匹配项，并且`/`后面的`pattern`可以省略。

`pattern`可以包含许多用于模式匹配的特殊元字符。

<div class="alert alert-note">
<b>代办事项</b>：bash metachars 表
</div>

如果 `extglob` 启用了 shell 选项，则可以使用许多其他构造。这些有时*非常*有用。

<div class="alert alert-note">
<b>代办事项</b>：额外的 bash 使用表
</div>

## `bash` 算术扩展

`$((expression))`构造可用于整数算术评估。 `expression`是类似 C 的算术表达式。支持以下运算符（该表按优先级顺序，从高到低）：

| **操作**                                                                            | **效果**                             |
| :---------------------------------------------------------------------------------- | :----------------------------------- |
| `var++， var-- `                                                                    | 变后递增，后递减                     |
| `++var， --var `                                                                    | 可变的预增，预减                     |
| `!， ~ `                                                                            | 逻辑求反，按位求反                   |
| `**`                                                                                | 求幂                                 |
| `*，/，% `                                                                          | 乘法，除法，余数                     |
| `+， -`                                                                             | 加，减                               |
| `<<， >>`                                                                           | 左，右按位移位                       |
| `<=，>=，<，>`                                                                      | 比较：小于等于，大于等于，小于，大于 |
| `==， != `                                                                          | 平等，不等                           |
| `&`                                                                                 | 按位与                               |
| `^`                                                                                 | 按位异或                             |
| <code>&#124;</code>                                                                 | 按位或                               |
| `&&`                                                                                | 逻辑与                               |
| <code>&#124;&#124;</code>                                                           | 逻辑或                               |
| `expr ? expr : expr`                                                                | 条件运算符                           |
| `=`，`*=`，`/=`，`%=`，`+=`，`-=`，`<<=`， `>>=`，`&=`，`^=`， <code>&#124;=</code> | 分配                                 |
| `expr1 , expr2`                                                                     | 多条陈述                             |

<div class="alert alert-note">
<b>注意</b>： 没有<code><pre>**=</pre></code>赋值运算符。
</div>
