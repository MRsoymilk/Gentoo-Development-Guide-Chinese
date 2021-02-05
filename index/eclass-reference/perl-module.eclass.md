PERL-MODULE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
perl-module.eclass - eclass for installing Perl module distributions
DESCRIPTION
The perl-module eclass is designed to allow easier installation of Perl module distributions, and their incorporation into the Gentoo Linux system. All exported functions from perl-functions.eclass (inherited here) explicitly also belong to the interface of perl-module.eclass. If your package does not use any Perl-specific build system (as, e.g., ExtUtils::MakeMaker or Module::Build), we recommend to use perl-functions.eclass instead.
SUPPORTED EAPIS
5 6 7
FUNCTIONS
perl-module_src_unpack
Unpack the ebuild tarball(s). This function is to be called during the ebuild src_unpack() phase.
perl-module_src_prepare
Get the ebuild sources ready. This function is to be called during the ebuild src_prepare() phase.
perl-module_src_configure
Configure the ebuild sources. This function is to be called during the ebuild src_configure() phase.
perl-module_src_compile
Compile the ebuild sources. This function is to be called during the ebuild src_compile() phase.
perl-module_src-test
This code attempts to work out your threadingness and runs tests according to the settings of DIST_TEST using Test::Harness.
perl-module_src_install
Install a Perl ebuild. This function is to be called during the ebuild src_install() phase.
perl-module_pkg_postinst
This function is to be called during the pkg_postinst() phase. It only does useful things for the perl-core category, where it handles the file renaming and symbolic links that prevent file collisions for dual-life packages installing scripts. In any other category it immediately exits.
perl-module_pkg_postrm
This function is to be called during the pkg_postrm() phase. It only does useful things for the perl-core category, where it handles the file renaming and symbolic links that prevent file collisions for dual-life packages installing scripts. In any other category it immediately exits.
ECLASS VARIABLES
GENTOO_DEPEND_ON_PERL
This variable controls whether a runtime and build time dependency on dev-lang/perl is automatically added by the eclass. It defaults to yes. Set to no to disable, set to noslotop to add a perl dependency without slot operator (EAPI=6). All packages installing into the vendor_perl path must use yes here.
DIST_NAME
(EAPI=6 and later) This variable provides a way to override PN for the calculation of S, SRC_URI, and HOMEPAGE. If unset, defaults to PN.
DIST_VERSION
(EAPI=6 and later) This variable provides a way to override PV for the calculation of S and SRC_URI. Use it to provide the non-normalized, upstream version number. If unset, defaults to PV. Named MODULE_VERSION in EAPI=5.
DIST_A_EXT
(EAPI=6 and later) This variable provides a way to override the distfile extension for the calculation of SRC_URI. If unset, defaults to tar.gz. Named MODULE_A_EXT in EAPI=5.
DIST_A
(EAPI=6 and later) This variable provides a way to override the distfile name for the calculation of SRC_URI. If unset, defaults to ${DIST_NAME}-${DIST_VERSION}.${DIST_A_EXT} Named MODULE_A in EAPI=5.
DIST_AUTHOR
(EAPI=6 and later) This variable sets the module author name for the calculation of SRC_URI. Named MODULE_AUTHOR in EAPI=5.
DIST_SECTION
(EAPI=6 and later) This variable sets the module section for the calculation of SRC_URI. Only required in rare cases for very special snowflakes. Named MODULE_SECTION in EAPI=5.
DIST_EXAMPLES
(EAPI=6 and later) This Bash array allows passing a list of example files to be installed in /usr/share/doc/${PF}/examples. If set before inherit, automatically adds a use-flag examples, if not you'll have to add the useflag in your ebuild. Examples are installed only if the useflag examples exists and is activated.
DIST_TEST
(EAPI=6 and later) Variable that controls if tests are run in the test phase at all, and if yes under which conditions. If unset, defaults to "do parallel" If neither "do" nor "parallel" is recognized, tests are skipped. (In EAPI=5 the variable is called SRC_TEST, defaults to "skip", and recognizes fewer options.) The following space-separated keywords are recognized:
  do       : run tests
  parallel : run tests in parallel
  verbose  : increase test verbosity
  network  : do not try to disable network tests
DIST_TEST_OVERRIDE
(EAPI=6 and later) Variable that controls if tests are run in the test phase at all, and if yes under which conditions. It is intended for use in make.conf or the environment by ebuild authors during testing, and accepts the same values as DIST_TEST. If set, it overrides DIST_TEST completely. DO NOT USE THIS IN EBUILDS!
AUTHORS
Seemant Kulleen <seemant@gentoo.org>
Andreas K. Hüttel <dilfridge@gentoo.org>
MAINTAINERS
perl@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
perl-module.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/perl-module.eclass
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
Time: 00:27:02 GMT, October 11, 2020