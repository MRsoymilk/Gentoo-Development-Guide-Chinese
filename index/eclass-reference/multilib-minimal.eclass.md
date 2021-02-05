MULTILIB-MINIMAL.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
multilib-minimal.eclass - wrapper for multilib builds providing convenient multilib_src_* functions
DESCRIPTION
src_configure, src_compile, src_test and src_install are exported.

Use multilib_src_* instead of src_* which runs this phase for all enabled ABIs.

multilib-minimal should _always_ go last in inherit order!

If you want to use in-source builds, then you must run multilib_copy_sources at the end of src_prepare! Also make sure to set correct variables such as ECONF_SOURCE=${S}

If you need generic install rules, use multilib_src_install_all function.

SUPPORTED EAPIS
4 5 6 7
MAINTAINERS
Multilib team <multilib@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
multilib-minimal.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/multilib-minimal.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:05 GMT, October 11, 2020