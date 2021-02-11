# 没有构建系统

有时，某些非常小的软件包仅作为单个`.c`文件发送。在这种情况下，你可以编写自己 `Makefile`并随源压缩包一起交付，也可以只从 ebuild 内手动编译，从而更好地解释原因。这是一个示例，来自`app-misc/hilite`：

```bash
src_compile() {
	$(tc-getCC) ${CFLAGS} ${CPPFLAGS} ${LDFLAGS} -o ${PN} ${P}.c || die
}
```

这是来自`x11-plugins/asclock`的示例，该示例附带了一个损坏的构建系统，该构建系统实际上不起作用：

```bash
src_compile() {
	local x
	for x in asclock parser symbols config
	do
		$(tc-getCC) \
			${CFLAGS} ${CPPFLAGS} \
			-I/usr/include \
			-Dlinux \
			-D_POSIX_C_SOURCE=199309L \
			-D_POSIX_SOURCE \
			-D_XOPEN_SOURCE \
			-D_BSD_SOURCE \
			-D_SVID_SOURCE \
			-DFUNCPROTO=15 \
			-DNARROWPROTO \
			-c -o ${x}.o ${x}.c || die "compile asclock failed"
	done
	$(tc-getCC) \
		${LDFLAGS} \
		-o asclock \
		asclock.o parser.o symbols.o config.o \
		-L/usr/lib \
		-L/usr/lib/X11 \
		-lXpm -lXext -lX11 || die "link asclock failed"
}
```

可能更好的选择是修复构建系统并将其发送到上游。
