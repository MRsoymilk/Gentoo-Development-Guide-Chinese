PYTHON-UTILS-R1.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
python-utils-r1.eclass - Utility functions for packages with Python parts.
DESCRIPTION
A utility eclass providing functions to query Python implementations, install Python modules and scripts.
This eclass does not set any metadata variables nor export any phase functions. It can be inherited safely.

For more information, please see the Python Guide: https://dev.gentoo.org/~mgorny/python-guide/

SUPPORTED EAPIS
5 6 7
FUNCTIONS
python_get_sitedir [<impl>]
Obtain and print the 'site-packages' path for the given implementation. If no implementation is provided, ${EPYTHON} will be used.
python_get_includedir [<impl>]
Obtain and print the include path for the given implementation. If no implementation is provided, ${EPYTHON} will be used.
python_get_library_path [<impl>]
Obtain and print the Python library path for the given implementation. If no implementation is provided, ${EPYTHON} will be used.
Please note that this function can be used with CPython only. Use in another implementation will result in a fatal failure.

python_get_CFLAGS [<impl>]
Obtain and print the compiler flags for building against Python, for the given implementation. If no implementation is provided, ${EPYTHON} will be used.
Please note that this function can be used with CPython only. It requires Python and pkg-config installed, and therefore proper build-time dependencies need be added to the ebuild.

python_get_LIBS [<impl>]
Obtain and print the compiler flags for linking against Python, for the given implementation. If no implementation is provided, ${EPYTHON} will be used.
Please note that this function can be used with CPython only. It requires Python and pkg-config installed, and therefore proper build-time dependencies need be added to the ebuild.

python_get_PYTHON_CONFIG [<impl>]
Obtain and print the PYTHON_CONFIG location for the given implementation. If no implementation is provided, ${EPYTHON} will be used.
Please note that this function can be used with CPython only. It requires Python installed, and therefore proper build-time dependencies need be added to the ebuild.

python_get_scriptdir [<impl>]
Obtain and print the script install path for the given implementation. If no implementation is provided, ${EPYTHON} will be used.
python_optimize [<directory>...]
Compile and optimize Python modules in specified directories (absolute paths). If no directories are provided, the default system paths are used (prepended with ${D}).
python_scriptinto <new-path>
Set the directory to which files passed to python_doexe(), python_doscript(), python_newexe() and python_newscript() are going to be installed. The new value needs to be relative to the installation root (${ED}).
If not set explicitly, the directory defaults to /usr/bin.

Example:

src_install() {
  python_scriptinto /usr/sbin
  python_foreach_impl python_doscript foo
}
python_doexe <files>...
Install the given executables into the executable install directory, for the current Python implementation (${EPYTHON}).
The executable will be wrapped properly for the Python implementation, though no shebang mangling will be performed.

python_newexe <path> <new-name>
Install the given executable into the executable install directory, for the current Python implementation (${EPYTHON}).
The executable will be wrapped properly for the Python implementation, though no shebang mangling will be performed. It will be renamed to <new-name>.

python_doscript <files>...
Install the given scripts into the executable install directory, for the current Python implementation (${EPYTHON}).
All specified files must start with a 'python' shebang. The shebang will be converted, and the files will be wrapped properly for the Python implementation.

Example:

src_install() {
  python_foreach_impl python_doscript ${PN}
}
python_newscript <path> <new-name>
Install the given script into the executable install directory for the current Python implementation (${EPYTHON}), and name it <new-name>.
The file must start with a 'python' shebang. The shebang will be converted, and the file will be wrapped properly for the Python implementation. It will be renamed to <new-name>.

Example:

src_install() {
  python_foreach_impl python_newscript foo.py foo
}
python_moduleinto <new-path>
Set the Python module install directory for python_domodule(). The <new-path> can either be an absolute target system path (in which case it needs to start with a slash, and ${ED} will be prepended to it) or relative to the implementation's site-packages directory (then it must not start with a slash). The relative path can be specified either using the Python package notation (separated by dots) or the directory notation (using slashes).
When not set explicitly, the modules are installed to the top site-packages directory.

In the relative case, the exact path is determined directly by each python_doscript/python_newscript function. Therefore, python_moduleinto can be safely called before establishing the Python interpreter and/or a single call can be used to set the path correctly for multiple implementations, as can be seen in the following example.

Example:

src_install() {
  python_moduleinto bar
  # installs ${PYTHON_SITEDIR}/bar/baz.py
  python_foreach_impl python_domodule baz.py
}
python_domodule <files>...
Install the given modules (or packages) into the current Python module installation directory. The list can mention both modules (files) and packages (directories). All listed files will be installed for all enabled implementations, and compiled afterwards.
Example:

src_install() {
  # (${PN} being a directory)
  python_foreach_impl python_domodule ${PN}
}
python_doheader <files>...
Install the given headers into the implementation-specific include directory. This function is unconditionally recursive, i.e. you can pass directories instead of files.
Example:

src_install() {
  python_foreach_impl python_doheader foo.h bar.h
}
python_wrapper_setup [<path> [<impl>]]
Backwards compatibility function. The relevant API is now considered private, please use python_setup instead.
python_is_python3 [<impl>]
Check whether <impl> (or ${EPYTHON}) is a Python3k variant (i.e. uses syntax and stdlib of Python 3.*).
Returns 0 (true) if it is, 1 (false) otherwise.

python_is_installed [<impl>]
Check whether the interpreter for <impl> (or ${EPYTHON}) is installed. Uses has_version with a proper dependency string.
Returns 0 (true) if it is, 1 (false) otherwise.

python_fix_shebang [-f|--force] [-q|--quiet] <path>...
Replace the shebang in Python scripts with the current Python implementation (EPYTHON). If a directory is passed, works recursively on all Python scripts.
Only files having a 'python*' shebang will be modified. Files with other shebang will either be skipped when working recursively on a directory or treated as error when specified explicitly.

Shebangs matching explicitly current Python version will be left unmodified. Shebangs requesting another Python version will be treated as fatal error, unless --force is given.

--force causes the function to replace even shebangs that require incompatible Python version. --quiet causes the function not to list modified files verbosely.

python_export_utf8_locale
Attempts to export a usable UTF-8 locale in the LC_CTYPE variable. Does nothing if LC_ALL is defined, or if the current locale uses a UTF-8 charmap. This may be used to work around the quirky open() behavior of python3.
Return value: 0 on success, 1 on failure.

build_sphinx <directory>
Build HTML documentation using dev-python/sphinx in the specified <directory>. Takes care of disabling Intersphinx and appending to HTML_DOCS.
If <directory> is relative to the current directory, care needs to be taken to run einstalldocs from the same directory (usually ${S}).

ECLASS VARIABLES
PYTHON
The absolute path to the current Python interpreter.
This variable is set automatically in the following contexts:

python-r1: Set in functions called by python_foreach_impl() or after calling python_setup().

python-single-r1: Set after calling python-single-r1_pkg_setup().

distutils-r1: Set within any of the python sub-phase functions.

Example value:

/usr/bin/python2.7
EPYTHON
The executable name of the current Python interpreter.
This variable is set automatically in the following contexts:

python-r1: Set in functions called by python_foreach_impl() or after calling python_setup().

python-single-r1: Set after calling python-single-r1_pkg_setup().

distutils-r1: Set within any of the python sub-phase functions.

Example value:

python2.7
AUTHORS
Author: Michał Górny <mgorny@gentoo.org>
Based on work of: Krzysztof Pawlik <nelchael@gentoo.org>
MAINTAINERS
Python team <python@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
python-utils-r1.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/python-utils-r1.eclass
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
Time: 00:27:03 GMT, October 11, 2020