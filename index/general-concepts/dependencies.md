# 依赖关系

自动依赖项解析是`emerge`所提供的最有用的函数之一 。

## 依赖类型

### CHOST vs CBUILD

为了避免歧义，我们在交叉编译时使用以下术语来表示不同的系统：

**CBUILD**

> 在其上执行构建的系统。可以在构建期间执行适用于 CBUILD 系统的依赖项。交叉编译时，它们不会安装到正在构建的系统中。

**CHOST**

> 将在其上执行软件包的系统。交叉编译时，无法执行应用于 CHOST 的依赖项。

如果不进行交叉编译，CBUILD 和 CHOST 的值相同，并且两种依赖关系都将被合并。

### 构建依赖关系

构建依赖项用于指定解压缩，修补，编译，测试或安装软件包所需的任何依赖项（例外请参阅 [隐式系统依赖项](./dependencies.md)）。

从 EAPI 7 开始，构建依赖项分为两个变量： `BDEPEND` 和 `DEPEND`。`BDEPEND` 指定适用于 CBUILD 的依赖项，即在构建过程中需要执行的程序，例如 `virtual/pkgconfig`。`DEPEND` 指定 CHOST 的依赖关系，即需要在内置系统上找到的软件包，例如库和标头。

在早期的 EAPI 中，所有构建依赖项都放在 DEPEND 中。

### 运行时依赖

`RDEPEND` ebuild 变量应指定运行时所需的任何依赖项。这包括库（在动态链接时），任何数据包和（对于解释语言）相关的解释器。

请注意，从二进制软件包安装时，将仅检查 `RDEPEND`。因此，即使项目列在 `DEPEND` 中，也必须包括这些项目。

_理论上_，`RDEPEND` 中而不是 `DEPEND` 中的项目可以在目标软件包*之后*合并。 Portage 当前不执行此操作。

### Post 依赖

`PDEPEND`变量指定了严格不要求立即满足的运行时依赖项。它们可以在打包*后*合并。该变量仅用于解决循环依赖性，而在一般情况下，应改用`RDEPEND`。

## 依赖语法

### 基本依赖语法

基本 `DEPEND` 规范可能如下所示：

```bash
DEPEND="dev-lang/ruby
	dev-ruby/ruby-gtk2
	dev-ruby/mysql-ruby"
```

每个*软件包相关性规范*都是软件包的完整类别和名称。依赖关系规范由任意空格分隔——出于可读性考虑，约定每行有一个规范。指定名称时，类别部分应视为必填项。

## 版本依赖

有时需要特定版本的软件包。如果知道这一点，则应指定。一个简单的例子：

```bash
DEPEND=">=dev-libs/openssl-0.9.7d"
```

这说明 `openssl` 至少需要 0.9.7d 版本。

### 版本说明符

可用的版本说明符是：

| **说明符**            | **含义**                                                     |
| :-------------------- | :----------------------------------------------------------- |
| `>=app-misc/foo-1.23` | 需要版本 1.23 或更高版本。                                   |
| `>app-misc/foo-1.23`  | 需要严格高于 1.23 的版本。                                   |
| `~app-misc/foo-1.23`  | 需要版本 1.23（或任何 `1.23-r\*`版本）。                     |
| `=app-misc/foo-1.23`  | 确实需要版本 1.23。如果可能的话，请使用`~`形式简化修订版本。 |
| `<=app-misc/foo-1.23` | 需要版本 1.23 或更低版本。                                   |
| `<app-misc/foo-1.23`  | 需要严格低于 1.23 的版本。                                   |

### 范围依赖

要指定包的“版本 2.x（不是 1.x 或 3.x）”，必须使用星号后缀。在以下情况下最常见：

```bash
DEPEND="gtk? ( =x11-libs/gtk+-2* )"
```

请注意，等号是强制性的，星号前没有点。另请注意，在选择特定版本中的所有版本时 `SLOT`，`SLOT` 应使用依赖项（请参见下文）。

### 阻止程序

当不能同时安装两个软件包（软件包 slot，版本）时，可以使用阻止程序将此类冲突暴露给软件包管理器。

阻止程序有两种：弱阻止程序和强阻止程序。

使用以下语法定义了弱阻止程序：

```bash
RDEPEND="!app-misc/foo"
```

软件包管理器将尝试自动解决此冲突。被弱阻止程序阻止的软件包可以在 安装阻止程序后卸载。但是，它使普通文件免于文件冲突检查。弱阻止程序通常用于解决软件包之间的文件冲突，仅在中有用 `RDEPEND`。

更具体地说，安装较新的软件包可能会覆盖属于被明确阻止的较旧软件包的所有冲突文件。当发生此类文件冲突时，冲突文件将不再属于较旧的软件包，并且在最终卸载较旧的软件包后仍会保留它们。仅在将任何较新的阻止软件包合并到其上之后，才卸载较旧的软件包。

<div class="alert alert-warning">
<b>警告</b>：单纯的<code><pre>DEPEND</pre></code>弱阻止程序 无法正常工作。虽然Portage似乎将要删除的软件包排队，但它并没有免除文件冲突检查中的内容。始终将弱阻止程序纳入<code><pre>RDEPEND</pre></code>中！
</div>

如果在构建（安装）软件包之前完全有必要解决该阻止程序，则必须使用功能强大的阻止程序。在这种情况下，不允许临时同时安装有冲突的软件包。强阻止程序使用以下语法表示：

```bash
RDEPEND="!!app-misc/foo"
```

强阻止程序会相应地应用于定义它们的依赖项类型。只要安装了软件包，便会强制执行`RDEPEND`中定义的阻止程序（但不阻止生成二进制软件包）。仅在`DEPEND`中定义的阻止程序仅适用于从源代码构建软件包，一旦安装了软件包或从二进制软件包安装了该程序，便可能无法应用。

<div class="alert alert-note">
<b>注意</b>：如果弱阻止程序和强阻止程序都匹配给定的软件包，则强阻止程序优先。
</div>

特定版本也可以被阻止：

```bash
RDEPEND="!<app-misc/foo-1.3"
```

根据正常的依赖关系，基于`USE`标志，阻止程序可以是可选的。

添加到较早的 ebuild 中的阻止程序不应具有追溯力。如果用户已经安装了 ebuild，则不应期望对 ebuild 进行任何更改。这意味着您应该将阻止程序添加到最新的 ebuild 中（即使从逻辑上讲，它似乎是向后的）。例如，某些版本的 portage 不适用某些版本的 bash，但是将阻止程序放入 bash 中是因为这是导致问题的较新软件包。

## slot 依赖性

要依赖于特定的`slot`，请在包名称后附加`:SLOT`，其中“SLOT”是所需包的`SLOT`：

```bash
DEPEND="qt3? ( x11-libs/qt:3 )
	gtk? ( x11-libs/gtk+:2 )
```

要依赖 SLOT 中的特定版本或版本范围，我们使用：

```bash
DEPEND="qt3? ( ~x11-libs/qt-3.3.8:3 )
	gtk? ( >=x11-libs/gtk+-2.24.9:2 )
```

## slot 操作符

在 `EAPI=5`或更高版本中，你可以使用附加在软件包名称后面的 slot 操作符来声明在满足其运行时依赖性的版本更新为具有不同 slot 或子 slot 的版本之后，是否应该重建软件包：

- `:=` 任何 slot 都是可以接受的，并且如果与运行时依赖性最匹配的版本更新为具有不同 slot 或子 slot 的版本，则应重新构建你的软件包；
- `:\*` 任何 slot 都是可接受的，并明确声明该 slot 或子 slot 中的更改可以忽略；
- `:SLOT=` 只允许使用“SLOT”的 slot，并且如果将与运行时相关性匹配的版本更新为具有该 slot 但具有不同子 slot 的另一个版本，则应重新构建你的软件包；
- `:SLOT` 仅接受“SLOT”的 slot，并且可以忽略子 slot 中的更改（就像以前的 EAPI 中一样）。
- `:SLOT/SUBSLOT` 表示对特定 slot 和子 slot 对的依赖关系，这对于安装预编译二进制文件的软件包很有用，这些软件包需要具有与子 slot 相对应的特定名称版本的库。

例如：

```bash
RDEPEND="media-libs/cogl:1.0=
	gnutls? ( >=net-libs/gnutls-2.8:= )"
```

## USE 条件依赖性

仅当设置了给定`USE`标志时才依赖特定软件包：

```bash
DEPEND="perl? ( dev-lang/perl )
	ruby? ( >=dev-lang/ruby-1.8 )
	python? ( dev-lang/python )"
```

如果*未设置*给定的`USE`标志，则还可以依赖于某个软件包：

```bash
RDEPEND="!crypt? ( net-misc/netkit-rsh )"
```

这**不应**用于禁用给定体系结构上的某个`USE`标志。为此，架构团队应将`USE`标志添加到 Gentoo 存储库的`profiles/arch`目录中的`use.mask`文件中。

这可以嵌套：

```bash
DEPEND="!build? (
	gcj? (
		gtk? (
			x11-libs/libXt
			x11-libs/libX11
			x11-libs/libXtst
			x11-proto/xproto
			x11-proto/xextproto
			>=x11-libs/gtk+-2.2
			x11-libs/pango
		)
		>=media-libs/libart_lgpl-2.1
	)
	>=sys-libs/ncurses-5.2-r2
	nls? ( sys-devel/gettext )
)"
```

## 任何依赖项

依赖 `foo` 或 `bar`：

```bash
依赖 `foo` 或 `bar`：
```

如果设置了`baz` `USE`标志，则要依赖`foo`或`bar`：

```bash
DEPEND="baz? ( || ( app-misc/foo app-misc/bar ) )"
```

### USE 中的任何一个

可以针对`foo`或`bar`建立`fnord`。然后，仅当以下所有条件成立时，才需要`USE`标志：

- `fnord`在具有 `foo` 且未安装 `bar` 的系统上安装。然后将 `foo` 卸载，并安装 `bar`。 `fnord` 必须继续正常工作。
- 在带有 `foo` 而不是 `bar` 的系统上制作的 `fnord` 二进制软件包可以被获取并安装在带有 `bar` 而不是 `foo` 的系统上。

## 内置 USE 依赖关系

可用的说明符包括：

| **说明符**               | **含义**                      |
| :----------------------- | :---------------------------- |
| `app-misc/foo[bar]`      | foo 必须启用 bar。            |
| `app-misc/foo[bar,baz]`  | foo 必须同时启用 bar 和 baz。 |
| `app-misc/foo[-bar,baz]` | foo 必须禁用 bar 并启用 baz。 |

对于特定情况，还有一些快捷方式：

| **紧凑的形式**        | **等效展开形式**                                          |
| :-------------------- | :-------------------------------------------------------- |
| `app-misc/foo[bar?]`  | `bar? ( app-misc/foo[bar] ) !bar? ( app-misc/foo )`       |
| `app-misc/foo[!bar?]` | `bar? ( app-misc/foo ) !bar? ( app-misc/foo[-bar] )`      |
| `app-misc/foo[bar=]`  | `bar? ( app-misc/foo[bar] ) !bar? ( app-misc/foo[-bar] )` |
| `app-misc/foo[!bar=]` | `bar? ( app-misc/foo[-bar] ) !bar? ( app-misc/foo[bar] )` |

## 使用默认依赖项

如果依赖性在新的软件包版本中引入或删除了`USE`标志，则在目标软件包中不存在该标志的情况下，可以将`(+)`或`(-)`添加到 use-dependency 规范中以定义默认值。 `(+)`表示丢失标志被假定为启用，`(-)`相反。

例如，以下将把没有`threads`标志的所有`boost`版本都视为已启用，而没有`openmp`的所有`gcc`版本都将其视为已禁用：

```bash
DEPEND="
	>=dev-libs/boost-1.48[threads(+)]
	sys-devel/gcc[openmp(-)]"
```

## 检查依赖项的提示

确保包的所有依赖项都完整很重要：

**查看已安装的二进制文件/库**

> 使用诸如`scanelf -n`（来自 app-misc/pax-utils）或`objdump -p`（来自 sys-devel/binutils）之类的工具列出`DT_NEEDED`条目

**查看`configure.ac`**

> 在此处查找软件包的检查。需要注意的是 pkg-config 检查或检查特定版本的`AM_ *`函数。

**查看包含`.spec`的文件**

> 一个好的依赖关系指示是查看包含的`.spec`文件以获取相关的依赖。但是，不要相信它们是最终的依赖关系完整列表。

**查看应用/库网站**

> 检查应用程序网站以了解他们建议的可能依赖关系

**查看软件包的`README`和 `INSTALL`**

> 它们通常还包含有关构建和安装软件包的有用信息。

**记住非二进制依赖性，例如 pkg-config，doc 生成程序等。**

> 通常，构建过程需要一些依赖项，例如 intltool，libtool，pkg-config，doxygen，scrollkeeper，gtk-doc 等。请确保明确说明了这些依赖项。

## 隐式系统依赖

所有软件包都对整个`@system`集合具有隐式的编译时和运行时依赖性。因此，除非需要特定版本或软件包（例如，在`uclibc`上使用`glibc`），否则不必（也不建议）指定对工具链软件包（如`gcc`，`libc`等）的依赖关系。请注意，此规则还需要考虑诸如`flex`，`zlib`和`libtool`之类的软件包，它们并非在每个配置文件的`@system`集中。例如，嵌入式配置文件在`@system`中没有`zlib`，`libtool`应用二进制接口可能会更改并破坏构建顺序，将来`flex`可能会从`@system`集合中删除。

但是，`@system`集合中包含的软件包或`@system`集合软件包的依赖项通常应包括完整的依赖项列表（不包括引导软件包）。这让从第 1 阶段或第 2 阶段压缩包安装时使用`emerge -e @system`成为可能，。

## 测试依赖

软件包通常具有可选的依赖项，只有在运行测试时才需要这些依赖项。这些应在 USE 标志后的 DEPEND 中指定。通常，“test” USE 标志用于此目的。

由于在未安装测试依赖项时测试可能会失败，因此在这种情况下应禁用测试阶段。这可以通过 RESTRICT 变量中的 USE 条件来完成。

```bash
# Define some USE flags
IUSE="debug test"

# Require debug support when tests are enabled
REQUIRED_USE="test? ( debug )"

# Disable test phase when test USE flag is disabled
RESTRICT="!test? ( test )"

# Running tests requires 'foo' to be installed
DEPEND="test? ( dev-util/foo )"
```

## 循环依赖

如果包的一个或多个（可能是间接的）依赖关系依赖于包本身，则会发生循环依赖关系。这将创建一个依赖周期，其中每个软件包在技术上都必须先安装另一个软件包。例如，如果软件包 A 依赖于 B，程序 B 依赖于 C，而程序 C 依赖于 A，则软件包管理器不能在 C 之前安装 A，而 C 在 A 之前安装。

有三种循环依赖项：

1. 如果只有一个软件包必须先安装，然后再安装，则发生循环依赖。例如，`dev-python/certifi`严格要求使用`dev-python/setuptools`进行构建，而后者的软件包则需要前者才能提供的某些运行时功能。结果，`dev-python/certifi`可以比其他软件包晚安装。 `PDEPEND`用于表达此信息并自动解决循环依赖关系。
2. 如果循环仅适用于其中一个软件包的 USE 标志的某种组合，则会发生循环依赖关系。例如，在`dev-python/setuptools`中运行测试需要许多软件包，这些软件包需要首先安装`dev-python/setuptools`。用户可以通过调整其中一个软件包的 USE 标志来解决这种循环依赖关系，例如通过在`dev-python/setuptools`上禁用测试，并在最初安装依赖项后重新启用它们。
3. 无法使用常规方法解决的循环依赖关系。例如，`dev-util/cmake` 过去依赖 `dev-libs/jsoncpp`，而后者则使用前者来构建。解决这种依赖关系通常需要有条件地捆绑其中一个依赖关系，或者提供备用的引导路径。
