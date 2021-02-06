# 特征

`FEATURES` 变量指定影响 Portage 操作方式和软件包编译方式的选项。它**不用于**对生成的软件包产生重大影响的设置。

与开发人员相关的`FEATURES`：

| **特征**            | **说明**                                                            |
| :------------------ | :------------------------------------------------------------------ |
| `collision-protect` | 如果安装软件包试图覆盖其他软件包提供的文件，则会引发错误。          |
| `noauto`            | 使用 `ebuild`时，仅运行要求的函数。                                 |
| `sandbox`           | 启用 沙盒。                                                         |
| `sign`              | GPG 签名 `Manifest` 文件。                                          |
| `strict`            | 对潜在的危险情况（例如丢失的 `Manifest` 文件）进行一些额外的检查 。 |
| `test`              | 启用 `src_test` 阶段。                                              |
| `userpriv`          | 在某些阶段，请使用非 root 用户权限。                                |
| `usersandbox`       | 即使在非特权运行时也启用 沙盒。                                     |