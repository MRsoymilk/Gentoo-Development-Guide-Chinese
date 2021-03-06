# 软件包维护者

软件包维护者负责 ebuild 维护任务——将 ebuild 升级到新的上游发行版，根据策略进行必要的更新、解决错误，等等。软件包的维护者列在软件包的 [metadata.xml](。/../../ebuild-writing/miscellaneous-file/package-and-category-metadata.xml.md) 文件中。分配错误时，除非在元数据中另有说明，否则列出的第一个维护者将成为错误的受让人，其余维护者将添加到 CC 中。

## 维护者权限

软件包维护者对其维护的软件包具有权限。除非另有说明，否则在提交他们的软件包之前，你应该请求维护者的批准。尝试提交一个错误，在 IRC 上告诉他们，或者先发送邮件。该规则的例外是更改微小，例如，由于软件包移动而导致的依赖关系更新。

如果维护者不再活跃，最好咨询其他开发人员如何处理该情况，尤其是在这是需要尽快处理的关键问题时。在这种情况下，可以使用常规的维护者超时工具。如果软件包的维护者在附加或链接到正确分配的错误后的 2-4 周内都未答复该修补程序，则可以使用超时工具。

<div class="alert alert-important">
<b>重要提示</b>： 常识告诉我们，工具链/基础系统/核心软件包和 eclass 或其他任何精致的（例如 glibc）或广泛使用的（例如 GTK +）的东西通常应该留给其维护者。如有疑问，请勿修改。
</div>

尊重开发人员的编码偏好。不必要地更改 ebuild 的语法只会给他人带来麻烦。仅当有实际好处时才应进行语法更改，例如更快的编译，改善了最终用户的信息，或者符合 Gentoo 政策。

## 添加和删除维护者

新添加到 Gentoo 存储库的所有软件包都必须至少指定一个维护者。新的维护者只有在获得他们的同意的情况下才能被添加。特别是，未经其成员批准或违反其明确政策，为通用项目（例如 Python 项目）添加为软件包维护者是不可接受的。

没有提交访问权限的维护者称为代理维护者。他们的更改必须由 Gentoo 开发人员提交，充当他们的代理。由代理维护人员维护的所有软件包必须在`metadata.xml`中明确列出其代理开发人员或项目。

添加新的维护者或删除旧的维护者时，请记住要重新分配错误修复。最后一个放弃维护软件包的维护者必须在`metadata.xml`中使用`<!-- maintainer-needed -->`注释以简化查找未维护软件包的过程。还需要向`gentoo-dev-announce`和`gentoo-dev`邮件列表发送“抢购”邮件，以提供新的未维护软件包的列表。
