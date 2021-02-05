DISTUTILS-R1.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
distutils-r1.eclass - A simple eclass to build Python packages using distutils.
DESCRIPTION
A simple eclass providing functions to build Python packages using the distutils build system. It exports phase functions for all the src_* phases. Each of the phases runs two pseudo-phases: python_..._all() (e.g. python_prepare_all()) once in ${S}, then python_...() (e.g. python_prepare()) for each implementation (see: python_foreach_impl() in python-r1).
In distutils-r1_src_prepare(), the 'all' function is run before per-implementation ones (because it creates the implementations), per-implementation functions are run in a random order.

In remaining phase functions, the per-implementation functions are run before the 'all' one, and they are ordered from the least to the most preferred implementation (so that 'better' files overwrite 'worse' ones).

If the ebuild doesn't specify a particular pseudo-phase function, the default one will be used (distutils-r1_...). Defaults are provided for all per-implementation pseudo-phases, python_prepare_all() and python_install_all(); whenever writing your own pseudo-phase functions, you should consider calling the defaults (and especially distutils-r1_python_prepare_all).

Please note that distutils-r1 sets RDEPEND and DEPEND unconditionally for you.

Also, please note that distutils-r1 will always inherit python-r1 as well. Thus, all the variables defined and documented there are relevant to the packages using distutils-r1.

For more information, please see the Python Guide: https://dev.gentoo.org/~mgorny/python-guide/

SUPPORTED EAPIS
5 6 7
FUNCTIONS
distutils_enable_sphinx <subdir> [--no-autodoc | <plugin-pkgs>...]
Set up IUSE, BDEPEND, python_check_deps() and python_compile_all() for building HTML docs via dev-python/sphinx. python_compile_all() will append to HTML_DOCS if docs are enabled.
This helper is meant for the most common case, that is a single Sphinx subdirectory with standard layout, building and installing HTML docs behind USE=doc. It assumes it's the only consumer of the three aforementioned functions. If you need to use a custom implemention, you can't use it.

If your package uses additional Sphinx plugins, they should be passed (without PYTHON_USEDEP) as <plugin-pkgs>. The function will take care of setting appropriate any-of dep and python_check_deps().

If no plugin packages are specified, the eclass will still utilize any-r1 API to support autodoc (documenting source code). If the package uses neither autodoc nor additional plugins, you should pass --no-autodoc to disable this API and simplify the resulting code.

This function must be called in global scope. Take care not to overwrite the variables set by it. If you need to extend python_compile_all(), you can call the original implementation as sphinx_compile_all.

distutils_enable_tests <test-runner>
Set up IUSE, RESTRICT, BDEPEND and python_test() for running tests with the specified test runner. Also copies the current value of RDEPEND to test?-BDEPEND. The test-runner argument must be one of:
- nose: nosetests (dev-python/nose) - pytest: dev-python/pytest - setup.py: setup.py test (no deps included) - unittest: for built-in Python unittest module

This function is meant as a helper for common use cases, and it only takes care of basic setup. You still need to list additional test dependencies manually. If you have uncommon use case, you should not use it and instead enable tests manually.

This function must be called in global scope, after RDEPEND has been declared. Take care not to overwrite the variables set by it.

esetup.py [<args>...]
Run setup.py using currently selected Python interpreter (if ${EPYTHON} is set; fallback 'python' otherwise).
setup.py will be passed the following, in order: 1. ${mydistutilsargs[@]} 2. additional arguments passed to the esetup.py function.

Please note that setup.py will respect defaults (unless overridden via command-line options) from setup.cfg that is created in distutils-r1_python_compile and in distutils-r1_python_install.

This command dies on failure.

distutils_install_for_testing [<args>...]
Install the package into a temporary location for running tests. Update PYTHONPATH appropriately and set TEST_DIR to the test installation root. The Python packages will be installed in 'lib' subdir, and scripts in 'scripts' subdir (like in BUILD_DIR).
Please note that this function should be only used if package uses namespaces (and therefore proper install needs to be done to enforce PYTHONPATH) or tests rely on the results of install command. For most of the packages, tests built in BUILD_DIR are good enough.

distutils-r1_python_prepare_all
The default python_prepare_all(). It applies the patches from PATCHES array, then user patches and finally calls python_copy_sources to create copies of resulting sources for each Python implementation.
At some point in the future, it may also apply eclass-specific distutils patches and/or quirks.

distutils-r1_python_prepare
The default python_prepare(). A no-op.
distutils-r1_python_configure
The default python_configure(). A no-op.
distutils-r1_python_compile [additional-args...]
The default python_compile(). Runs 'esetup.py build'. Any parameters passed to this function will be appended to setup.py invocation, i.e. passed as options to the 'build' command.
This phase also sets up initial setup.cfg with build directories and copies upstream egg-info files if supplied.

distutils-r1_python_install [additional-args...]
The default python_install(). Runs 'esetup.py install', doing intermediate root install and handling script wrapping afterwards. Any parameters passed to this function will be appended to the setup.py invocation (i.e. as options to the 'install' command).
This phase updates the setup.cfg file with install directories.

distutils-r1_python_install_all
The default python_install_all(). It installs the documentation.
ECLASS VARIABLES
DISTUTILS_OPTIONAL
If set to a non-null value, distutils part in the ebuild will be considered optional. No dependencies will be added and no phase functions will be exported.
If you enable DISTUTILS_OPTIONAL, you have to set proper dependencies for your package (using ${PYTHON_DEPS}) and to either call distutils-r1 default phase functions or call the build system manually.

DISTUTILS_SINGLE_IMPL
If set to a non-null value, the ebuild will support setting a single Python implementation only. It will effectively replace the python-r1 eclass inherit with python-single-r1.
Note that inheriting python-single-r1 will cause pkg_setup() to be exported. It must be run in order for the eclass functions to function properly.

DISTUTILS_USE_SETUPTOOLS ?= bdepend (SET BEFORE INHERIT)
Controls adding dev-python/setuptools dependency. The allowed values are:
- no -- do not add the dependency (pure distutils package) - bdepend -- add it to BDEPEND (the default) - rdepend -- add it to BDEPEND+RDEPEND (when using entry_points) - pyproject.toml -- use pyproject2setuptools to install a project
                    using pyproject.toml (flit, poetry...) - manual -- do not add the depedency and suppress the checks
            (assumes you will take care of doing it correctly)

This variable is effective only if DISTUTILS_OPTIONAL is disabled. It needs to be set before the inherit line.

PATCHES
An array containing patches to be applied to the sources before copying them.
If unset, no custom patches will be applied.

Please note, however, that at some point the eclass may apply additional distutils patches/quirks independently of this variable.

Example:

PATCHES=( "${FILESDIR}"/${P}-make-gentoo-happy.patch )
DOCS
An array containing documents installed using dodoc. The files listed there must exist in the directory from which distutils-r1_python_install_all() is run (${S} by default).
If unset, the function will instead look up files matching default filename pattern list (from the Package Manager Specification), and install those found.

Example:

DOCS=( NEWS README )
HTML_DOCS
An array containing documents installed using dohtml. The files and directories listed there must exist in the directory from which distutils-r1_python_install_all() is run (${S} by default).
If unset, no HTML docs will be installed.

Example:

HTML_DOCS=( doc/html/. )
EXAMPLES
OBSOLETE: this variable is deprecated and banned in EAPI 6
An array containing examples installed into 'examples' doc subdirectory. The files and directories listed there must exist in the directory from which distutils-r1_python_install_all() is run (${S} by default).

The 'examples' subdirectory will be marked not to be compressed automatically.

If unset, no examples will be installed.

Example:

EXAMPLES=( examples/. demos/. )
DISTUTILS_IN_SOURCE_BUILD
If set to a non-null value, in-source builds will be enabled. If unset, the default is to use in-source builds when python_prepare() is declared, and out-of-source builds otherwise.
If in-source builds are used, the eclass will create a copy of package sources for each Python implementation in python_prepare_all(), and work on that copy afterwards.

If out-of-source builds are used, the eclass will instead work on the sources directly, prepending setup.py arguments with files in the specific root.

DISTUTILS_ALL_SUBPHASE_IMPLS
An array of patterns specifying which implementations can be used for *_all() sub-phase functions. If undefined, defaults to '*' (allowing any implementation). If multiple values are specified, implementations matching any of the patterns will be accepted.
The patterns can be either fnmatch-style patterns (matched via bash == operator against PYTHON_COMPAT values) or '-2' / '-3' to indicate appropriately all enabled Python 2/3 implementations (alike python_is_python3). Remember to escape or quote the fnmatch patterns to prevent accidental shell filename expansion.

If the restriction needs to apply conditionally to a USE flag, the variable should be set conditionally as well (e.g. in an early phase function or other convenient location).

Please remember to add a matching || block to REQUIRED_USE, to ensure that at least one implementation matching the patterns will be enabled.

Example:

REQUIRED_USE="doc? ( || ( $(python_gen_useflags 'python2*') ) )"

pkg_setup() {
    use doc && DISTUTILS_ALL_SUBPHASE_IMPLS=( 'python2*' )
}
mydistutilsargs
An array containing options to be passed to setup.py.
Example:

python_configure_all() {
        mydistutilsargs=( --enable-my-hidden-option )
}
AUTHORS
Author: Michał Górny <mgorny@gentoo.org>
Based on the work of: Krzysztof Pawlik <nelchael@gentoo.org>
MAINTAINERS
Python team <python@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
distutils-r1.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/distutils-r1.eclass
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
Time: 00:27:05 GMT, October 11, 2020