# 适用于 Gentoo 开发人员的 Git

本指南涵盖了特定于 Ge​​ntoo ebuild 开发的 git 使用说明和策略。它假定读者具有基本的 git 知识。有关通用指南，请参见官方 [git book](https://git-scm.com/book/)。

## 准备开发

### 克隆 gentoo.git 存储库

ebuild 的开发在官方 git 仓库上进行。你只能通过 SSH 协议推送更改。因此，使用以下命令克隆存储库：

```bash
git clone git@git.gentoo.org：repo/gentoo.git
```

如果你没有对 Gentoo git 服务的 SSH 访问权限，则可以使用 anongit 镜像进行只读访问：

```bash
git clone https://anongit.gentoo.org/git/repo/gentoo.git
```

通常，git 将从 git 仓库开始（2015 年 8 月）获取完整的历史记录。这可能需要大量的磁盘空间。如果不需要完整的历史记录，则可以使用`--depth`选项来创建浅表克隆，仅包括包含最新提交的子集。例如， `--depth=50`将包括 50 个最新的提交。

请注意，使用浅克隆时，需要 git 1.9 或更高版本。

### 特定于 Ge​​ntoo 存储库的配置

为了确保遵循 Gentoo 策略，你应该设置以下配置变量：

```bash
git config --local user.name "${YOUR_FULL_NAME}"
# use your @gentoo.org address even if you have a different default
git config --local user.email "${YOUR_NICKNAME}@gentoo.org"

# enable commit and push signing
git config --local user.signingkey "0x${LONG_OPENPGP_KEY_ID}"
git config --local commit.gpgsign 1
git config --local push.gpgsign 1

# prevent implicit merges on 'git pull'
git config --local pull.ff only
```

### 将转换后的 CVS 历史记录移植到克隆中

要将转换后的 CVS 历史记录包含在 git 存储库中，可以将其移植到存储库中：

```bash
git remote add history https://anongit.gentoo.org/git/repo/gentoo/historical.git/
git fetch history
git replace --graft 56bd759df1d0c750a065b8c845e93d5dfa6b549d history/master
```

完成此操作后，诸如`git log`之类的 git 命令将在初始 git commit 之后包括历史提交。

## 提交 gentoo.git

### 提交和验证提交

建议提交到 Gentoo 存储库的方法是使用`repoman commit`。它会自动对提交的软件包执行必要的质量检查，并具有其他功能来帮助 Gentoo 工作流程。但是，当前仅限于对单个软件包创建单个提交。

对于任何其他用例，都需要使用`git commit`和其他 git 命令。 git 的有效用法包括：

- 创建跨越多个软件包和/或 Gentoo 存储库的多个区域（eclass，许可证，配置文件…）的提交，
- 使用其他文件或修正程序修改通过`repoman commit`创建的提交，
- 结合使用`git rebase`通过`repoman commit`创建的多个提交。

只要不使用 repoman 提交，就需要使用`repoman full`手动验证受提交影响的所有软件包。由于 repoman 无法识别已进行的更改，因此请确保所有文件都包含在提交中。另外，当不使用 repoman 时，使用 git commit 命令必须添加`-s`或`--signoff`选项对[原证书](https://www.gentoo.org/glep/glep-0076.html#certificate-of-origin)进行手动签名。确保您已阅读并理解实际的证书。

### 原子提交

尽可能使用原子提交。尝试将更改分成逻辑提交，并尽可能遵循以下三个规则：

1. 不要在一次提交中包含多个无关的更改。但是，请确保不要不必要地拆分相关更改。例如，如果版本变更要求在 ebuild 中进行更改，则在一次提交中执行更改是正确的。但是，如果要修复先前版本中存在的其他错误，则此修复属于单独的提交。
2. 拆分在逻辑单元边界提交。更新多个软件包时，最好对每个软件包使用一次提交。避免在一次提交中将对 ebuild，eclass，许可证，配置文件等的更改组合在一起。但是，请勿在单个软件包中拆分相关或相互依赖的更改。
3. 避免创建会造成暂时性破坏的提交。除非不可能，否则请按依赖性安装顺序添加软件包。在需要许可证的软件包之前添加许可证。在 ebuild 依赖它们之前，提交`package.mask`和其他配置文件更改。通常，也可以将那些更改与添加包的提交一起包括在内。

请注意，修订改变被视为需要它们的更改的副作用，并且不属于单独的提交。当进行多个不需要修订的不相关更改时，只需在一次推送引入的系列中的第一次提交中更改修订。

## Git 提交消息格式

重要的是，正确格式化提交消息，以便它们以清晰简洁的方式将更改传达给读者。此外，消息格式的一致性使外部工具更易于解析。提交消息的第一行应包含所做更改的简短摘要，然后是空白的新行。从第三行开始，应该详细，多行地解释提交所引起的更改。最后，应使用 RFC822/git 样式标签列出辅助信息，例如与提交，贡献者和审阅者相关的错误。当与 commit 动作一起应用时，签署协议也将添加到此处。消息中的行长度最多应为 70-75 个字符。

摘要行应首先引用受更改影响的内容，后跟冒号“:”字符。使用以下列表中的规则，根据更改内容确定适当的格式，并适当替换软件包，类别和 eclass 名称：

- `${CATEGORY}/${PN}:`单个包（请注意，`repoman commit`会自动为你插入）
- `${CATEGORY}:` 包类别
- `profiles:` 个人设定目录
- `${ECLASS}.eclass:` Eclass 目录
- `licenses:` 许可证目录
- `metadata:` 元数据目录

对于`$ {CATEGORY}/$ {PN}:`很长的软件包，如果绝对必要，可以超出行长度限制，以确保更有用的摘要行。如果一次提交影响多个目录，请在该消息前添加一个最能反映更改意图的消息。如果与提交相关的 Gentoo Bugzilla 上有任何错误，可以使用`#nnnnnn`格式将错误的 ID 附加到摘要行中。如果要修改关键字，请明确说明要添加/删除的关键字。

<div class="alert alert-warning">
<b>警告</b>：默认情况下，以<code><pre>#</pre></code>开头的行被git视为注释，而不包含在提交消息中。确保新行不以<code><pre>#nnnnnn</pre></code>开头。可选地，可以通过更改<code><pre>commentchar</pre></code>选项将git配置为对注释使用其他字符。
</div>

对于特殊的提交，该消息应包含有关提交打算更改的内容，为何要求进行更改以及如何完成的详细说明，以及任何其他补充信息。

最后，提交消息应列出辅助信息，例如参与创作，建议，检查和测试更改的人员以及相关的错误。如 [Linux Kernel 修补程序指南](https://kernel.org/doc/html/latest/process/submitting-patches.html)中所述，使用 RFC822/git 样式标签 。此外，在 Gentoo 中可以选择使用以下标签：

- `Bug:`使用此引用错误而不关闭它们。该值是错误的 URL。如果需要引用多个错误，则应在单独的`Bug`标记中列出每个错误。如果引用了 Gentoo Bugzilla 上的错误，或者是 GitHub 上的问题或请求请求，则将自动添加对提交的引用。
- `Closes:`使用它来引用错误并自动关闭它们。与`Bug:`类似，该值是 Bug 的单个 URL，并且可以使用多个标签来关闭多个 Bug。如果引用了 Gentoo Bugzilla 上的错误，或者引用了 Gentoo 镜像的 GitHub 存储库上的问题或拉取请求，则会根据提交自动关闭（固定）。
- `Package-Manager:`这是由`repoman commit`自动插入的，它指定系统上`sys-apps/portage`的版本。
- `RepoMan-Options:`它由`repoman commit`自动插入，并记录传递给 repoman 的提交选项（例如--force）。

提交[用户贡献](./../ebuild-writing/user-submitted-ebuilds.md)时，请确保将其全名和电子邮件地址记入您的提交消息中。请注意并尊重其隐私：某些用户更喜欢仅以昵称来知道。在将此类信息输入到提交消息时，请利用诸如`Suggested-By`或`Reported-By:`之类的标签。

下面显示了示例提交消息：

```bash
app-misc/foo: version bump to 0.5

This also adds a new USE flag 'bar' which controls the
new bar functionality introduced with this version.

Bug: https://bugs.gentoo.org/00000
Closes: https://bugs.gentoo.org/00001
Signed-off-by: Alice Bar <a.bar@example.org>
```
