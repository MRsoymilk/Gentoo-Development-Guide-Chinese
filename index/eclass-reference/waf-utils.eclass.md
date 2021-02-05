WAF-UTILS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
waf-utils.eclass - common ebuild functions for waf-based packages
DESCRIPTION
The waf-utils eclass contains functions that make creating ebuild for waf-based packages much easier. Its main features are support of common portage default settings.
SUPPORTED EAPIS
4 5 6
FUNCTIONS
waf-utils_src_configure
General function for configuring with waf.
waf-utils_src_compile
General function for compiling with waf.
waf-utils_src_install
Function for installing the package.
ECLASS VARIABLES
WAF_VERBOSE ?= ON
Set to OFF to disable verbose messages during compilation this is _not_ meant to be set in ebuilds
AUTHORS
Original Author: Gilles Dartiguelongue <eva@gentoo.org>
Various improvements based on cmake-utils.eclass: Tomáš Chvátal <scarabeus@gentoo.org>
Proper prefix support: Jonathan Callen <jcallen@gentoo.org>
MAINTAINERS
maintainer-needed@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
waf-utils.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/waf-utils.eclass
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
Time: 00:27:04 GMT, October 11, 2020