OASIS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
oasis.eclass - Provides common ebuild phases for oasis-based packages.
DESCRIPTION
Provides common ebuild phases for oasis-based packages. Most of these packages will just have to inherit the eclass, set their dependencies and the DOCS variable for base.eclass to install it and be done.
It inherits multilib, findlib, eutils and base eclasses. Ebuilds using oasis.eclass must be EAPI>=3.

SUPPORTED EAPIS
3 4 5
FUNCTIONS
oasis_use_enable < useflag > < variable >
A use_enable-like function for oasis configure variables. Outputs '--override variable (true|false)', whether useflag is enabled or not. Typical usage: $(oasis_use_enable ocamlopt is_native) as an oasis configure argument.
oasis_src_configure
src_configure phase shared by oasis-based packages. Extra arguments may be passed via oasis_configure_opts.
oasis_src_compile
Builds an oasis-based package. Will build documentation if OASIS_BUILD_DOCS is defined and the doc useflag is enabled.
oasis_src_test
Runs the testsuite of an oasis-based package.
oasis_src_install
Installs an oasis-based package. It calls base_src_install_docs, so will install documentation declared in the DOCS variable.
ECLASS VARIABLES
OASIS_BUILD_DOCS
Will make oasis_src_compile build the documentation if this variable is defined and the doc useflag is enabled. The eclass takes care of setting doc in IUSE but the ebuild should take care of the extra dependencies it may need. Set before inheriting the eclass.
OASIS_BUILD_TESTS
Will make oasis_src_configure enable building the tests if the test useflag is enabled. oasis_src_test will then run them. Note that you sometimes need to enable this for src_test to be useful, sometimes not. It has to be enabled on a per-case basis. The eclass takes care of setting test in IUSE but the ebuild should take care of the extra dependencies it may need. Set before inheriting the eclass.
OASIS_NO_DEBUG
Disable debug useflag usage. Old oasis versions did not support it so we allow disabling it in those cases. The eclass takes care of setting debug in IUSE. Set before inheriting the eclass.
OASIS_DOC_DIR ?= "/usr/share/doc/${PF}/html"
Specify where to install documentation. Default is for ocamldoc HTML. Change it before inherit if this is not what you want. EPREFIX is automatically prepended.
AUTHORS
Original Author: Alexis Ballier <aballier@gentoo.org>
MAINTAINERS
maintainer-needed@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
oasis.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/oasis.eclass
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