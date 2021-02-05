FORTRAN-2.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
fortran-2.eclass - Simplify fortran compiler management
DESCRIPTION
If you need a fortran compiler, then you should be inheriting this eclass. In case you only need optional support, please export FORTRAN_NEEDED before inheriting the eclass.
The eclass tests for working fortran compilers and exports the variables FC and F77. Optionally, it checks for extended capabilities based on the variable options selected in the ebuild The only phase function exported is fortran-2_pkg_setup.

SUPPORTED EAPIS
4 5 6 7
EXAMPLE
FORTRAN_NEEDED="lapack fortran"
inherit fortran-2

FORTRAN_NEED_OPENMP=1

FUNCTIONS
fortran_int64_abi_fflags
Return the Fortran compiler flag to enable 64 bit integers for array indices
fortran-2_pkg_setup
Setup functionality, checks for a valid fortran compiler and optionally for its openmp support.
ECLASS VARIABLES
FORTRAN_NEED_OPENMP ?= 0
Set to "1" in order to automatically have the eclass abort if the fortran compiler lacks openmp support.
FORTRAN_STANDARD ?= 77
Set this, if a special dialect needs to be supported. Generally not needed as default is sufficient.
Valid settings are any combination of: 77 90 95 2003

FORTRAN_NEEDED ?= always
If your package has an optional fortran support, set this variable to the space separated list of USE triggering the fortran dependency.
e.g. FORTRAN_NEEDED=lapack would result in

DEPEND="lapack? ( virtual/fortran )"

If unset, we always depend on virtual/fortran.

AUTHORS
Author Justin Lecher <jlec@gentoo.org>
Test functions provided by Sebastien Fabbro and Kacper Kowalik
MAINTAINERS
sci@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
fortran-2.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/fortran-2.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
EXAMPLE
FUNCTIONS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:02 GMT, October 11, 2020