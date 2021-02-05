OUT-OF-SOURCE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
out-of-source.eclass - convenient wrapper to build autotools packages out-of-source
DESCRIPTION
This eclass provides a minimalistic wrapper interface to easily build autotools (and alike) packages out-of-source. It is meant to resemble the interface used by multilib-minimal without actually requiring the package to be multilib.
For the simplest ebuilds, it is enough to inherit the eclass and the new phase functions will automatically build the package out-of-source. If you need to redefine one of the default phases src_configure() through src_install(), you need to define the matching sub-phases: my_src_configure(), my_src_compile(), my_src_test() and/or my_src_install(). Those sub-phase functions will be run inside the build directory. Additionally, my_src_install_all() is provided to perform doc-install and other common tasks that are done in source directory.

Example use:

inherit out-of-source

my_src_configure() {
    econf \
        --disable-static
}
SUPPORTED EAPIS
6 7
FUNCTIONS
out-of-source_src_configure
The default src_configure() implementation establishes a BUILD_DIR, sets ECONF_SOURCE to the current directory (usually S), and runs my_src_configure() (or the default) inside it.
out-of-source_src_compile
The default src_compile() implementation runs my_src_compile() (or the default) inside the build directory.
out-of-source_src_test
The default src_test() implementation runs my_src_test() (or the default) inside the build directory.
out-of-source_src_install
The default src_install() implementation runs my_src_install() (or the 'make install' part of the default) inside the build directory, followed by a call to my_src_install_all() (or 'einstalldocs' part of the default) in the original working directory.
MAINTAINERS
Michał Górny <mgorny@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
out-of-source.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/out-of-source.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:02 GMT, October 11, 2020