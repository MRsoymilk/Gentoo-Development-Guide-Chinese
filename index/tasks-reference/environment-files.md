# 环境文件

一些软件包需要为所有用户全局设置一个环境变量。做到这一点的规范方法是通过`/etc/env.d/`。生成用户环境设置时，此目录中的所有文件都将被获取。

此目录应当**仅**应用于设置环境变量。

要将文件安装到此目录中，请使用`doenvd`或`newenvd`（请参阅 [安装函数参考](./../function-reference/install-functions-reference.md)）。文件的格式应为一系列行，格式为`VARIABLE="the value"`。

在[环境变量部分](https://wiki.gentoo.org/wiki/Handbook:X86/Working/EnvVar)中有进一步的讨论。
