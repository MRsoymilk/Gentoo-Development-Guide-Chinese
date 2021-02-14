# sed —— 流编辑器

有时，最好使用正则表达式来操纵内容，而不是修补源。这可用于较小的更改，尤其是那些可能在各个版本之间造成补丁冲突的更改。规范的方法是通过 `sed`：

```bash
# This plugin is mapped to the 'h' key by default, which conflicts with some
# other mappings. Change it to use 'H' instead.
sed -i 's/\(noremap <buffer> \)h/\1H/' info.vim \
	|| die 'sed failed'
```

另一个常见的示例是附加一个`-gentoo-blah`版本字符串（某些像我们这样的上游公司这样做，以便他们可以准确知道要处理的软件包）。同样，我们可以使用`sed`。请注意，如果我们的版本中没有`-r`组件，则`${PR}`变量将设置为`r0`。

```bash
# Add in the Gentoo -r number to fluxbox -version output. We need to look
# for the line in version.h.in which contains "__fluxbox_version" and append
# our content to it.
if [[ "${PR}" == "r0" ]] ; then
	suffix="gentoo"
else
	suffix="gentoo-${PR}"
fi
sed -i \
    -e "s~\(__fluxbox_version .@VERSION@\)~\1-${suffix}~" \
    version.h.in || die "version sed failed"
```

也可以从现有文件中提取内容以这种方式创建新文件。许多 `app-vime` ebuild 使用此技术从插件文件中提取文档并将其转换为 Vim 帮助格式。

```bash
# This plugin uses an 'automatic HelpExtractor' variant. This causes
# problems for us during the unmerge. Fortunately, sed can fix this
# for us. First, we extract the documentation:
sed -e '1,/^" HelpExtractorDoc:$/d' \
	"${S}"/plugin/ZoomWin.vim > ${S}/doc/ZoomWin.txt \
	|| die "help extraction failed"
# Then we remove the help extraction code from the plugin file:
sed -i -e '/^" HelpExtractor:$/,$d' "${S}"/plugin/ZoomWin.vim \
	|| die "help extract remove failed"
```

以下是使用 `sed` 的更常用方法的摘要以及常用地址和代表模式的描述。请注意，其中的某些构造特定于 `GNU sed 4` —— 在非 GNU 用户区的架构上，`sed` 命令必须别名为 `GNU sed`。另请注意，保证将`GNU sed 4` 作为`@system` 的一部分进行安装。这并非总是如此，这是为什么某些软件包（尤其是使用 `sed -i` 的软件包）在`>=sys-apps/sed-4` 上具有 `DEPEND` 的原因。

## 基本 `sed` 调用

调用的基本形式为：

```bash
sed [ option flags ] \
	-e 'first command' \
	-e 'second command' \
	-e 'and so on' \
	input-file > output-file \
	|| die "Oops, sed didn't work!"
```

对于输入和输出文件相同的情况，应使用 inplace 选项。这是通过`-i`作为选项标志之一传递来完成的。

通常 `sed` 会打印出所创建内容的每一行。要仅获取显式打印的行， 应使用`-n`标志。

<div class="alert alert-note">
<b>注意</b>：  术语<i>模式（pattern）</i>是指要匹配的文本的描述。
</div>

## 使用`sed`的简单文本替换

`sed`的最常见形式是用`different content`替换`some text`的所有实例。如下：

```bash
# replace all instances of "some text" with "different content" in
# somefile.txt
sed -i -e 's/some text/different content/g' somefile.txt || \
	die "Sed broke!"
```

<div class="alert alert-note">
<b>注意</b>： <code><pre>/g</pre></code>标志是替换所有匹配项所必需的。如果没有此标志，则仅替换每行的第一个匹配项。
</div>

<div class="alert alert-warning">
<b>警告</b>：以上将替换<code><pre>irksome texting</pre></code>为 <code><pre>irkdifferent contenting</pre></code>，这可能是不希望的。
</div>

如果模式或替换字符串包含正斜杠字符，则通常最容易使用其他定界符。允许使用大多数标点符号，但是应避免使用反斜杠和任何形式的括号。

```bash
# replace all instances of "/usr/local" with "/usr"
sed -i -e 's~/usr/local~/usr~g' somefile.txt || \
	die "sed broke"
```

通过使用`^`和`$`元字符，可以使模式仅在行的开头或结尾进行匹配。 `^`表示“仅在行的开头匹配”，`$`表示“仅在行的结尾匹配”。通过在单个语句中同时使用两者，可以匹配精确的行。

```bash
# Replace any "hello"s which occur at the start of a line with "howdy".
sed -i -e 's!^hello!howdy!' data.in || die "sed failed"
```

<div class="alert alert-note">
<b>注意</b>：这里<code><pre>!g</pre></code>不需要后缀。
</div>

```bash
# Replace any "bye"s which occur at the end of a line with "cheerio!".
sed -i -e 's,bye$,cheerio!,' data.in || die "sed failed"
```

```bash
# Replace any lines which are exactly "change this line" with "have a
# cookie".
sed -i -e 's-^change this line$-have a cookie-' data.in || die "Oops"
```

要忽略模式中的大小写，请添加`/i`标志。

```bash
# Replace any "emacs" instances (ignoring case) with "Vim"
sed -i -e 's/emacs/Vim/gi' editors.txt || die "Ouch"
```

<div class="alert alert-warning">
<b>警告</b>：使用反向引用时，不区分大小写的匹配无法正常工作。
</div>

## 使用`sed`替换正则表达式

使用`sed`也可以进行更复杂的匹配。一些示例可能是：

- 匹配任意三位数
- 匹配“foo”或“bar”
- 匹配任何字母“a”，“e”，“i”，“o”或“u”

这些类型的模式可以链接在一起，导致诸如“匹配任何元音，后跟两位数字，后跟 foo 或 bar”之类的事情。

要匹配一组字符中的任何一个，可以使用字符类。这些有三种形式。

- 反斜杠后跟一个字母。 例如`\d`，匹配一个数字（0、1、2，... 9 中的任何一个）。`\s`匹配单个空格字符。本文档后面将提供更有用的类的表格。
- 方括号内的一组字符。例如，`[aeiou]`匹配“a”，“e”，“i”，“o”或“u”中的任何一个。允许使用范围，例如`[0-9A-Fa-fxX]`，可用于匹配任何十六进制数字或字符“x”和“X”。反向字符类（例如`[^aeiou]`）匹配*除列出的字符外*的任何单个字符。
- POSIX 字符类是具有区域设置意识的特殊命名的字符组。例如，`[[:alpha:]]`匹配当前语言环境中的任何“字母”字符。本文档后面将提供更有用的类的表格。

<div class="alert alert-note">
<b>注意</b>： 正则表达式 <code><pre>a[^b]</pre></code>并<b>不能</b>意味着“匹配，只要它没有一个‘B’后”。它的意思是“匹配一个后跟一个不是“b”的字符”。当考虑以字符“a”结尾的行时，这一点很重要。
</div>

<div class="alert alert-note">
<b>注意</b>： 在撰写本文时，<code><pre>sed</pre></code>文档（<code><pre>man sed</pre></code>和 <code><pre>sed.info</pre></code>）并未提及支持POSIX字符类。请参阅<a href="https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap09.html#tag_09_03">IEEE Std 1003.1-2017, section 9.3</a>，以获取有关它们应如何工作<code><pre>sed</pre></code>的完整详细信息以及有关其<i>实际上如何工作</i>的完整详细信息的源代码。
</div>

要匹配多个选项中的任何一个，可以使用*轮流*。基本形式是 `first\|second\|third`。

为了对项目进行分组以避免歧义，可以使用`\(parentheses\)`构造。要匹配“iniquity”或“infinity”，可以使用 `in\(iqui\|fini\)ty`。

要选择匹配项，请在其后添加一个`\?`。例如，同时 `colou\?r` 匹配“colour”和“color”。这也可以应用于括号中的字符类和组，例如`\(in\)\?finite\(ly\)\?`。可以使用其他原子来匹配“一个或多个”，“零个或多个”，“至少 n”，“n 和 m 之间”等等，这些在本文的后面进行了概述。

还有一些特殊的构造可用于替代命令的替代部分。要插入模式的第一个匹配括号组的内容，请使用`\1`，将其用于第二个使用`\2`，以此类推，直到`\9`。可以使用未转义的`&`字符插入匹配的全部内容。这些和其他取代原子在本文档的后面进行了概述。

## `sed`中的地址

许多`sed`命令只能应用于特定的行或行范围。例如，如果希望只对文档的前十行进行操作，这可能会很有用。

地址的最简单形式是单个正整数。这将导致以下命令仅应用于相关行。行编号从 1 开始，但是当希望在第一行*之前*插入文本时，地址 0 可能会有用。如果在 50 行文档中使用地址 100，则将永远不会执行关联的命令。

为了匹配文档中的最后一行，可以使用`$`地址。

为了匹配与给定正则表达式匹配的任何行，允许使用格式`/pattern/`。这对于查找特定的行然后对其进行某些更改很有用-有时分两步来处理它比使用一个大的`s///`命令更简单。在范围内使用时，对于查找两个给定标记之间或给定标记与文档末尾之间的所有文本可能很有用。

为了匹配地址范围，可以使用`addr1`，`addr2`。起始地址和结束地址都允许使用大多数地址构造。

地址可能带有感叹号而反转。要匹配除最后一行以外的所有行，可以使用`$!`。

最后，如果没有为命令提供地址，则该命令将应用于输入中的每一行。

<div class="alert alert-note">
<b>注意</b>：GNU <code><pre>sed</pre></code> 并没有支持<code><pre>%</pre></code>其他一些实施中发现的地址形式。它也不支持<code><pre>/addr/+offset</pre></code>，那是 <code><pre>ex</pre></code> 一回事...
</div>

还可以使用其他更复杂的选项，包括链接地址。这些没有在本文讨论。

## 使用`sed`删除内容

可以使用 `address d` 命令从文件中删除行。要删除文件的第三行，可以使用 `3d`，使用`/fred/d`过滤掉所有包含“fred”的行。

<div class="alert alert-note">
<b>注意</b>： 用 sed -e <i><code><pre>/fred/d</pre></code></i> 是<i>不一样</i>的 —— 前者将删除线，包括新行，而后者将删除线的内容，但不换行。 <code><pre>s/.fred.//</pre></code>
</div>

## 使用`sed`提取内容

将`-n` 选项传递给`sed`时，默认情况下不打印任何输出。`p` 命令可用于显示内容。例如，要打印包含“红外线猴子”的行，可以使用`sed -n -e '/infra monkey/p'`命令。也可以打印范围 —— `sed -n -e '/^START$/,/^END$/p'`有时很有用。

## 使用`sed`插入内容

要使用 `sed` 插入文本，请使用 `address a` 或 `i` 命令。`a` 命令插入在匹配之后的行中，而 `i` 命令插入在匹配之前的行中。

通常，地址可以是行号或正则表达式：行号命令将只执行一次，并且每次匹配都会执行正则表达式插入/追加。

```bash
# Add 'Bob' after the 'To:' line:
sed -i -e '/^To: $/a    Bob' data.in || die "Oops"

# Add 'From: Alice' before the 'To:' line:
sed -i -e '/^To: $/i    From: Alice'

# Note that the spacing between the 'i' or 'a' and 'Bob' or 'From: Alice' is simply ignored'

# Add 'From: Alice' indented by two spaces: (You only need to escape the first space)
sed -i -e '/^To: $/i\  From: Alice'
```

请注意，应尽可能使用匹配项而不是行号。如果在文件的开头添加了一行，则可以减少问题，例如，导致 sed 脚本中断。

## `sed`中的原子正则表达式

### 基本原子

| **原子**              | **目的**                 |
| :-------------------- | :----------------------- |
| `text`                | 文字                     |
| `\( \)`               | 分组                     |
| <code>\\&#124;</code> | 交替，a _或_ b           |
| `- \? \+ \{\}`        | 重复，见下文             |
| `.`                   | 任何单个字符             |
| `^`                   | 行首                     |
| `$`                   | 行结束                   |
| `[abc0-9]`            | 任何一个                 |
| `[^abc0-9]`           | 除以下任何一个字符       |
| `[[:alpha:]]`         | POSIX 字符类，请参见下文 |
| `\1 .. \9`            | 反向引用                 |
| `\x （任何特殊字符）` | 从字面上匹配字符         |
| `\x （正常字符）`     | 快捷方式，请参见下文     |

### 字符类简写

| **原子** | **描述**                         |
| :------- | :------------------------------- |
| `\a`     | “BEL”字符                        |
| `\f`     | “换页”字符                       |
| `\t`     | “制表符”字符                     |
| `\w`     | “单词”（字母，数字或下划线）字符 |
| `\W`     | “非单词”字符                     |

### POSIX 字符类

阅读源代码，这是正确记录这些信息的唯一位置。

| **类**         | **描述**               |
| :------------- | :--------------------- |
| `[[:alpha:]]`  | 字母字符               |
| `[[:upper:]]`  | 大写字母               |
| `[[:lower:]]`  | 小写字母               |
| `[[:digit:]]`  | 位数                   |
| `[[:alnum:]]`  | 字母和数字字符         |
| `[[:xdigit:]]` | 十六进制数字允许的数字 |
| `[[:space:]]`  | 空格字符               |
| `[[:print:]]`  | 可打印字符             |
| `[[:punct:]]`  | 标点符号               |
| `[[:graph:]]`  | 非空白字符             |
| `[[:cntrl:]]`  | 控制字符               |

### 计数说明符

| **原子**  | **描述**                  |
| :-------- | :------------------------ |
| `\*`      | 零或更多（贪婪）          |
| `\+`      | 一个或多个（贪婪）        |
| `\?`      | 零或一（贪婪）            |
| `\{N\}`   | 恰好 N                    |
| `\{N,M\}` | 至少 N 且不超过 M（贪婪） |
| `\{N,\}`  | 至少 N（贪婪）            |

### 中的替换原子 sed

| **原子**   | **描述**                   |
| :--------- | :------------------------- |
| `\1 .. \9` | 捕获的`\( \)`内容          |
| `&`        | 整个匹配文本               |
| `\L`       | 所有后续字符都将转换为小写 |
| `\l`       | 以下字符转换为小写         |
| `\U`       | 所有后续字符都将转换为大写 |
| `\u`       | 以下字符转换为大写         |
| `\E`       | 取消最近的`\L` 或`\U`      |

### `sed` 匹配机制的详细信息

GNU `sed`使用具有扩展功能的传统（非 POSIX）不确定性有限自动机来支持捕获以进行匹配。这意味着在所有情况下，都将偏向最开始位置的比赛。在所有最左边的匹配项中，将优先考虑最左边的交替选项。最后，所有其他条件相同的事物将被赋予最长的最左侧计数选项。

其中大多数违反了严格的 POSIX 合规性，因此最好不要依赖它。*可以肯定*地认为`sed`总是会选择最左边的匹配项，并且它会优先考虑模式中较早的项目而贪婪地进行匹配。

## 关于`sed`性能的注意事项

<div class="alert alert-note">
<b>代办事项</b>：写
</div>

## 建议对正则表达式进行进一步阅读

对于那些想了解更多关于正则表达式的人， 作者建议使用 Jeffrey EF Friedl 的 _Mastering Regular Expressions_。该文本明显没有诸如“让 `t` 是这样的有限连续序列”之类的短语 `t[n] ∈ ∑ ∀ n`，并且*不是*由使用数学和希腊符号的人所写。
