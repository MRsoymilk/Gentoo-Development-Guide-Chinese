ASPELL-DICT-R1.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
aspell-dict-r1.eclass - An eclass to streamline the construction of ebuilds for new aspell dicts
DESCRIPTION
The aspell-dict-r1 eclass is designed to streamline the construction of ebuilds for the new aspell dictionaries (from gnu.org) which support aspell-0.50. Support for aspell-0.60 has been added by Sergey Ulanov.
SUPPORTED EAPIS
6
FUNCTIONS
aspell-dict-r1_src_configure
The aspell-dict-r1 src_configure function which is exported.
aspell-dict-r1_src_install
The aspell-dict-r1 src_install function which is exported.
ECLASS VARIABLES
ASPELL_LANG (REQUIRED)
Pure cleartext string that is included into DESCRIPTION. This is the name of the language, for instance "Hungarian". Needs to be defined before inheriting the eclass.
ASPELL_VERSION
What major version of aspell is this dictionary for? Valid values are 5, 6 or undefined. This value is used to construct SRC_URI and *DEPEND strings. If defined to 6, >=app-text/aspell-0.60 will be added to DEPEND and RDEPEND, otherwise, >=app-text/aspell-0.50 is added to DEPEND and RDEPEND. If the value is to be overridden, it needs to be overridden before inheriting the eclass.
ASPELL_SPELLANG = ${PN/aspell-/}
Short (readonly) form of the language code, generated from ${PN} For instance, 'aspell-hu' yields the value 'hu'.
AUTHORS
Original author: Seemant Kulleen

     -r1 author: David Seifert
MAINTAINERS
maintainer-needed@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
aspell-dict-r1.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/aspell-dict-r1.eclass
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
Time: 00:27:01 GMT, October 11, 2020