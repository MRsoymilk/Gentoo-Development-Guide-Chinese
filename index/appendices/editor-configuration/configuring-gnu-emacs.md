# 配置 GNU Emacs

## UTF-8 支持

GNU Emacs 具有内置的 Unicode 支持，从版本 23 开始，它使用 UTF-8 作为其内部编码。建议从 UTF-8 语言环境中启动 Emacs，它将控制默认的编码系统。

Emacs 通常会自动检测给定文本的编码系统。在极少数情况下，在极少数情况下，可能需要通过在`C-x C-f`或 `C-x C-v` 命令前加上 `C-x C-m c utf-8 RET`来告知 Emacs 正在打开 UTF-8 额外那件。作为诊断措施，可以使用`C-h C RET`确定当前使用的编码系统 。

如果希望在非 UTF-8 语言环境中使用 UTF-8 而不是常规字符集，则可以在 Emacs 启动文件中使用以下命令：

```bash
    (prefer-coding-system 'utf-8)
```

## 配置技巧和窍门

文件必须以换行符结尾，以使 diff 之类的工具 正常运行。为避免意外删除，在启动文件中设置`(setq require-final-newline 'ask)` 将自动检查该文件是否存在否则将要求你添加一个。请注意，许多编程语言会在保存文件之前自动添加换行符。

其他有用的设置可以（通过`(setq make-backup-files nil)`）禁用备份文件，因此你不会搞乱 git 仓库目录，也不会使 repoman 与之混淆（例如，将不必要的条目添加到 Manifest 文件中）。Emacs 甚至可以使用 X 服务器剪贴板函数与外界联系，该函数由`(setq x-select-enable-clipboard t)`激活。

## Gentoo 特定添加

为了易于编辑 ebuild，已创建了 Emacs 模式，该模式可在软件包`app-emacs/ebuild-mode`中找到 。它支持 ebuild 和 eclass、高亮关键字，还为个人定制提供了 hook。

软件包`app-emacs/nxml-gentoo-schemas` 改善了 Gentoo 特定 XML 文件（例如 metadata.xml）的编辑。通过使用 RELAX NG 模式为不同类型文档提供自动补全和即时验证。

## 进一步阅读

- https://www.gnu.org/software/emacs/manual/html_node/emacs/Recognize-Coding.html
- https://www.gnu.org/software/emacs/manual/html_node/emacs/Specify-Coding.html
- https://www.emacswiki.org/emacs/UnicodeEncoding
- https://www.emacswiki.org/emacs/ChangingEncodings
