PYTHON-ANY-R1.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
python-any-r1.eclass - An eclass for packages having build-time dependency on Python.
DESCRIPTION
A minimal eclass for packages which need any Python interpreter installed without a need for explicit choice and invariability. This usually involves packages requiring Python at build-time but having no other relevance to it.
This eclass provides a minimal PYTHON_DEPS variable with a dependency string on any of the supported Python implementations. It also exports pkg_setup() which finds the best supported implementation and sets it as the active one.

Optionally, you can define a python_check_deps() function. It will be called by the eclass with EPYTHON set to each matching Python implementation and it is expected to check whether the implementation fulfills the package requirements. You can use the locally exported PYTHON_USEDEP to check USE-dependencies of relevant packages. It should return a true value (0) if the Python implementation fulfills the requirements, a false value (non-zero) otherwise.

Please note that python-any-r1 will always inherit python-utils-r1 as well. Thus, all the functions defined there can be used in the packages using python-any-r1, and there is no need ever to inherit both.

For more information, please see the Python Guide: https://dev.gentoo.org/~mgorny/python-guide/

SUPPORTED EAPIS
5 6 7
FUNCTIONS
python_gen_any_dep <dependency-block>
Generate an any-of dependency that enforces a version match between the Python interpreter and Python packages. <dependency-block> needs to list one or more dependencies with verbatim '${PYTHON_USEDEP}' references (quoted!) that will get expanded inside the function.
This should be used along with an appropriate python_check_deps() that checks which of the any-of blocks were matched.

Example use:

DEPEND="$(python_gen_any_dep '
dev-python/foo[${PYTHON_USEDEP}]
|| ( dev-python/bar[${PYTHON_USEDEP}]
        dev-python/baz[${PYTHON_USEDEP}] )')"

python_check_deps() {
has_version "dev-python/foo[${PYTHON_USEDEP}]" \
        && { has_version "dev-python/bar[${PYTHON_USEDEP}]" \
                || has_version "dev-python/baz[${PYTHON_USEDEP}]"; }
}
Example value:

|| (
(
        dev-lang/python:2.7
        dev-python/foo[python_targets_python2_7(-)?,python_single_target_python2_7(+)?]
        || ( dev-python/bar[python_targets_python2_7(-)?,python_single_target_python2_7(+)?]
                dev-python/baz[python_targets_python2_7(-)?,python_single_target_python2_7(+)?] )
)
(
        dev-lang/python:3.3
        dev-python/foo[python_targets_python3_3(-)?,python_single_target_python3_3(+)?]
        || ( dev-python/bar[python_targets_python3_3(-)?,python_single_target_python3_3(+)?]
                dev-python/baz[python_targets_python3_3(-)?,python_single_target_python3_3(+)?] )
)
)
python_setup
Determine what the best installed (and supported) Python implementation is, and set the Python build environment up for it.
This function will call python_check_deps() if defined.

python-any-r1_pkg_setup
Runs python_setup during from-source installs.
In a binary package installs is a no-op. If you need Python in pkg_* phases of a binary package, call python_setup directly.

ECLASS VARIABLES
PYTHON_COMPAT (REQUIRED)
This variable contains a list of Python implementations the package supports. It must be set before the `inherit' call. It has to be an array.
Example:

PYTHON_COMPAT=( python{2_5,2_6,2_7} )
PYTHON_COMPAT_OVERRIDE (USER VARIABLE)
This variable can be used when working with ebuilds to override the in-ebuild PYTHON_COMPAT. It is a string naming the implementation which will be used to build the package. It needs to be specified in the calling environment, and not in ebuilds.
It should be noted that in order to preserve metadata immutability, PYTHON_COMPAT_OVERRIDE does not affect dependencies. The value of EPYTHON and eselect-python preferences are ignored. Dependencies need to be satisfied manually.

Example:

PYTHON_COMPAT_OVERRIDE='pypy' emerge -1v dev-python/bar
PYTHON_REQ_USE
The list of USEflags required to be enabled on the Python implementations, formed as a USE-dependency string. It should be valid for all implementations in PYTHON_COMPAT, so it may be necessary to use USE defaults.
Example:

PYTHON_REQ_USE="gdbm,ncurses(-)?"
It will cause the Python dependencies to look like:

|| ( dev-lang/python:X.Y[gdbm,ncurses(-)?] ... )
PYTHON_DEPS (GENERATED BY ECLASS)
This is an eclass-generated Python dependency string for all implementations listed in PYTHON_COMPAT.
Any of the supported interpreters will satisfy the dependency.

Example use:

DEPEND="${RDEPEND}
${PYTHON_DEPS}"
Example value:

|| ( dev-lang/python:2.7[gdbm]
        dev-lang/python:2.6[gdbm] )
PYTHON_USEDEP (GENERATED BY ECLASS)
An eclass-generated USE-dependency string for the currently tested implementation. It is set locally for python_check_deps() call.
The generate USE-flag list is compatible with packages using python-r1, python-single-r1 and python-distutils-ng eclasses. It must not be used on packages using python.eclass.

Example use:

python_check_deps() {
has_version "dev-python/foo[${PYTHON_USEDEP}]"
}
Example value:

python_targets_python2_7(-)?,python_single_target_python2_7(+)?
AUTHORS
Author: Michał Górny <mgorny@gentoo.org>
Based on work of: Krzysztof Pawlik <nelchael@gentoo.org>
MAINTAINERS
Python team <python@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
python-any-r1.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/python-any-r1.eclass
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