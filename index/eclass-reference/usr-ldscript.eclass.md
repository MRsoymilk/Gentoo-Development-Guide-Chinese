USR-LDSCRIPT.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
usr-ldscript.eclass - Defines the gen_usr_ldscript function.
SUPPORTED EAPIS
4 5 6 7
FUNCTIONS
gen_usr_ldscript [-a] <list of libs to create linker scripts for>
This function generate linker scripts in /usr/lib for dynamic libs in /lib. This is to fix linking problems when you have the .so in /lib, and the .a in /usr/lib. What happens is that in some cases when linking dynamic, the .a in /usr/lib is used instead of the .so in /lib due to gcc/libtool tweaking ld's library search path. This causes many builds to fail. See bug #4411 for more info.
Note that you should in general use the unversioned name of the library (libfoo.so), as ldconfig should usually update it correctly to point to the latest version of the library present.

MAINTAINERS
Toolchain Ninjas <toolchain@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
usr-ldscript.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/usr-ldscript.eclass
Index
NAME
SUPPORTED EAPIS
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:01 GMT, October 11, 2020