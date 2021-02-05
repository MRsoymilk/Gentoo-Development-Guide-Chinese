# Overlay

通过使用 Overlay，Portage 可以在多个位置查找软件包，overlay 的位置由 `location` 一个或多个 repos.conf 文件中的变量控制。

overlay 应包含与 `PORTDIR`相同的目录结构（尽管仅需要包含必要的目录）。例如，一个简单的 overlay 可能具有如下目录结构：

```bash
overlay
|-- dev-util
    \`-- gengetopt
        |-- Manifest
        |-- files
        |   \`-- gengetopt-2.13-foobar.patch
        \`-- gengetopt-2.13.ebuild
```

overlay 可用于将项目“添加”到 tree 中（尽管你必须确保在添加任何新类别的情况下使用`/etc/portage/categories` ）或覆盖现有条目。

## Overlay 和 Eclass

在 overlay 中使用 eclass 时要非常小心。当更改一个 overlay eclass 时，Portage 不会更新缓存，也不会在 overlay ebuild 使用的主要 Gentoo 存储库 eclass 更改时更新缓存。在 overlay 中使用 eclass 时，你还可能还会遇到假的“非法继承”通知（参阅 [质量检查通知：ECLASS foo 被非法继承](./../appendices/common-problems.md)）。为了安全起见，在更新 overlay eclass 之后，手动`touch` 所有相关 overlay 文件。
