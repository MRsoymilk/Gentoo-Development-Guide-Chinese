LLVM.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
llvm.eclass - Utility functions to build against slotted LLVM
DESCRIPTION
The llvm.eclass provides utility functions that can be used to build against specific version of slotted LLVM (with fallback to :0 for old versions).
This eclass does not generate dependency strings. You need to write a proper dependency string yourself to guarantee that appropriate version of LLVM is installed.

Example use for a package supporting LLVM 5 to 7:

inherit cmake-utils llvm

RDEPEND="
<sys-devel/llvm-8:=
|| (
        sys-devel/llvm:7
        sys-devel/llvm:6
        sys-devel/llvm:5
)
"
DEPEND=${RDEPEND}

LLVM_MAX_SLOT=7

# only if you need to define one explicitly
pkg_setup() {
llvm_pkg_setup
do-something-else
}
Example for a package needing LLVM+clang w/ a specific target:

inherit cmake-utils llvm

# note: do not use := on both clang and llvm, it can match different
# slots then. clang pulls llvm in, so we can skip the latter.
RDEPEND="
>=sys-devel/clang-6:=[llvm_targets_AMDGPU(+)]
"
DEPEND=${RDEPEND}

llvm_check_deps() {
has_version -d "sys-devel/clang:${LLVM_SLOT}[llvm_targets_AMDGPU(+)]"
}
SUPPORTED EAPIS
6 7
FUNCTIONS
get_llvm_prefix [-b|-d] [<max_slot>]
Find the newest LLVM install that is acceptable for the package, and print an absolute path to it.
If -b is specified, the checks are performed relative to BROOT, and BROOT-path is returned. This is appropriate when your package calls llvm-config executable. -b is supported since EAPI 7.

If -d is specified, the checks are performed relative to ESYSROOT, and ESYSROOT-path is returned. This is appropriate when your package uses CMake find_package(LLVM). -d is the default.

If <max_slot> is specified, then only LLVM versions that are not newer than <max_slot> will be considered. Otherwise, all LLVM versions would be considered acceptable. The function does not support specifying minimal supported version -- the developer must ensure that a version new enough is installed via providing appropriate dependencies.

If llvm_check_deps() function is defined within the ebuild, it will be called to verify whether a particular slot is accepable. Within the function scope, LLVM_SLOT will be defined to the SLOT value (0, 4, 5...). The function should return a true status if the slot is acceptable, false otherwise. If llvm_check_deps() is not defined, the function defaults to checking whether sys-devel/llvm:${LLVM_SLOT} is installed.

llvm_pkg_setup
Prepend the appropriate executable directory for the newest acceptable LLVM slot to the PATH. For path determination logic, please see the get_llvm_prefix documentation.
The highest acceptable LLVM slot can be set in LLVM_MAX_SLOT variable. If it is unset or empty, any slot is acceptable.

The PATH manipulation is only done for source builds. The function is a no-op when installing a binary package.

If any other behavior is desired, the contents of the function should be inlined into the ebuild and modified as necessary.

ECLASS VARIABLES
LLVM_MAX_SLOT
Highest LLVM slot supported by the package. Needs to be set before llvm_pkg_setup is called. If unset, no upper bound is assumed.
AUTHORS
Michał Górny <mgorny@gentoo.org>
MAINTAINERS
Michał Górny <mgorny@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
llvm.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/llvm.eclass
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