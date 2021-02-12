# 补丁

补丁程序命名没有固定的规则。以下仅是准则。

小补丁（少于几 KB）应添加到`${FILESDIR}`中。 如果您预期会有多个补丁程序，通常会有助于创建版本编号的子目录——`${FILESDIR}/${PV}/`是常规的。 补丁最好命名为`${P}-what-it-does.patch`（或`.diff`），其中，`what-it-does`是由两三个单词组成的补丁说明。 如果补丁程序是为了修复特定的错误，通常添加错误号通常很有用，例如，`vim-7.0-cron-vars-79981.patch`。 如果补丁是从上游的 VCS 存储库中提取的，则可以帮助在补丁名称中包含修订号作为版本部分的后缀——`fluxbox-0.9.12-3860-menu-backups.patch`。

较大的补丁应进行[镜像](./../../general-concepts/mirrors.md)，最好在 Gentoo 基础结构上进行镜像。 在镜像补丁时，选择不会引起冲突的名称很重要——此处强烈建议使用`${P}`前缀。 镜像补丁通常使用`xz`或`bzip2`压缩。 请记住在`SRC_URI`中列出这些补丁。

<div class="alert alert-note">
<b>注意</b>：在<code><pre>${FILESDIR}</pre></code>中包含的补丁切勿压缩。
</div>
<div class="alert alert-warning">
<b>警告</b>：从EAPI=6开始，剥离补丁程序级别仅限于<code><pre>-p1</pre></code>。尽管可以使用<code><pre>eapply -p&lt;strip_level&gt;</pre></code> 命令覆盖它，但还是建议使用补丁本身以使其与<code><pre>-p1</pre></code>默认值兼容。
</div>

如果一个软件包需要很多补丁，即使它们很小，那么通常最好创建补丁压缩包，以免使 tree 变得过于混乱。

## 补丁说明

可以在补丁中包含描述。当其他人开始使用你的软件包时，或者实际上几个月后你回来查看自己的软件包时，这通常会很有帮助。描述中最好要包括的东西是：

- 修补程序实际上是做什么的。错误数字在这里很好。
- 补丁来自哪里。它是上游 VCS 拉取，是 Bugzilla 带来的，还是你编写的？
- 补丁是否已发送到上游（如果适用）。

要包含描述，只需将其插入补丁文件的顶部即可。`patch`工具将忽略开头的文本，直到找到看起来像是“开始修补”指令的内容为止，因此，只要每条描述行都以字母（而不是数字，符号或空格）开头，就不会有问题。或者，在每条内容描述前面加一个井号（即`#`，在 USians 中为“pound”）。最好在描述之后和主补丁之前留一个空白行。

这是 vim 补丁压缩包中的一个简单示例（`023_all_vim-6.3-apache-83565.patch`）：

```bash
# Detect Gentoo apache files properly. Gentoo bug 83565.

--- runtime/filetype.vim.orig   2005-03-25 01:44:12.000000000 +0000
+++ runtime/filetype.vim        2005-03-25 01:45:15.000000000 +0000
@@ -93,6 +93,9 @@
 " Gentoo apache config file locations (Gentoo bug #76713)
 au BufNewFile,BufRead /etc/apache2/conf/*/* setf apache

+" More Gentoo apache config file locations (Gentoo bug #83565)
+au BufNewFile,BufRead /etc/apache2/{modules,vhosts}.d/*.conf setf apache
+
 " XA65 MOS6510 cross assembler
 au BufNewFile,BufRead *.a65                    setf a65
```
