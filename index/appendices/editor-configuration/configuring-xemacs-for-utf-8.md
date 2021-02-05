# 为 UTF-8 配置 XEmacs

首先，你需要使用 `USE=mule`构建 `xemacs`本身。然后，安装软件包`app-xemacs/mule-ucs` 。若要自动加载 Unicode 支持，你可以将以下内容添加到你的`~/.xemacs/init.el`中：

```bash
    (require 'un-define)
```

如果文件上的自动字符集检测无法正常工作，以下三个命令可以提供帮助。

- `set-buffer-file-coding-system (C-x RET f)` 更改当前文件的编码系统。
- `set-default-buffer-file-coding-system (C-x RET F)` 为没有显式设置字符集编码的未保存缓冲区和 ASCII 外观文件设置默认值。
- `universal-coding-system-argument (C-x RET c)` 能让你为下一个命令设置编码系统，因此可以在错误出现之前，从新的窗口发送此命令后再打开问题文件，避免错误出现。

当前的编码系统将显示在左侧状态栏的附近。
