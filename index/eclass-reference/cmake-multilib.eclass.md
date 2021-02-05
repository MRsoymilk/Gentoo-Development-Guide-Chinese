CMAKE-MULTILIB.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
cmake-multilib.eclass - cmake-utils wrapper for multilib builds
DESCRIPTION
The cmake-multilib.eclass provides a glue between cmake-utils.eclass(5) and multilib-minimal.eclass(5), aiming to provide a convenient way to build packages using cmake for multiple ABIs.
Inheriting this eclass sets IUSE and exports default multilib_src_*() sub-phases that call cmake-utils phase functions for each ABI enabled. The multilib_src_*() functions can be defined in ebuild just like in multilib-minimal, yet they ought to call appropriate cmake-utils phase rather than 'default'.

SUPPORTED EAPIS
6 7
ECLASS VARIABLES
CMAKE_ECLASS ?= cmake-utils
Default is "cmake-utils" for compatibility. Specify "cmake" for ebuilds that ported from cmake-utils.eclass to cmake.eclass already.
AUTHORS
Author: Michał Górny <mgorny@gentoo.org>
MAINTAINERS
gx86-multilib team <multilib@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
cmake-multilib.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/cmake-multilib.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020