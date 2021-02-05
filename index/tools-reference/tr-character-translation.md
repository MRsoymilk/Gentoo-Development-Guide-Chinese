# tr —— 字符转换

`tr`命令可用于转换，压缩和删除字符序列。有关完整规范，请参见`man tr`和 [IEEE Std 1003.1-2017-tr](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/tr.html)。

 <div class="alert alert-note">
<b>注意</b>：<code><pre>tr</pre></code>与大多数其他实用程序不同，它仅从标准输入读取，而仅写入标准输出。因此，你将不得不使用<code><pre>tr [options] < input > output</pre></code> 。
</div>

`tr` 根据调用的方式以多种模式运行：

**删除字符**

> 要删除所有出现的某些字符，请使用 `tr -d asdf`。

**删除重复字符**

> 要将重复的字符替换为单个字符（‘压缩’），请使用 `tr -s asdf`。

**转换字符**

> 要将所有‘a’字符替换为‘1’，将所有‘b’替换为‘2’，并将所有‘c’替换为‘3’，请使用 `tr abc 123`。

参数允许使用某些特殊形式。`a-z` 扩展为从'a'到'z'的所有字符，`\t` 代表制表符，依此类推。请参阅文档以获取完整列表。
