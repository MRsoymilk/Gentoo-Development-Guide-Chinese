# 用户和组

创建用户和组受[GLEP 81](https://gentoo.org/glep/glep-0081.html)约束。通过 `acct-user` 和 `acct-groupeclasss` 实现。新的用户和组在分别包创建 `acct-user` 和 `acct-group` 类别。

首先，检查 [UID / GID 分配列表](https://api.gentoo.org/uid-gid.txt)，并在 101 到 499 之间选择一个空闲的 UID / GID。如果要添加一个用户和一个具有相同名称的组，请分别为其 UID 和 GID 使用相同的编号。如有疑问，请从 499 下取下一个免费号码。

将你的新用户和组添加到 `uid-gid.txt` 位于 [data / api.git](https://gitweb.gentoo.org/data/api.git) 存储库中的文件中， 并在添加实际包之前将其推送。这算作保留标识符，将防止冲突。之后，你可以推送新的 ebuild。

`user.eclass` 现在不建议使用直接使用的历史记录方式，并且不得将其用于新软件包。

## ebuild 组

ebuild 组放置在`acct-group`类别中，软件包名称与组名称匹配。 `acct-group / suricata`的以下 ebuild 可用作编写组 ebuild 的模板：

```bash
# Copyright 2019-2020 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2

EAPI=7

inherit acct-group

DESCRIPTION="Group for Suricata IDS"
ACCT_GROUP_ID=477
```

ACCT_GROUP_ID 必须设置为请求的 GID。

## ebuild 用户

ebuild 用户 放在 `acct-user` 类别中，包名称与用户名匹配。以下`acct-user / suricata`的 ebuild 可用作编写用户 ebuild 的模板：

```bash
# Copyright 2019-2020 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2

EAPI=7

inherit acct-user

DESCRIPTION="User for Suricata IDS"
ACCT_USER_ID=477
ACCT_USER_GROUPS=( ${PN} )

acct-user_add_deps
```

`ACCT_USER_ID` 必须设置为请求的 GID。 `ACCT_USER_GROUPS` 应该列出用户所属的所有组，首先是主要组。所有其他关键字都是可选的。

`ACCT_USER_SHELL` 可用于为用户设置 shell。如果未设置，则使用等同于未登录的最佳系统。 `ACCT_USER_HOME` 指定主目录，`/dev/null`使用默认目录。 `ACCT_USER_HOME_OWNER` 可用于覆盖主目录的所有权，并 `ACCT_USER_HOME_PERMS` 覆盖默认权限。默认值是所有者的用户和主要组，权限是 0755。

<div>
<b>要点</b>： 只要有可能，都应使用默认的 Shell 和主目录。其理由在下面说明。
</div>

你应该开始进行 GLEP 81 迁移，例如 EAPI 更新。与其将用户的设置简单地复制到 `acct-user`软件包中，不如借此机会重新评估用户的名称，shell，主目录及其权限。我们的 GLEP 81 实施方案将揭示许多过去被淘汰的用户管理问题。

## 选择 shell

在大多数情况下，应使用默认 shell 程序（即无 shell 程序）。仍然能以无 shell 程序的用户身份启动服务，并且守护程序可以将特权授予无 shell 程序的用户。如有必要，管理员可以使用覆盖用户的默认`shell su -s <shell> <username>`。这对于测试，管理 SSH 凭据以及在 ebuild `pkg_config` 阶段进行初始配置就足够了。

该规则的一个明显例外是，如果用户需要交互地登录该帐户，就像 `root` 用户一样。当然也存在其他例外情况，但应逐案评估。换句话说，如果你尚未检查，请不要将用户的 shell 程序设置为，`/bin/bash` 因为你认为他*可能*需要它。

这里的目标是双重的。首先，最小特权原则说，如果用户不需要真正的 shell，那么他就不应该拥有 shell。同样，无 shell 会让系统管理员安心：他不必担心该用户是否拥有密码（以及密码的强度），或者是否具有密码。文件系统权限全部设置正确，或者是否可以通过 SSH 等登录。

## 选择主目录

在大多数情况下，应使用默认的主目录（即没有主目录）。GLEP 81 更改了有关主目录的用户管理的两个方面：

1. 创建用户可以修改现有目录上的权限。如果需要，对于新版本的 `acct-user` 包来说，必须能够修复其主目录的所有权和权限。
2. 用户名以外的所有用户数据对于依赖于该用户的 ebuild 来说都是非本地的。这仅仅是将用户创建从客户端软件包移到单独 `acct-user`软件包中的副作用 。

第一项表示选择主目录时应保持保守。如果有可能，请避免选择其他软件包使用的主目录。特别是，没有两个 `acct-user`软件包应使用相同的主目录。最好，共享主目录上的所有权和权限需要在共享它的所有软件包之间保持同步。最坏的情况是，一个软件包不同步，并为其他不再具有预期权限的软件包引入了安全漏洞。

第二项表示如果软件包需要用户，则无法再确定该用户的主目录或其所有权和权限。如果你的软件包要求某个目录必须由某个用户拥有和可写，则该软件包的 ebuild 应该创建该目录并确保该目录可被用户写入。换句话说，即使该依赖项是一个 `acct-user`软件包，也不应依赖该依赖项“以传递方式”创建的目录 。

综上所述，

- 避免使用 `ACCT_USER_HOME` 属于另一个软件包的。
- 没有两个 acct 用户软件包应定义相同的 `ACCT_USER_HOME`。
- 例如，如果软件包的配置需要`<username>`才能写入`/var/lib/<username>`，则软件包的 ebuild 应该创建该目录并设置其所有权和权限。除其他注意事项外，相应的 `acct-user`软件包应保留 `ACCT_USER_HOME` 其默认值（空）；设置 `ACCT_USER_HOME=/var/lib/<username>`会产生不必要的重复，并可能会导致权限失去同步。

## 选择主目录所有权

在大多数情况下，默认的主目录所有权是正确的。如果根本需要一个非默认的主目录，则该目录应可由其用户写入，并且将其所有权授予其他人可以防止该情况发生。不可写表示已选择一个共享且可能敏感的位置。而且，主目录不可写的事实表明，默认的主目录（也是不可写的）就足够了。主目录指南说明了在这种情况下为什么最好使用默认值。例如，设置`ACCT_USER_HOME_OWNER="root:root"`是可疑的，因为它似乎是要“撤消”用户软件包更改的所有权，并且只有在其他软件包使用了相关路径时才需要设置。

## 选择主目录权限

在许多情况下，默认的主目录权限（0755）就足够了。但是，如果你的软件包将在 0700 或 0750 模式下工作，则最好使用这些模式。这又是最小特权的原则。如果你的软件包使用不可写的主目录，则你可能应该使用默认的无主目录！

<div class="alert alert-warning">
<b>警告</b>： 绝对不可在<code><pre>ACCT_USER_HOME_PERMS</pre></code>中设置世界可写位。这永远是没有必要的，并且通常是可利用的。
</div>

## 结语

这些建议不是规则，也不是一成不变的，除非可能是世界可写的警告。tree 中有`chroot()`到`$HOME`的软件包，出于安全原因，要求该目录归`root:root`拥有。在这种情况下，无法避免通过主目录将用户隐式绑定到需要它的包，而您能做的最好的就是尝试确保用户和路径是唯一的，以免发生冲突。

但是，除非您的软件包是特殊的，否则遵循这些准则将最大程度地减少将来出现问题的可能性。

## 利用软件包中的用户和组

为了使你的软件包安装特定的用户和组，请将它们指定为依赖项。构建时所需的帐户必须包含在`DEPEND`中 ，运行时所需的帐户必须包含在`RDEPEND`中。

例如，`foo` 在运行时需要用户和组的 ebuild 将指定：

```bash
RDEPEND="
    acct-user/foo
    acct-group/foo"
```

如果在`pkg_preinst`中设置了已安装文件的所有权，这也就足够了。但是，如果 ebuild 需要在构建时已经存在用户和组，它将指定：

```bash
RDEPEND="
    acct-user/foo
    acct-group/foo"
DEPEND="${RDEPEND}"
```
