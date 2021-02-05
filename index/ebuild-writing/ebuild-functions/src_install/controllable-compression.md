# 可控压缩

可以在`src_install`中调用`docompress`函数来控制目标文件夹`${D}`中的哪些项目应该被压缩，哪些不应该被压缩。你可以加入或去除目录或纯文件。默认的包含列表包含：

- `/usr/share/doc`
- `/usr/share/info`
- `/usr/share/man`

默认排除列表包含：

- `/usr/share/doc/${PF}/html`

如果目录位于其中或不在其中，则给定目录中的所有文件和目录都应添加到相应的列表中。如果一个文件位于其中或不在其中，则该文件应添加到相应的列表中（排除要比包含要强——如果文件在两个列表中都将被忽略）。

如果第一个参数 `docompress` 是`-x`，则指定的项目将被添加到排除列表中，否则它们将被添加到包括列表中。

<div class="alert alert-note">
<b>注</b>： 当 <code><pre>docompress</pre></code> 被调用时，它<i>不需要</i>指定为它的参数的路径指向现有的文件或目录。但是，如果<code><pre>src_install</pre></code>完成后文件仍然不存在 ，则会通过警告将其忽略。
</div>
