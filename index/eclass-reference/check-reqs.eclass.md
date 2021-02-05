CHECK-REQS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
check-reqs.eclass - Provides a uniform way of handling ebuild which have very high build requirements
DESCRIPTION
This eclass provides a uniform way of handling ebuilds which have very high build requirements in terms of memory or disk space. It provides a function which should usually be called during pkg_setup().
The chosen action only happens when the system's resources are detected correctly and only if they are below the threshold specified by the package.

# need this much memory (does *not* check swap)
CHECKREQS_MEMORY="256M"

# need this much temporary build space
CHECKREQS_DISK_BUILD="2G"

# install will need this much space in /usr
CHECKREQS_DISK_USR="1G"

# install will need this much space in /var
CHECKREQS_DISK_VAR="1024M"

If you don't specify a value for, say, CHECKREQS_MEMORY, then the test is not carried out.

These checks should probably mostly work on non-Linux, and they should probably degrade gracefully if they don't. Probably.

SUPPORTED EAPIS
4 5 6 7
FUNCTIONS
check-reqs_pkg_setup
Exported function running the resources checks in pkg_setup phase. It should be run in both phases to ensure condition changes between pkg_pretend and pkg_setup won't affect the build.
check-reqs_pkg_pretend
Exported function running the resources checks in pkg_pretend phase.
ECLASS VARIABLES
CHECKREQS_MEMORY
How much RAM is needed? Eg.: CHECKREQS_MEMORY=15M
CHECKREQS_DISK_BUILD
How much diskspace is needed to build the package? Eg.: CHECKREQS_DISK_BUILD=2T
CHECKREQS_DISK_USR
How much space in /usr is needed to install the package? Eg.: CHECKREQS_DISK_USR=15G
CHECKREQS_DISK_VAR
How much space is needed in /var? Eg.: CHECKREQS_DISK_VAR=3000M
AUTHORS
Bo Ørsted Andresen <zlin@gentoo.org>
Original Author: Ciaran McCreesh <ciaranm@gentoo.org>
MAINTAINERS
QA Team <qa@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
check-reqs.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/check-reqs.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:02 GMT, October 11, 2020