# 文件系统

文件系统的基本布局和用途如下：

- `/bin`：关键启动应用程序
- `/etc`：系统管理员控制的配置文件
- `/lib`：关键启动库
- `/opt`：非标准布局应用
- `/sbin`：系统管理员启动关键型应用程序
- `/tmp`：临时数据
- `/usr`：一般应用
  - `/usr/bin`：应用
  - `/usr/lib`：库
  - `/usr/local`：非便携式应用程序。**Ebuilds 不得在此处安装**。
  - `/usr/sbin`：非系统关键的系统管理员应用程序
  - `/usr/share`：与体系结构无关的应用程序数据和文档
- `/var`：程序生成的数据
  - `/var/cache`：可以重新生成的长期数据
  - `/var/lib`：通用应用程序生成的数据
  - `/var/log`：日志文件

在可能的情况下，我们尽量将对非启动关键程序放在`/usr`中，而不是放在`/`中。如果在启动过程中直到挂载文件系统之后才需要的程序，则该程序通常不属于`/`。

链接到`/usr`下的库的任何二进制文件本身都必须进入`/usr`（或者可能是`/opt`）。

`/opt`顶层应该只用于那些不符合标准的文件系统布局的应用。这尤其包括预装的软件包，这些软件包希望安装在单个目录中。

`/usr/local`层次结构适用于非移植软件。Ebuild 不得尝试在此处放置任何内容。

`/usr/share`目录用于与体系结构无关的应用程序数据，该数据在运行时不会修改。

尽量避免在`/etc`中安装不必要的东西——系统管理员需要为其中的每个文件进行额外的工作。特别是，非文本文件和不适合系统管理员使用的文件应移至`/usr/share`。

## FHS

Gentoo 不认为 [文件系统层次结构标准](http://www.pathname.com/fhs/)是权威性标准，尽管我们的许多策略都与之吻合。
