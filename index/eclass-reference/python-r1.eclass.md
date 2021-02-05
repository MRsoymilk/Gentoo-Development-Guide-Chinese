PYTHON-R1.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
python-r1.eclass - A common, simple eclass for Python packages.
DESCRIPTION
A common eclass providing helper functions to build and install packages supporting being installed for multiple Python implementations.
This eclass sets correct IUSE. Modification of REQUIRED_USE has to be done by the author of the ebuild (but PYTHON_REQUIRED_USE is provided for convenience, see below). python-r1 exports PYTHON_DEPS and PYTHON_USEDEP so you can create correct dependencies for your package easily. It also provides methods to easily run a command for each enabled Python implementation and duplicate the sources for them.

Please note that python-r1 will always inherit python-utils-r1 as well. Thus, all the functions defined there can be used in the packages using python-r1, and there is no need ever to inherit both.

For more information, please see the Python Guide: https://dev.gentoo.org/~mgorny/python-guide/

SUPPORTED EAPIS
5 6 7
FUNCTIONS
python_gen_usedep <pattern> [...]
DEPRECATED. Please use python_gen_cond_dep instead.
Output a USE dependency string for Python implementations which are both in PYTHON_COMPAT and match any of the patterns passed as parameters to the function.

The patterns can be either fnmatch-style patterns (matched via bash == operator against PYTHON_COMPAT values) or '-2' / '-3' to indicate appropriately all enabled Python 2/3 implementations (alike python_is_python3). Remember to escape or quote the fnmatch patterns to prevent accidental shell filename expansion.

When all implementations are requested, please use ${PYTHON_USEDEP} instead. Please also remember to set an appropriate REQUIRED_USE to avoid ineffective USE flags.

Example:

PYTHON_COMPAT=( python{2_7,3_4} )
DEPEND="doc? ( dev-python/epydoc[$(python_gen_usedep 'python2*')] )"
It will cause the dependency to look like:

DEPEND="doc? ( dev-python/epydoc[python_targets_python2_7?] )"
python_gen_useflags [<pattern>...]
Output a list of USE flags for Python implementations which are both in PYTHON_COMPAT and match any of the patterns passed as parameters to the function.
The patterns can be either fnmatch-style patterns (matched via bash == operator against PYTHON_COMPAT values) or '-2' / '-3' to indicate appropriately all enabled Python 2/3 implementations (alike python_is_python3). Remember to escape or quote the fnmatch patterns to prevent accidental shell filename expansion.

Example:

PYTHON_COMPAT=( python{2_7,3_4} )
REQUIRED_USE="doc? ( || ( $(python_gen_useflags python2*) ) )"
It will cause the variable to look like:

REQUIRED_USE="doc? ( || ( python_targets_python2_7 ) )"
python_gen_cond_dep <dependency> [<pattern>...]
Output a list of <dependency>-ies made conditional to USE flags of Python implementations which are both in PYTHON_COMPAT and match any of the patterns passed as the remaining parameters.
The patterns can be either fnmatch-style patterns (matched via bash == operator against PYTHON_COMPAT values) or '-2' / '-3' to indicate appropriately all enabled Python 2/3 implementations (alike python_is_python3). Remember to escape or quote the fnmatch patterns to prevent accidental shell filename expansion.

In order to enforce USE constraints on the packages, verbatim specification. It will get expanded within the function into a proper USE dependency string.

Example:

PYTHON_COMPAT=( python{2_7,3_{3,4}} pypy )
RDEPEND="$(python_gen_cond_dep \
  'dev-python/unittest2[${PYTHON_USEDEP}]' python2_7 pypy )"
It will cause the variable to look like:

RDEPEND="python_targets_python2_7? (
    dev-python/unittest2[python_targets_python2_7?] )
python_targets_pypy? (
    dev-python/unittest2[python_targets_pypy?] )"
python_gen_impl_dep [<requested-use-flags> [<impl-pattern>...]]
Output a dependency on Python implementations with the specified USE dependency string appended, or no USE dependency string if called without the argument (or with empty argument). If any implementation patterns are passed, the output dependencies will be generated only for the implementations matching them.
The patterns can be either fnmatch-style patterns (matched via bash == operator against PYTHON_COMPAT values) or '-2' / '-3' to indicate appropriately all enabled Python 2/3 implementations (alike python_is_python3). Remember to escape or quote the fnmatch patterns to prevent accidental shell filename expansion.

Use this function when you need to request different USE flags on the Python interpreter depending on package's USE flags. If you only need a single set of interpreter USE flags, just set PYTHON_REQ_USE and use ${PYTHON_DEPS} globally.

Example:

PYTHON_COMPAT=( python{2_7,3_{3,4}} pypy )
RDEPEND="foo? ( $(python_gen_impl_dep 'xml(+)') )"
It will cause the variable to look like:

RDEPEND="foo? (
  python_targets_python2_7? (
    dev-lang/python:2.7[xml(+)] )
python_targets_pypy? (
    dev-python/pypy[xml(+)] ) )"
python_gen_any_dep <dependency-block> [<impl-pattern>...]
Generate an any-of dependency that enforces a version match between the Python interpreter and Python packages. <dependency-block> needs to list one or more dependencies with verbatim '${PYTHON_USEDEP}' references (quoted!) that will get expanded inside the function. Optionally, patterns may be specified to restrict the dependency to a subset of Python implementations supported by the ebuild.
The patterns can be either fnmatch-style patterns (matched via bash == operator against PYTHON_COMPAT values) or '-2' / '-3' to indicate appropriately all enabled Python 2/3 implementations (alike python_is_python3). Remember to escape or quote the fnmatch patterns to prevent accidental shell filename expansion.

This should be used along with an appropriate python_check_deps() that checks which of the any-of blocks were matched, and python_setup call that enables use of the matched implementation.

Example use:

DEPEND="$(python_gen_any_dep '
dev-python/foo[${PYTHON_USEDEP}]
|| ( dev-python/bar[${PYTHON_USEDEP}]
        dev-python/baz[${PYTHON_USEDEP}] )' -2)"

python_check_deps() {
has_version "dev-python/foo[${PYTHON_USEDEP}]" \
        && { has_version "dev-python/bar[${PYTHON_USEDEP}]" \
                || has_version "dev-python/baz[${PYTHON_USEDEP}]"; }
}

src_compile() {
python_foreach_impl usual_code

# some common post-build task that requires Python 2
python_setup -2
emake frobnicate
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
python_copy_sources
Create a single copy of the package sources for each enabled Python implementation.
The sources are always copied from initial BUILD_DIR (or S if unset) to implementation-specific build directory matching BUILD_DIR used by python_foreach_abi().

python_foreach_impl <command> [<args>...]
Run the given command for each of the enabled Python implementations. If additional parameters are passed, they will be passed through to the command.
The function will return 0 status if all invocations succeed. Otherwise, the return code from first failing invocation will be returned.

For each command being run, EPYTHON, PYTHON and BUILD_DIR are set locally, and the former two are exported to the command environment.

python_setup [<impl-pattern>...]
Find the best (most preferred) Python implementation that is suitable for running common Python code. Set the Python build environment up for that implementation. This function has two modes of operation: pure and any-of dep.
The pure mode is used if python_check_deps() function is not declared. In this case, an implementation is considered suitable if it is supported (in PYTHON_COMPAT), enabled (via USE flags) and matches at least one of the patterns passed (or '*' if no patterns passed).

Implementation restrictions in the pure mode need to be accompanied by appropriate REQUIRED_USE constraints. Otherwise, the eclass may fail at build time due to unsatisfied dependencies.

The any-of dep mode is used if python_check_deps() is declared. In this mode, an implementation is considered suitable if it is supported, matches at least one of the patterns and python_check_deps() has successful return code. USE flags are not considered.

The python_check_deps() function in the any-of mode needs to be accompanied by appropriate any-of dependencies.

The patterns can be either fnmatch-style patterns (matched via bash == operator against PYTHON_COMPAT values) or '-2' / '-3' to indicate appropriately all enabled Python 2/3 implementations (alike python_is_python3). Remember to escape or quote the fnmatch patterns to prevent accidental shell filename expansion.

This function needs to be used when Python is being called outside of python_foreach_impl calls (e.g. for shared processes like doc building). python_foreach_impl sets up the build environment itself.

Pure mode example:

DEPEND="doc? ( dev-python/epydoc[$(python_gen_usedep 'python2*')] )"
REQUIRED_USE="doc? ( $(python_gen_useflags 'python2*') )"

src_compile() {
  #...
  if use doc; then
    python_setup 'python2*'
    make doc
  fi
}
Any-of mode example:

DEPEND="doc? (
$(python_gen_any_dep 'dev-python/epydoc[${PYTHON_USEDEP}]' 'python2*') )"

python_check_deps() {
has_version "dev-python/epydoc[${PYTHON_USEDEP}]"
}

src_compile() {
  #...
  if use doc; then
    python_setup 'python2*'
    make doc
  fi
}
python_replicate_script <path>...
Copy the given script to variants for all enabled Python implementations, then replace it with a symlink to the wrapper.
All specified files must start with a 'python' shebang. A file not having a matching shebang will be refused.

ECLASS VARIABLES
PYTHON_COMPAT (REQUIRED)
This variable contains a list of Python implementations the package supports. It must be set before the `inherit' call. It has to be an array.
Example:

PYTHON_COMPAT=( python2_7 python3_3 python3_4 )
Please note that you can also use bash brace expansion if you like:

PYTHON_COMPAT=( python2_7 python3_{3,4} )
PYTHON_COMPAT_OVERRIDE (USER VARIABLE)
This variable can be used when working with ebuilds to override the in-ebuild PYTHON_COMPAT. It is a string listing all the implementations which package will be built for. It need be specified in the calling environment, and not in ebuilds.
It should be noted that in order to preserve metadata immutability, PYTHON_COMPAT_OVERRIDE does not affect IUSE nor dependencies. The state of PYTHON_TARGETS is ignored, and all the implementations in PYTHON_COMPAT_OVERRIDE are built. Dependencies need to be satisfied manually.

Example:

PYTHON_COMPAT_OVERRIDE='pypy python3_3' emerge -1v dev-python/foo
PYTHON_REQ_USE
The list of USEflags required to be enabled on the chosen Python implementations, formed as a USE-dependency string. It should be valid for all implementations in PYTHON_COMPAT, so it may be necessary to use USE defaults.
This should be set before calling `inherit'.

Example:

PYTHON_REQ_USE="gdbm,ncurses(-)?"
It will cause the Python dependencies to look like:

python_targets_pythonX_Y? ( dev-lang/python:X.Y[gdbm,ncurses(-)?] )
PYTHON_DEPS (GENERATED BY ECLASS)
This is an eclass-generated Python dependency string for all implementations listed in PYTHON_COMPAT.
Example use:

RDEPEND="${PYTHON_DEPS}
dev-foo/mydep"
DEPEND="${RDEPEND}"
Example value:

dev-lang/python-exec:=
python_targets_python2_7? ( dev-lang/python:2.7[gdbm] )
python_targets_pypy? ( dev-python/pypy[gdbm] )
PYTHON_USEDEP (GENERATED BY ECLASS)
This is an eclass-generated USE-dependency string which can be used to depend on another Python package being built for the same Python implementations.
The generate USE-flag list is compatible with packages using python-r1 and python-distutils-ng eclasses. It must not be used on packages using python.eclass.

Example use:

RDEPEND="dev-python/foo[${PYTHON_USEDEP}]"
Example value:

python_targets_python2_7(-)?,python_targets_python3_4(-)?
PYTHON_REQUIRED_USE (GENERATED BY ECLASS)
This is an eclass-generated required-use expression which ensures at least one Python implementation has been enabled.
This expression should be utilized in an ebuild by including it in REQUIRED_USE, optionally behind a use flag.

Example use:

REQUIRED_USE="python? ( ${PYTHON_REQUIRED_USE} )"
Example value:

|| ( python_targets_python2_7 python_targets_python3_4 )
BUILD_DIR (GENERATED BY ECLASS)
The current build directory. In global scope, it is supposed to contain an initial build directory; if unset, it defaults to ${S}.
In functions run by python_foreach_impl(), the BUILD_DIR is locally set to an implementation-specific build directory. That path is created through appending a hyphen and the implementation name to the final component of the initial BUILD_DIR.

Example value:

${WORKDIR}/foo-1.3-python2_7
AUTHORS
Author: Michał Górny <mgorny@gentoo.org>
Based on work of: Krzysztof Pawlik <nelchael@gentoo.org>
MAINTAINERS
Python team <python@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
python-r1.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/python-r1.eclass
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