LLVM.ORG.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
llvm.org.eclass - Common bits for fetching & unpacking llvm.org projects
DESCRIPTION
The llvm.org eclass provides common code to fetch and unpack parts of the llvm.org project tree. It takes care of handling both git checkouts and source tarballs, making it possible to unify the code of live and release ebuilds and effectively reduce the work needed to package new releases/RCs/branches.
In order to use this eclass, the ebuild needs to declare LLVM_COMPONENTS and then call llvm.org_set_globals. If tests require additional components, they need to be listed in LLVM_TEST_COMPONENTS. The eclass exports an implementation of src_unpack() phase.

Example:

inherit llvm.org

LLVM_COMPONENTS=( lld )
LLVM_TEST_COMPONENTS=( llvm/utils/lit )
llvm.org_set_globals
FUNCTIONS
llvm.org_set_globals
Set global variables. This must be called after setting LLVM_* variables used by the eclass.
llvm.org_src_unpack
Unpack or checkout requested LLVM components.
llvm.org_src_prepare
Call appropriate src_prepare (cmake or default) depending on inherited eclasses. Make sure that PATCHES and user patches are applied in top ${WORKDIR}, so that patches straight from llvm-project repository work correctly with -p1.
get_lit_flags
Get the standard recommended lit flags for running tests, in CMake list form (;-separated).
ECLASS VARIABLES
LLVM_COMPONENTS (REQUIRED)
List of components needed unconditionally. Specified as bash array with paths relative to llvm-project git. Automatically translated for tarball releases.
The first path specified is used to construct default S.

LLVM_TEST_COMPONENTS
List of additional components needed for tests.
LIT_JOBS (USER VARIABLE)
Number of test jobs to run simultaneously. If unset, defaults to '-j' in MAKEOPTS. If that is not found, default to nproc.
AUTHORS
Michał Górny <mgorny@gentoo.org>
MAINTAINERS
Michał Górny <mgorny@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
llvm.org.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/llvm.org.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020