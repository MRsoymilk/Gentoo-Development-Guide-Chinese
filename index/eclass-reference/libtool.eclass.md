LIBTOOL.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
libtool.eclass - quickly update bundled libtool code
DESCRIPTION
This eclass patches ltmain.sh distributed with libtoolized packages with the relink and portage patch among others
Note, this eclass does not require libtool as it only applies patches to generated libtool files. We do not run the libtoolize program because that requires a regeneration of the main autotool files in order to work properly.

SUPPORTED EAPIS
0 1 2 3 4 5 6 7
FUNCTIONS
elibtoolize [dirs] [--portage] [--reverse-deps] [--patch-only] [--remove-internal-dep=xxx] [--shallow] [--no-uclibc]
Apply a smorgasbord of patches to bundled libtool files. This function should always be safe to run. If no directories are specified, then ${S} will be searched for appropriate files.
If the --shallow option is used, then only ${S}/ltmain.sh will be patched.

The other options should be avoided in general unless you know what's going on.

MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
libtool.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/libtool.eclass
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