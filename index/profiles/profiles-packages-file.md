# 个人文件 `packages` 文件

`profiles/`中的`packages`文件控制`@system`集合引入的软件包。 `base/packages`未经`gentoo-dev`列表讨论，不得修改。子配置文件覆盖取决于相关的架构团队。

请注意，`packages`可用于要求使用特定版本的软件包，例如，使用`<sys-kernel/linux-headers-2.5` 以配置 2.4 的内核。如果这样做**仅**是为了要求较低版本（也就是说，不是>或>=限制，并且 slot 不影响结果），则`packages`必须在其中输入相应的 `package.mask`条目，以确保没有安装其他版本。
