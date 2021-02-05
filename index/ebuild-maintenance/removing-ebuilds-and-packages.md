# 删除 Ebuild 和软件包

## 删除 ebuild

删除 ebuild 时，请确保不会因为删除而破坏了 Portage 中的任何依赖关系 —— 此外，您的 git commit 消息应清楚地说明为什么从 git 存储库中删除 ebuild 的原因。

如果需要删除 ebuild，请确保不要意外删除任何体系结构的最新/唯一稳定的 ebuild。如果您想获得标记为稳定的较新版本，请提交错误或在 IRC 上询问。

删除 ebuild 时，也不要对任何`~arch`造成不必要的降级-相反，最好先获取标记为`~arch`的最新版本，然后再删除 ebuild 的冗余版本。

## 删除软件包

删除软件包时，请按照下列步骤操作：

1. 确保没有由于删除而破坏了 Gentoo 存储库中的依赖项
2. 将最后一个仪式发送到 gentoo-dev-announce 和 gentoo-dev
3. 遮罩包装
4. 等待 30 天（或更长时间）
5. 从 git tree 中删除，除非已解决删除原因
6. 从其他 ebuild 中删除对该包的任何引用，例如使用条件依赖项。阻止程序是唯一的例外。
7. 删除 package.mask 和所有 package.use.mask 条目
8. 在其他软件包的[metadata.xml](https://devmanual.gentoo.org/ebuild-writing/misc-files/metadata/index.html)文件中删除引用此软件包的`<pkg>`标记。
9. 以 WONTFIX 形式关闭打开的错误

这是将从 tree 中删除`dev-util/pmk`的命令列表：

```bash
＃cd dev-qt
＃git rm -rf qtphonon
＃git commit --signoff --gpg-sign
```

下面显示了示例提交消息：

```bash
commit b97eb6d43f45dfd5b739638928db22d3f3392685
Author: Michael Palimaka <kensington@gentoo.org>
Date:   Tue Oct 3 21:43:03 2017 +1100

  dev-qt/qtphonon: remove last rited package

  Closes: https://bugs.gentoo.org/629144
```
