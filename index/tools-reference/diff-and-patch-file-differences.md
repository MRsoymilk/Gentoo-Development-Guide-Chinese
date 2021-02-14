# `diff`和`patch` —— 文件差异

`diff` 工具用于创建补丁（有时称为差异）。修补程序是一种用于修改一个或多个文件中的文本程序（计算机科学定义）。通常，这些代码用于在编译源代码之前对其进行更改。

最简单的调用是 `diff -u oldfile newfile`，它将在`oldfile` 和`newfile`之间创建统一格式的差异列表。要改用目录进行操作，请使用 `diff -urN olddir newdir`。

<code><pre></pre></code>

<div class="alert alert-note">
<b>注</b>：VCS像<code><pre>git</pre></code>，<code><pre>svn</pre></code> 或者 <code><pre>cvs</pre></code> 提供内置的diff函数（<code><pre>git diff</pre></code>，<code><pre>svn diff</pre></code>，<code><pre>cvs diff</pre></code>），这可能是更有用的。
</div>

对于主 tree 中的补丁，请使用统一（`-u`）格式。通常，这也是向上游发送补丁程序时使用的最佳格式 —— 但是，有时可能会要求你提供上下文差异，它们比统一性更可移植（但不能像处理干净冲突那样）。在这种情况下，请使用`-c` 而不是`-u`。

要应用补丁，请使用 `patch -pX < whatever.patch`，其中 `X` 的数字代表必须删除的路径组件的数量（通常为 `0`或` 1`）。在 ebuild 中，从 EAPI=6 开始请使用 `epatch`函数 或 `eapply`函数 —— 请参阅 [使用 epatch 和 eapply 进行修补](./../ebuild-writing/ebuild-phase-functions/src_prepare/patching-with-epatch-and-eapply.md)。

diff-1 和 patch-1 手册页提供了更多信息。
