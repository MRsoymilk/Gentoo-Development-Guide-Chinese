SGML-CATALOG-R1.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
sgml-catalog-r1.eclass - Functions for installing SGML catalogs
DESCRIPTION
This eclass regenerates /etc/sgml/catalog, /etc/sgml.{,c}env and /etc/env.d/93sgmltools-lite as necessary for the DocBook tooling. This is done via exported pkg_postinst and pkg_postrm phases.
SUPPORTED EAPIS
7
FUNCTIONS
sgml-catalog-r1_update_catalog
Regenerate /etc/sgml/catalog to include all installed catalogs.
sgml-catalog-r1_update_env
Regenerate environment variables and copy them to env.d.
AUTHORS
Michał Górny <mgorny@gentoo.org>
MAINTAINERS
Michał Górny <mgorny@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
sgml-catalog-r1.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/sgml-catalog-r1.eclass
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