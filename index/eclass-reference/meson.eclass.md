MESON.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
meson.eclass - common ebuild functions for meson-based packages
DESCRIPTION
This eclass contains the default phase functions for packages which use the meson build system.
SUPPORTED EAPIS
6 7
EXAMPLE
Typical ebuild using meson.eclass:
EAPI=6

inherit meson

...

src_configure() {
        local emesonargs=(
                $(meson_use qt4)
                $(meson_feature threads)
                $(meson_use bindist official_branding)
        )
        meson_src_configure
}

...

FUNCTIONS
emesonargs
Optional meson arguments as Bash array; this should be defined before calling meson_src_configure.
emesontestargs
Optional meson test arguments as Bash array; this should be defined before calling meson_src_test.
MYMESONARGS
User-controlled environment variable containing arguments to be passed to meson in meson_src_configure.
meson_use <USE flag> [option name]
Given a USE flag and meson project option, outputs a string like:

  -Doption=true
  -Doption=false

If the project option is unspecified, it defaults to the USE flag.

meson_feature <USE flag> [option name]
Given a USE flag and meson project option, outputs a string like:

  -Doption=enabled
  -Doption=disabled

If the project option is unspecified, it defaults to the USE flag.

meson_src_configure [extra meson arguments]
This is the meson_src_configure function.
meson_src_compile [extra ninja arguments]
This is the meson_src_compile function.
meson_src_test [extra meson test arguments]
This is the meson_src_test function.
meson_src_install [extra ninja install arguments]
This is the meson_src_install function.
ECLASS VARIABLES
BUILD_DIR
Build directory, location where all generated files should be placed. If this isn't set, it defaults to ${WORKDIR}/${P}-build.
EMESON_SOURCE
The location of the source files for the project; this is the source directory to pass to meson. If this isn't set, it defaults to ${S}
MAINTAINERS
William Hubbs <williamh@gentoo.org>
Mike Gilbert <floppym@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
meson.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/meson.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
EXAMPLE
FUNCTIONS
ECLASS VARIABLES
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020