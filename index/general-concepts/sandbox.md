# 沙盒

在`src_unpack`，`src_compile`，`src_test`和`src_install` 阶段，`ebuild.sh`在沙盒中运行。这是一个特殊的环境，它尝试帮助防止写得不好的 ebuild（或帮助 ebuild 与写得不好的构建系统一起工作）在正确的位置执行。

**当沙盒处于活动状态时，所有软件包都必须正确构建。**软件包一定不能通过使用狡猾的技巧使沙盒屏蔽警告而达到目的 —— 沙盒是为了确保二进制软件包能够正确运行，并且写得不好`Makefile`不会引起问题。使用`addwrite`通常**不是**正确的解决方案。

沙盒相关的函数的详细信息，请参见[沙盒函数参考](./../function-reference/sandbox-functions-reference.md)。解决与沙盒相关的构建问题，请参见[处理访问冲突](./../appendices/common-problems.md)。
