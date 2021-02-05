# 构建函数参考

以下函数由 `ebuild.sh`提供，在解压缩和编译阶段十分有用。

| **函数**          | **细节**                                                                                                   |
| :---------------- | :--------------------------------------------------------------------------------------------------------- |
| `unpack archives` | 解压缩指定压缩文件。                                                                                       |
| `econf args`      | `./configure`包装器。传递所有 `args`。 `configure` 失败将中止（通过 `die`）。详细信息参见 [econf 选项](./src_config/../../ebuild-writing/ebuild-functions/src_configure/configuring-a-package.md)。 |
| `emake args`      | `make`包装器。传递所有 `args`。                                                                            |
