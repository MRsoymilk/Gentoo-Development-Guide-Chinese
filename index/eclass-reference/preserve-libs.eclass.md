PRESERVE-LIBS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
preserve-libs.eclass - preserve libraries after SONAME changes
FUNCTIONS
preserve_old_lib <libs to preserve> [more libs]
These functions are useful when a lib in your package changes ABI SONAME. An example might be from libogg.so.0 to libogg.so.1. Removing libogg.so.0 would break packages that link against it. Most people get around this by using the portage SLOT mechanism, but that is not always a relevant solution, so instead you can call this from pkg_preinst. See also the preserve_old_lib_notify function.
preserve_old_lib_notify <libs to notify> [more libs]
Spit helpful messages about the libraries preserved by preserve_old_lib.
MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
preserve-libs.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/preserve-libs.eclass
Index
NAME
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020