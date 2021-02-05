# `grep` —— 文本过滤

`grep` 工具可用于从文件中提取与给定正则表达式匹配的行，或检查给定正则表达式是否与文件中的任何行匹配。

用法是 `grep "pattern" files`。如果未指定文件，则从标准输入中读取文本。`pattern` 是一个标准的基本正则表达式，如在 [IEEE std 1003.1-2017, section 9.3](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap09.html#tag_09_03)。

如果提供了自变量`-E`，则将 `pattern` 视为扩展的正则表达式，如 [IEEE Std 1003.1-2017, section 9.4](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap09.html#tag_09_04)。

如果提供了`-F` 参数，则将 `pattern` 视为固定字符串而不是正则表达式。

默认情况下，`grep` 从输入中打印出匹配的行。如果指定 `-q` ，则不显示任何输出。如果指定 `-l` （小写字母 ell），则仅显示包含匹配行的文件的文件名。

`-v` 选项可用于选择与模式不匹配的行。

`-s` 选项可用于禁止显示关于不存在或不可读文件的消息。

返回码可用于测试是否发生匹配。返回码 `0` 表示发生了一个或多个匹配；的代码 `1` 表示没有匹配项。

有关详细信息，请参见 [IEEE Std 1003.1-2017-grep](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/grep.html)。GNU 系统上的 grep-1 手册页记录了许多不可移植的附加函数。
