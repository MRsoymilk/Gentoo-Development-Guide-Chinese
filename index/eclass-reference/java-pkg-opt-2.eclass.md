JAVA-PKG-OPT-2.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
java-pkg-opt-2.eclass - Eclass for package with optional Java support
DESCRIPTION
Inherit this eclass instead of java-pkg-2 if you only need optional Java support.
FUNCTIONS
java-pkg-opt-2_pkg_setup
default pkg_setup, wrapper for java-utils-2_pkg_init
java-pkg-opt-2_src_prepare
default src_prepare, wrapper for java-utils-2_src_prepare
java-pkg-opt-2_pkg_preinst
default pkg_preinst, wrapper for java-utils-2_pkg_preinst
ECLASS VARIABLES
JAVA_PKG_OPT_USE = ${JAVA_PKG_OPT_USE:-java}
USE flag to control if optional Java stuff is build. Defaults to 'java'.
AUTHORS
Thomas Matthijs <axxo@gentoo.org>
MAINTAINERS
java@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
java-pkg-opt-2.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/java-pkg-opt-2.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020
