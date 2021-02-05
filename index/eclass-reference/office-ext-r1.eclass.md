OFFICE-EXT-R1.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
office-ext-r1.eclass - Eclass for installing libreoffice/openoffice extensions
DESCRIPTION
Eclass for easing maintenance of libreoffice/openoffice extensions.
SUPPORTED EAPIS
5 6 7
FUNCTIONS
office-ext-r1_src_unpack
Flush the cache after removal of an extension.
office-ext-r1_src_install
Install the extension source to the proper location.
office-ext-r1_add_extension
Install the extension into the libreoffice/openoffice.
office-ext-r1_remove_extension
Remove the extension from the libreoffice/openoffice.
office-ext-r1_pkg_postinst
Add the extensions to the openoffice.
office-ext-r1_pkg_prerm
Remove the extensions from the openoffice.
ECLASS VARIABLES
OFFICE_REQ_USE
Useflags required on office implementation for the extension.
Example:

OFFICE_REQ_USE="java,jemalloc(-)?"
OFFICE_IMPLEMENTATIONS = ( "libreoffice" "openoffice" )
List of implementations supported by the extension. Some work only for libreoffice and vice versa. Default value is all implementations.
Example:

OFFICE_IMPLEMENTATIONS=( "libreoffice" "openoffice" )
OFFICE_EXTENSIONS (REQUIRED)
Array containing list of extensions to install.
Example:

OFFICE_EXTENSIONS=( ${PN}_${PV}.oxt )
OFFICE_EXTENSIONS_LOCATION ?= ${DISTDIR}
Path to the extensions location. Defaults to ${DISTDIR}.
Example:

OFFICE_EXTENSIONS_LOCATION="${S}/unpacked/"
AUTHORS
Tomáš Chvátal <scarabeus@gentoo.org>
MAINTAINERS
The office team <office@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
office-ext-r1.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/office-ext-r1.eclass
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
Time: 00:27:03 GMT, October 11, 2020