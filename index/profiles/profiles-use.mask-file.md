# 个人文件 `use.mask` 文件

`use.mask` 文件可用于将 `USE` 标志标记为在特定配置文件上不可用。由于多种原因，这可能很有用：

- 屏蔽特定于硬件的功能标志。例如，`mmx`和`sse`仅在 x86 上可用，`altivec`仅在`ppc`上可用，而`vis`仅在 sparc v9 上可用。
- 禁用不可用的软件依赖项。一个简单的假设示例——\_假设 `fooapp` 适用于 `mips`，但是对 `libbar` 具有可选的依赖项（由 `bar` 标志控制），对`mips` 不适用。然后，通过将 `bar` 标志添加到 `profiles/arch/mips/use.mask`，`fooapp`可以用于强制禁用不可解决的依赖项的`mips`用户。

注意`use.mask`是按标志的，而不是每个包对给定标志的使用。这是 USE 标志必须具有明确定义的特定目的的原因之一。

`use.mask`的更新应通过相关的架构团队进行处理。

更多讨论，请参见 [noblah USE 标志](./../general-concepts/use-flags.md)。
