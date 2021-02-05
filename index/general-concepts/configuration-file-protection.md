# 配置文件保护

Portage 包含一个用于保护配置文件的系统，这意味着 ebuild 不必担心会意外破坏`/etc` 中的文件。这就是所谓的“保护”，它由 `CONFIG_PROTECT` 和 `CONFIG_PROTECT_MASK` 变量控制。

将 image 从 `DESTDIR` 复制到 `ROOT` 时，Portage 会自动“保护” `CONFIG_PROTECT`（及其任何子目录）中列出的任何目录，但 `CONFIG_PROTECT_MASK`（及其子目录）中列出的目录除外。 Portage 不会直接安装受保护的文件，而是将其安装为`._cfg0000_filename`。然后，可以由用户自行决定由 `etc-update` 或 `dispatch-conf` 文件处理这些文件。

软件包**不得**尝试通过 `pkg_postinst` 或类似方法覆盖此系统。如果你需要以特定方式重命名、删除或更改文件，则应显示一条消息通知用户。
