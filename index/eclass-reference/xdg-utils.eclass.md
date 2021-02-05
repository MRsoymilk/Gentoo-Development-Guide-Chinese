XDG-UTILS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
xdg-utils.eclass - Auxiliary functions commonly used by XDG compliant packages.
DESCRIPTION
This eclass provides a set of auxiliary functions needed by most XDG compliant packages. It provides XDG stack related functions such as:
 * GTK/Qt5 icon theme cache management
 * XDG .desktop files cache management
 * XDG mime information database management
SUPPORTED EAPIS
0 1 2 3 4 5 6 7
FUNCTIONS
xdg_environment_reset
Clean up environment for clean builds.
xdg_desktop_database_update
Updates the .desktop files database. Generates a list of mimetypes linked to applications that can handle them
xdg_icon_cache_update
Updates icon theme cache files under /usr/share/icons. This function should be called from pkg_postinst and pkg_postrm.
xdg_mimeinfo_database_update
Update the mime database. Creates a general list of mime types from several sources
AUTHORS
Original author: Gilles Dartiguelongue <eva@gentoo.org>
MAINTAINERS
gnome@gentoo.org
freedesktop-bugs@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
xdg-utils.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/xdg-utils.eclass
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
Time: 00:27:01 GMT, October 11, 2020