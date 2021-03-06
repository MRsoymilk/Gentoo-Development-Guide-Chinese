# `cut` —— 列串联

`cut` 工具可用于从文件中提取特定列，这些文件由特定字符或列号分隔。可以在命令行中传递文件名。如果未指定，则从标准输入读取。

`cut` 工具认为一行中的第一个字符具有索引 `1`。`-c`，`-f` 和`-b` 开关采取的参数列出所需的列。这可以是单个值，也可以是更复杂的值列表，以逗号分隔。每个值可以是一个数字，也可以是两个数字，并用表示连字符的连字符分隔 `low-high`。如果 `low` 未指定，则将其视为第一列。如果 `high` 未指定，则将其视为“直到最后一个字符（包括最后一个字符）”。

要从每一行中选择特定字符，请使用`-c` 开关。对于特定字节（使用多字节文本时与字符不同），请使用`-b`。要指定特定字段，请使用`-f`。

使用`-f`时，可以使用`-d` 开关指定字段分隔符。默认值为制表符。`-s` 开关指示 `cut` 禁止不包含任何定界符实例的行 —— 默认情况下，它们将完整地回显。

例如，要提取以逗号分隔的文件中的第二，第四和第五列，而忽略不包含逗号的行，则可以使用：

```bash
cut -s -d , -f 2,4-5 input.txt > output.txt
```

要将第一个字符从标准输入中去除，可以使用：

```bash
do_stuff | cut -c 2-
```

有关完整的文档，请参见 cut 手册页和 [IEEE Std 1003.1-2017-cut](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/cut.html)。
