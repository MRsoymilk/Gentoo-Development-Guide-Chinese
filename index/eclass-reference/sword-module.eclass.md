SWORD-MODULE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
sword-module.eclass - Simplify installation of SWORD modules
DESCRIPTION
This eclass provides dependencies, ebuild environment and the src_install function common to all app-text/sword modules published by the SWORD Project.
Note that as of 2020-07-26 module archives published by SWORD are still not versioned and it is necessary to look at respective module pages in order to see what versions the currently available files are. Once a module file has been replicated to the Gentoo mirror network it will be versioned and remain available even after upstream has changed their version, however users not using mirrors will encounter hash conflicts on updated modules. Should that happen, please notify the relevant package maintainers that a new version is available.

SUPPORTED EAPIS
7
EXAMPLE
sword-Personal-1.0.ebuild, a typical ebuild using sword-module.eclass:
EAPI=7

SWORD_MINIMUM_VERSION="1.5.1a"

inherit sword-module

DESCRIPTION="SWORD module for storing one's own commentary"
HOMEPAGE="https://crosswire.org/sword/modules/ModInfo.jsp?modName=Personal"
LICENSE="public-domain"
KEYWORDS="~amd64 ~ppc ~x86"

FUNCTIONS
sword-module_src_install
Install all the module files into directories used by app-text/sword.
ECLASS VARIABLES
SWORD_MINIMUM_VERSION (SET BEFORE INHERIT)
If set to a non-null value, specifies the minimum version of app-text/sword the module requires. This will be included in RDEPEND. If null or unset, the dependency will be unversioned. Needs to be set before the inherit line.
SWORD_MODULE ?= ${PN#sword-} (SET BEFORE INHERIT)
Case-sensitive name of the SWORD-Project module to install. If unset or null, use the name produced by removing the prefix 'sword-' from PN. Needs to be set before the inherit line.
MAINTAINERS
Marek Szuba <marecki@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
sword-module.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/sword-module.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
EXAMPLE
FUNCTIONS
ECLASS VARIABLES
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020