KDE.ORG.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
kde.org.eclass - Support eclass for packages that are hosted on kde.org infrastructure.
DESCRIPTION
This eclass is mainly providing facilities for the upstream release groups Frameworks, Plasma, Applications to assemble default SRC_URI for tarballs, set up git-r3.eclass for stable/master branch versions or restrict access to unreleased (packager access only) tarballs in Gentoo KDE overlay, but it may be also used by any other package hosted on kde.org. It also contains default meta variables for settings not specific to any particular build system.
SUPPORTED EAPIS
7
FUNCTIONS
kde.org_pkg_nofetch
Intended for use in the KDE overlay. If this package matches something in KDE_UNRELEASED, display a giant warning that the package has not yet been released upstream and should not be used.
kde.org_src_unpack
Unpack the sources, automatically handling both release and live ebuilds.
ECLASS VARIABLES
KDE_BUILD_TYPE = "release"
If PV matches "*9999*", this is automatically set to "live". Otherwise, this is automatically set to "release".
KDE_ORG_NAME ?= $PN
If unset, default value is set to ${PN}. Name of the package as hosted on kde.org mirrors.
KDE_RELEASE_SERVICE ?= false
If set to "false", do nothing. If set to "true", set SRC_URI accordingly and apply KDE_UNRELEASED.
KDE_SELINUX_MODULE ?= none
If set to "none", do nothing. For any other value, add selinux to IUSE, and depending on that useflag add a dependency on sec-policy/selinux-${KDE_SELINUX_MODULE} to (R)DEPEND.
MAINTAINERS
kde@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
kde.org.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/kde.org.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
ECLASS VARIABLES
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020