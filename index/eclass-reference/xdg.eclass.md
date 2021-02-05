XDG.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
xdg.eclass - Provides phases for XDG compliant packages.
DESCRIPTION
Utility eclass to update the desktop, icon and shared mime info as laid out in the freedesktop specs & implementations
SUPPORTED EAPIS
4 5 6 7
FUNCTIONS
xdg_src_prepare
Prepare sources to work with XDG standards.
xdg_pkg_preinst
Finds .desktop, icon and mime info files for later handling in pkg_postinst. Locations are stored in XDG_ECLASS_DESKTOPFILES, XDG_ECLASS_ICONFILES and XDG_ECLASS_MIMEINFOFILES respectively.
xdg_pkg_postinst
Handle desktop, icon and mime info database updates.
xdg_pkg_postrm
Handle desktop, icon and mime info database updates.
AUTHORS
Original author: Gilles Dartiguelongue <eva@gentoo.org>
MAINTAINERS
freedesktop-bugs@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
xdg.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/xdg.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:02 GMT, October 11, 2020
