# 补全文件

从 v2.05a 开始，`bash` 提供了智能的可编程补全。如果你已经学习了 bash，那么为自己维护的程序/事物编写这样的补全相对容易。有关如何安装补全文件，请参见[bash-completion-r1.eclass](./../eclass-reference/bash-completion-r1.eclass.md)。

## 与补全相关的内部 Bash 变量

这些变量中的大多数在取消设置时都会丢失其特殊属性，即使随后将其重置也是如此。

| **变量**          | **目的**                                                                                                  |
| :---------------- | :-------------------------------------------------------------------------------------------------------- |
| `COMP_CWORD`      | `${COMP_WORDS}`包含当前光标位置的单词的索引。                                                             |
| `COMP_LINE`       | 当前命令行。                                                                                              |
| `COMP_POINT`      | 当前光标位置相对于当前命令开头的索引。如果当前光标位置在当前命令的末尾，则此变量的值等于`${#COMP_LINE}`。 |
| `COMP_WORDBREAKS` | Readline 库在执行单词补全时将其视为单词分隔符的字符集。                                                   |
| `COMP_WORDS`      | 一个数组变量，由当前命令行`${COMP_LINE}`中的各个单词组成。                                                |
| `COMPREPLY`       | bash 从中读取补全函数生成的可能补全的数组变量。                                                           |

## 与补全相关的 Bash 内置函数

有关这些内建函数及其选项的完整说明，请参见`man bash`。

| **内置**   | **用法**                                                                                                                                                                                                                                                                                                                                                                    |
| :--------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `compgen`  | `compgen [-abcdefgjksuv] [-o option] [-A action] [-G globpat] [-W wordlist] [-P prefix] [-S suffix] [-X filterpat] [-F function] [-C command] [word]` 根据选项显示可能的补全。旨在从 shell 函数中使用，以生成可能的补全。如果提供了可选的 WORD 参数，则会生成与 WORD 的匹配。                                                                                               |
| `complete` | `complete [-abcdefgjksuv] [-pr] [-o option] [-A action] [-G globpat] [-W wordlist] [-P prefix] [-S suffix] [-X filterpat] [-F function] [-C command] [name ...]` 对于每个 NAME，指定参数的填写方式。如果提供了-p 选项，或者没有提供任何选项，则以允许它们重新用作输入的方式打印现有的补全规范。-r 选项删除每个 NAME 的补全规范，或者，如果未提供 NAME，则删除所有补全规范。 |

对于极其简单的情况， 只需要一个简单的`complete`语句即可。例如，最小 `cd` 补全（最小补全不支持 `${CDPATH}`）将很简单：

```bash
complete -o nospace -d cd
```

## 补全函数的剖析

几乎所有补全函数都将以相同的方式启动。对于这些情况，可以将以下内容用作创建新的补全函数的模板：

```bash
01: _foo() {
02: 	local cur prev opts
03: 	COMPREPLY=()
04: 	cur="${COMP_WORDS[COMP_CWORD]}"
05: 	prev="${COMP_WORDS[COMP_CWORD-1]}"
06: 	opts=""
07:
08: 	if [[ ${cur} == -* || ${COMP_CWORD} -eq 1 ]] ; then
09: 		COMPREPLY=( $(compgen -W "${opts}" -- ${cur}) )
10: 		return 0
11: 	fi
12:
13: 	case "${prev}" in
14: 		# ...
15: 	esac
16: }
17: complete -F _foo foo
```

### 1

完成函数名称的约定通常为\_NAME，其中 NAME 是您为其编写完成函数的应用程序/函数的名称。如果 NAME 包含“-”，则应将其替换为“\_”。因此，bash-completion-config 将是\_bash_completion_config()。如果在环境中包含'-'的函数名在 POSIX 模式下调用 bash（如果 POSIX sh 不允许在函数名中使用'-'），则这样做可能会导致奇怪的错误。

### 3

重置`${COMPREPLY}`数组，因为它可以从先前的调用中设置。

### 4

将局部变量`${cur}`设置为命令行的当前单词。如果当前命令行`${COMP_LINE}`使用'foo --fil'，则`${cur}`等于'--fil'。如果`${COMP_LINE}`等于'foo --file'（请注意末尾的空格），则`${cur}`为 null。

### 5

将局部变量`${prev}`设置为命令行的前一个单词。` ${prev}`是`${cur}`之前的单词。

### 6

设置局部变量`${opts}`。在真正的完成功能中，此变量将设置为 foo 可以识别的所有选项。

### 8

测试当前单词是否等于-\*（一个选项），或者我们是否在第一个单词上完成（即`${COMP_CWORD}` == 1）。

### 9

如果测试返回 true，请显示可用选项`${opts}`。 `compgen`的-W 选项告诉 bash 在单词列表（字符串或计算结果为字符串的东西）上完成。在大多数情况下，你会通过`--${cur}`来告知`compgen`仅返回与`${cur}`匹配的补全。

### 13

在大多数情况下，如果`${prev}`等于某个选项，您将希望执行某种操作。例如，如果`foo`的--file 选项（简称-f）可以接收任何类型的文件，则可以执行以下操作：

```bash
case "${prev}" in
	-f|--file)
		COMPREPLY=( $(compgen -f ? ${cur}) )
	;;
esac
```

### 17

告诉 bash 使用`_foo`函数为`foo`应用程序/函数生成任何补全。

## 真实示例

对于本文档，我将带你看一个真实的示例，并为`revdep-rebuild`编写一个实际的补全函数 （甚至在 `gentoo-bashcomp` 阅读本文时可用 :]）。

```bash
_revdep_rebuild() {
02: 	local cur prev opts
03: 	COMPREPLY=()
04: 	cur="${COMP_WORDS[COMP_CWORD]}"
05: 	prev="${COMP_WORDS[COMP_CWORD-1]}"
06: 	opts="-X --package-names --soname --soname-regexp -q --quiet"
07:
08: 	if [[ ${cur} == -* || ${COMP_CWORD} -eq 1 ]] || \
09: 	   [[ ${prev} == @(-q|--quiet) ]] ; then
10: 		COMPREPLY=( $(compgen -W "${opts}" -- ${cur}) )
11: 		return 0
12: 	fi
13:
14: 	case "${prev}" in
15: 		-X|--package-names)
16: 			_pkgname -I ${cur}
17: 			;;
18: 		--soname)
19: 			local sonames=$(for x in /lib/*.so?(.)* /usr/lib*/*.so\?(.)* ; do \
20: 						echo ${x##*/} ; \
21: 						done)
22: 			COMPREPLY=( $(compgen -W "${sonames}" -- ${cur}) )
23: 			;;
24: 		--soname-regexp)
25: 			COMPREPLY=()
26: 			;;
27: 		*)
28: 			if [[ ${COMP_LINE} == *" "@(-X|--package-names)* ]] ; then
29: 				_pkgname -I ${cur}
30: 				COMPREPLY=(${COMPREPLY[@]} $(compgen -W "${opts}"))
31: 			else
32: 				COMPREPLY=($(compgen -W "${opts} -- ${cur}"))
33: 			fi
34: 		;;
35: 	esac
36: }
37: complete -F _revdep_rebuild revdep-rebuild
```

第 1 至 12 行与上一节大致相同。

### 15

如果`${prev}`等于`-X`或`--package-names`，则调用`_pkgname`（由`gentoo-bashcomp`定义的函数，该函数在软件包名称上补全 —— 因为设置了`${COMPREPLY}`，因此我们在这里不必担心） 。

### 18

如果`${prev}`等于`--soname`，则在`/lib`和`/usr/lib *`中生成所有共享库的列表。将该列表传递给`compgen`以生成与`${cur}`匹配的可能补全列表。

### 24

显然，我们无法补全任何正则表达式，因此如果`${prev}`等于`--soname-regexp`，则不执行任何操作。

### 27

对于其他任何内容（上述 case 语句中未指定的任何选项，或 case 语句中指定的选项之一的任何参数）执行测试。由于`--package-names`可以采用多个软件包名称，因此我们希望继续补全软件包名称，直到遇到另一个可识别的选项（即`${prev}`）为止。

### 30

由于`_pkgname`设置了`${COMPREPLY}`，并且我们想添加到该列表中，因此我们必须使用`COMPREPLY=(${COMPREPLY[@]...)`构造。

### 37

告诉 bash 使用`_revdep_rebuild` 来生成 revdep-rebuild 的所有可能的补全内容。
