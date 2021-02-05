GNOME.ORG.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
gnome.org.eclass - Helper eclass for gnome.org hosted archives
DESCRIPTION
Provide a default SRC_URI for tarball hosted on gnome.org mirrors.
ECLASS VARIABLES
GNOME_TARBALL_SUFFIX
Most projects hosted on gnome.org mirrors provide tarballs as tar.bz2 or tar.xz. This eclass defaults to bz2 for EAPI 0, 1, 2, 3 and defaults to xz for everything else. This is because the gnome mirrors are moving to only have xz tarballs for new releases.
GNOME_ORG_MODULE ?= $PN
Name of the module as hosted on gnome.org mirrors. Leave unset if package name matches module name.
AUTHORS
Authors: Spidler <spidler@gentoo.org> with help of carparski.
eclass variable additions and documentation: Gilles Dartiguelongue <eva@gentoo.org>
MAINTAINERS
gnome@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
gnome.org.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/gnome.org.eclass
Index
NAME
DESCRIPTION
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020