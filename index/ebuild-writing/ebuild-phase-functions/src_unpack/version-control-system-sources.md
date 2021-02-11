# 版本控制系统（VCS）源

除了使用源代码压缩包之外，还可以直接使用上游源代码存储库。当需要定期测试未发布的快照时这很有用。为此，存在许多 eclass。有关可用函数和变量的列表，请参见其文档。

- [bzr.eclass](./../../../eclass-reference.md)
- [cvs.eclass](./../../../eclass-reference.md)
- [darcs.eclass](./../../../eclass-reference.md)
- [git-r3.eclass](./../../../eclass-reference.md)
- [mercurial.eclass](./../../../eclass-reference.md)
- [subversion.eclass](./../../../eclass-reference.md)

<div class="alert alert-note">
<b>注意</b>： VCS ebuild必须使用空<code><pre>KEYWORDS</pre></code>，但不能使用package.masked（除非掩码是出于独立原因）。通常，应该完全省略<code><pre>KEYWORDS</pre></code>行。这适用于“实时” ebuild（<code><pre>-9999</pre></code>）以及提取静态修订版但仍使用版本控制系统进行提取的ebuild。
</div>

## VCS 源的缺点

请注意，出于以下原因，通常**不应**将 VCS ebuilds 添加到 tree 中：

- 上游 VCS 服务器的可靠性往往不如我们的镜像系统可靠。
- VCS ebuilds 会给服务器带来沉重的负担——不仅没有对存储库进行镜像，而且从服务器中获取源代码对于服务器来说要比通过 HTTP 或 FTP 提供文件简单得多。
- 存储库的本地副本比相同来源的压缩包的本地副本大几倍，并且由于包含历史记录，因此往往会随着时间的增长而增长。
- 许多位于严格防火墙之后的用户无法使用 CVS 之类的协议。

制作快照更安全（对用户而言更好）。例如，使用以下命令制作`app-editors/emacs`快照：

```bash
$ git archive --prefix=emacs/ HEAD | xz > emacs-${PV}.tar.xz
```

## VCS Live Sources 的缺点

针对最新标题（或提示）而不是特定日期或修订版本的 VCS ebuilds 甚至更不适合添加到 tree 中：

- 你永远无法确定上游的源头是否会在任何给定的点上实际建立。这很可能会导致质量检查问题。
- 当你无法安装与报告程序相同版本的软件包时，要跟踪错误非常困难。
- 如果人们不使用特定的发行版本，许多上游软件包维护者往往会感到沮丧。
