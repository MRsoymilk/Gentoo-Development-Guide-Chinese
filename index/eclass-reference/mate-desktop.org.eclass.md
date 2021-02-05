MATE-DESKTOP.ORG.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
mate-desktop.org.eclass - Helper eclass for mate-desktop.org hosted archives
DESCRIPTION
Provide a default SRC_URI and EGIT_REPO_URI for MATE packages as well as exporting some useful values like the MATE_BRANCH
SUPPORTED EAPIS
6
ECLASS VARIABLES
MATE_DESKTOP_ORG_PN ?= $PN
Name of the package as hosted on mate-desktop.org. Leave unset if package name matches PN.
MATE_DESKTOP_ORG_PV ?= $PV
Package version string as listed on mate-desktop.org. Leave unset if package version string matches PV.
MATE_BRANCH ?= "$(get_version_component_range 1-2)"
Major and minor numbers of the version number, unless live. If live ebuild, will be set to '9999'.
AUTHORS
Authors: NP-Hardass <NP-Hardass@gentoo.org> based upon the gnome.org eclass.
MAINTAINERS
mate@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
mate-desktop.org.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/mate-desktop.org.eclass
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
Time: 00:27:04 GMT, October 11, 2020