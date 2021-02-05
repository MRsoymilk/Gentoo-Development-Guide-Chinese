# 添加一个新的 ebuild

## 在 Gentoo 存储库中(不应)放入什么

在编写新的 ebuild 之前，请检查[bugs.gentoo.org](https://bugs.gentoo.org/)以查看是否已经为该软件包编写了 ebuild，但尚未将其添加到 Gentoo 存储库中。转到[bugs.gentoo.org](https://bugs.gentoo.org/)，选择查询，然后选择高级搜索；选择产品*Gentoo Linux*作为产品，选择*ebuilds*作为组件。在搜索字段中，输入 ebuild 的名称，并在状态栏中选择所有可能的字段，然后提交查询。对于懒惰的人，请单击[此处](https://bugs.gentoo.org/query.cgi?format=advanced&product=Gentoo%20Linux&component=New%20Ebuilds&bug_Status=UNCONFIRMED&bug_status=CONFIRMED&bug_status=IN_PROGRESS&bug_status=RESOLVED&bug_status=VERIFIED)。

Gentoo 存储库仅应用于存储`.ebuild`文件以及任何小的随行文件，例如补丁程序或样本配置文件。这些文件的大小最多为 20 KiB，并且必须放置在`mycat/mypkg/files`目录中，在 ebuild 中可以将其称为`${FILESDIR}`。较大的修补程序文件应放在`dev.gentoo.org`上的开发人员空间中，以最小化存储库的大小。

存储库中的文件应为未压缩的纯文本文件，即没有二进制文件。这将允许版本控制系统合并更改并正确通知开发人员冲突。

请记住，当您提交的软件包稳定时，必须为开箱即用的终端用户准备好它们。确保您有一套不错的默认设置，可以满足使用软件包的大多数系统和用户的需求。如果您的软件包损坏了，并且您不确定如何使它工作，请检查其他一些已经完成了各自版本的发行版。您可以查看[Debian](https://www.debian.org/distrib/packages)或[Fedora](https://src.fedoraproject.org/projects/rpms/*)中的一些示例。

提交 git 时，所有开发人员都应使用`repoman commit`而不是`git commit`提交其 ebuild。在提交之前，请运行`repoman full`以确保您没有忘记什么。

## 初始架构关键字

添加新的 ebuild 时，应仅对实际测试过 ebuild 的体系结构包含`KEYWORDS`，以确认它可以正常工作并且在将要安装的结果包中正确使用了`USE`标志。如果可能，您还应该对实际的库或应用程序进行全面的测试，因为您将对体系结构的任何损坏负责。应始终执行最小限度的测试，例如检查应用程序是否启动且没有任何错误。

如果要添加用户提交的 ebuild，请不要假定提交者已经在各种体系结构上进行了测试：`KEYWORDS`通常是跨软件包克隆的，或者是从源软件包中的文档生成的，这并不表示该软件包确实可以在这些架构。

## 文件目录

如前所述，每个包子目录下都有一个`files/`目录。您的软件包可能需要的所有补丁程序，配置文件或其他辅助文件都应添加到此目录中；任何大于或等于 20KB 的文件都应转到镜像文件中，以减少用户必须下载的（不需要的）文件量。您可能要考虑命名自己创建的补丁程序，只是为了使您的软件包以特定于版本的名称构建，例如`mypkg-1.0-gentoo.diff`，或更简单的是`1.0-gentoo.diff`。还要注意，`gentoo`扩展告知人们该补丁是由我们（Gentoo Linux 开发人员）创建的，而不是从邮件列表或其他地方获取的。同样，您不应该压缩这些补丁。

请考虑在您放置到`files/`目录中的每个文件加上前缀或后缀（例如`mypkg-1.0`），以便使 ebuild 上每个单独版本使用的文件彼此区分开，并且可以看到不同修订版之间的更改。通常这是一个非常好的主意:)。如果希望通过补丁名称传达更多含义，则可能需要使用其他后缀。

如果您有许多文件应进入`files/`目录，请考虑创建子目录（例如`files/1.0/`）并将相 ​​ 关文件放在适当的子目录中。如果使用此方法，则无需在文件名中添加版本信息，这通常更方便。
