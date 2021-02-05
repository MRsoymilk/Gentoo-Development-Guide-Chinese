AUTOTOOLS-MULTILIB.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
autotools-multilib.eclass - autotools-utils wrapper for multilib builds
DESCRIPTION
The autotools-multilib.eclass provides a glue between autotools-utils.eclass(5) and multilib-minimal.eclass(5), aiming to provide a convenient way to build packages using autotools for multiple ABIs.
Inheriting this eclass sets IUSE and exports default multilib_src_*() sub-phases that call autotools-utils phase functions for each ABI enabled. The multilib_src_*() functions can be defined in ebuild just like in multilib-minimal.

SUPPORTED EAPIS
4 5
AUTHORS
Author: Michał Górny <mgorny@gentoo.org>
MAINTAINERS
gx86-multilib team <multilib@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
autotools-multilib.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/autotools-multilib.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020