# Manifest

## 生成 Manifest

在 tree 中，每个包都有一个 `Manifest` 文件。该文件与软件包的 ebuilds 位于同一目录中。该 `Manifest` 文件包含摘要（当前目录中可以以`manifest-hashes`形式在 `metadata/layout.conf`找到 ）和文件大小的数据包所使用的每个 distfile。这用于在获取它们时验证完整性。

要生成 `Manifest`，请使用 `ebuild foo.ebuild manifest` 或 `repoman manifest`。您可能需要设置`GENTOO_MIRRORS=`，同时调用它以立即从其原始位置获取发布文件。

## 薄和厚的 Manifest

Gentoo 中有两种 Manifest 文件：用于开发存储库的瘦 Manifest，以及通过 同步分发给最终用户的瘦 Manifest。上面介绍了薄清单。

厚 Manifest 将为存储库中的所有文件添加校验和，并添加一个 OpenPGP 签名。当通过不安全的通道传输存储库时，这将提供完整性和真实性检查。厚 Manifest 是在 Gentoo 基础架构上自动生成的，不需要开发人员采取任何具体措施。
