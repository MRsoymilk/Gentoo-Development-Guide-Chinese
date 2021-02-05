DUNE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
dune.eclass - Provides functions for installing dune packages.
DESCRIPTION
Provides dependencies on dune and ocaml and default src_compile, src_test and src_install for dune-based packages.
SUPPORTED EAPIS
5 6 7
FUNCTIONS
dune-install <list of packages>
Installs the dune packages given as arguments. For each "${pkg}" element in that list, "${pkg}.install" must be readable from "${PWD}/_build/default"
ECLASS VARIABLES
DUNE_PKG_NAME
Sets the actual dune package name, if different from gentoo package name. Set before inheriting the eclass.
AUTHORS
Rafael Kitover <rkitover@gmail.com>
MAINTAINERS
rkitover@gmail.com
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
dune.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/dune.eclass
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