OPAM.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
opam.eclass - Provides functions for installing opam packages.
DESCRIPTION
Provides dependencies on opam and ocaml, opam-install and a default src_install for opam-based packages.
SUPPORTED EAPIS
5 6 7
FUNCTIONS
opam-install <list of packages>
Installs the opam packages given as arguments. For each "${pkg}" element in that list, "${pkg}.install" must be readable from current working directory.
AUTHORS
Alexis Ballier <aballier@gentoo.org>
MAINTAINERS
maintainer-needed@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
opam.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/opam.eclass
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
Time: 00:27:05 GMT, October 11, 2020