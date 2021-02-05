PYTHON-SINGLE-R1.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
python-single-r1.eclass - An eclass for Python packages not installed for multiple implementations.
DESCRIPTION
An extension of the python-r1 eclass suite for packages which don't support being installed for multiple Python implementations. This mostly includes tools embedding Python and packages using foreign build systems.
This eclass sets correct IUSE. It also provides PYTHON_DEPS and PYTHON_REQUIRED_USE that need to be added to appropriate ebuild metadata variables.

The eclass exports PYTHON_SINGLE_USEDEP that is suitable for depending on other packages using the eclass. Dependencies on packages using python-r1 should be created via python_gen_cond_dep() function, using PYTHON_USEDEP placeholder.

Please note that packages support multiple Python implementations (using python-r1 eclass) can not depend on packages not supporting them (using this eclass).

Please note that python-single-r1 will always inherit python-utils-r1 as well. Thus, all the functions defined there can be used in the packages using python-single-r1, and there is no need ever to inherit both.

For more information, please see the Python Guide: https://dev.gentoo.org/~mgorny/python-guide/

SUPPORTED EAPIS
5 6 7
FUNCTIONS
python_gen_useflags [<pattern>...]
Output a list of USE flags for Python implementations which are both in PYTHON_COMPAT and match any of the patterns passed as parameters to the function.
The patterns can be either fnmatch-style patterns (matched via bash == operator against PYTHON_COMPAT values) or '-2' / '-3' to indicate appropriately all enabled Python 2/3 implementations (alike python_is_python3). Remember to escape or quote the fnmatch patterns to prevent accidental shell filename expansion.

Example:

PYTHON_COMPAT=( python{2_7,3_4} )
REQUIRED_USE="doc? ( ^^ ( $(python_gen_useflags 'python2*') ) )"
It will cause the variable to look like:

REQUIRED_USE="doc? ( ^^ ( python_single_target_python2_7 ) )"
python_gen_cond_dep <dependency> [<pattern>...]
Output a list of <dependency>-ies made conditional to USE flags of Python implementations which are both in PYTHON_COMPAT and match any of the patterns passed as the remaining parameters.
The patterns can be either fnmatch-style patterns (matched via bash == operator against PYTHON_COMPAT values) or '-2' / '-3' to indicate appropriately all enabled Python 2/3 implementations (alike python_is_python3). Remember to escape or quote the fnmatch patterns to prevent accidental shell filename expansion.

In order to enforce USE constraints on the packages, verbatim be placed in the dependency specification. It will get expanded within the function into a proper USE dependency string.

Example:

PYTHON_COMPAT=( python{2_7,3_{3,4}} pypy )
RDEPEND="$(python_gen_cond_dep \
  'dev-python/unittest2[${PYTHON_USEDEP}]' python2_7 pypy )"
It will cause the variable to look like:

RDEPEND="python_single_target_python2_7? (
    dev-python/unittest2[python_targets_python2_7(-)?,...] )
python_single_target_pypy? (
    dev-python/unittest2[python_targets_pypy(-)?,...] )"
python_gen_impl_dep [<requested-use-flags> [<impl-pattern>...]]
Output a dependency on Python implementations with the specified USE dependency string appended, or no USE dependency string if called without the argument (or with empty argument). If any implementation patterns are passed, the output dependencies will be generated only for the implementations matching them.
The patterns can be either fnmatch-style patterns (matched via bash == operator against PYTHON_COMPAT values) or '-2' / '-3' to indicate appropriately all enabled Python 2/3 implementations (alike python_is_python3). Remember to escape or quote the fnmatch patterns to prevent accidental shell filename expansion.

Use this function when you need to request different USE flags on the Python interpreter depending on package's USE flags. If you only need a single set of interpreter USE flags, just set PYTHON_REQ_USE and use ${PYTHON_DEPS} globally.

Example:

PYTHON_COMPAT=( python{2_7,3_{3,4}} pypy )
RDEPEND="foo? ( $(python_gen_impl_dep 'xml(+)') )"
It will cause the variable to look like:

RDEPEND="foo? (
  python_single_target_python2_7? (
    dev-lang/python:2.7[xml(+)] )
python_single_target_pypy? (
    dev-python/pypy[xml(+)] ) )"
python_setup
Determine what the selected Python implementation is and set the Python build environment up for it.
python-single-r1_pkg_setup
Runs python_setup.
ECLASS VARIABLES
PYTHON_COMPAT (REQUIRED)
This variable contains a list of Python implementations the package supports. It must be set before the `inherit' call. It has to be an array.
Example:

PYTHON_COMPAT=( python2_7 python3_3 python3_4 )
Please note that you can also use bash brace expansion if you like:

PYTHON_COMPAT=( python2_7 python3_{3,4} )
PYTHON_COMPAT_OVERRIDE (USER VARIABLE)
This variable can be used when working with ebuilds to override the in-ebuild PYTHON_COMPAT. It is a string naming the implementation which package will be built for. It needs to be specified in the calling environment, and not in ebuilds.
It should be noted that in order to preserve metadata immutability, PYTHON_COMPAT_OVERRIDE does not affect IUSE nor dependencies. The state of PYTHON_SINGLE_TARGET is ignored, and the implementation in PYTHON_COMPAT_OVERRIDE is built instead. Dependencies need to be satisfied manually.

Example:

PYTHON_COMPAT_OVERRIDE='pypy' emerge -1v dev-python/bar
PYTHON_REQ_USE
The list of USEflags required to be enabled on the chosen Python implementations, formed as a USE-dependency string. It should be valid for all implementations in PYTHON_COMPAT, so it may be necessary to use USE defaults.
This should be set before calling `inherit'.

Example:

PYTHON_REQ_USE="gdbm,ncurses(-)?"
It will cause the Python dependencies to look like:

python_single_target_pythonX_Y? ( dev-lang/python:X.Y[gdbm,ncurses(-)?] )
PYTHON_DEPS (GENERATED BY ECLASS)
This is an eclass-generated Python dependency string for all implementations listed in PYTHON_COMPAT.
The dependency string is conditional on PYTHON_SINGLE_TARGET.

Example use:

RDEPEND="${PYTHON_DEPS}
dev-foo/mydep"
DEPEND="${RDEPEND}"
Example value:

dev-lang/python-exec:=
python_single_target_python2_7? ( dev-lang/python:2.7[gdbm] )
python_single_target_pypy? ( dev-python/pypy[gdbm] )
PYTHON_SINGLE_USEDEP (GENERATED BY ECLASS)
This is an eclass-generated USE-dependency string which can be used to depend on another python-single-r1 package being built for the same Python implementations.
If you need to depend on a multi-impl (python-r1) package, use python_gen_cond_dep with PYTHON_USEDEP placeholder instead.

Example use:

RDEPEND="dev-python/foo[${PYTHON_SINGLE_USEDEP}]"
Example value:

python_single_target_python3_4(-)?
PYTHON_USEDEP (GENERATED BY ECLASS)
This is a placeholder variable supported by python_gen_cond_dep, in order to depend on python-r1 packages built for the same Python implementations.
Example use:

RDEPEND="$(python_gen_cond_dep '
    dev-python/foo[${PYTHON_USEDEP}]
  ')"
Example value:

python_targets_python3_4(-)
PYTHON_MULTI_USEDEP (GENERATED BY ECLASS)
This is a backwards-compatibility placeholder. Use PYTHON_USEDEP instead.
PYTHON_REQUIRED_USE (GENERATED BY ECLASS)
This is an eclass-generated required-use expression which ensures that exactly one PYTHON_SINGLE_TARGET value has been enabled.
This expression should be utilized in an ebuild by including it in REQUIRED_USE, optionally behind a use flag.

Example use:

REQUIRED_USE="python? ( ${PYTHON_REQUIRED_USE} )"
Example value:

^^ ( python_single_target_python2_7 python_single_target_python3_3 )
AUTHORS
Author: Michał Górny <mgorny@gentoo.org>
Based on work of: Krzysztof Pawlik <nelchael@gentoo.org>
MAINTAINERS
Python team <python@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
python-single-r1.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/python-single-r1.eclass
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