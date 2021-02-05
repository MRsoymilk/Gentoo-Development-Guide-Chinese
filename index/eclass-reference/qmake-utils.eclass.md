QMAKE-UTILS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
qmake-utils.eclass - Common functions for qmake-based packages.
DESCRIPTION
Utility eclass providing wrapper functions for Qt4 and Qt5 qmake.
This eclass does not set any metadata variables nor export any phase functions. It can be inherited safely.

SUPPORTED EAPIS
6 7
FUNCTIONS
qt4_get_bindir
Echoes the directory where Qt4 binaries are installed. EPREFIX is already prepended to the returned path.
qt4_get_headerdir
Echoes the directory where Qt4 headers are installed.
qt4_get_libdir
Echoes the directory where Qt4 libraries are installed.
qt4_get_mkspecsdir
Echoes the directory where Qt4 mkspecs are installed.
qt4_get_plugindir
Echoes the directory where Qt4 plugins are installed.
qt5_get_bindir
Echoes the directory where Qt5 binaries are installed. EPREFIX is already prepended to the returned path.
qt5_get_headerdir
Echoes the directory where Qt5 headers are installed.
qt5_get_libdir
Echoes the directory where Qt5 libraries are installed.
qt5_get_mkspecsdir
Echoes the directory where Qt5 mkspecs are installed.
qt5_get_plugindir
Echoes the directory where Qt5 plugins are installed.
EQMAKE4_EXCLUDE
List of files to be excluded from eqmake4 CONFIG processing. Paths are relative to the current working directory (usually ${S}).
Example: EQMAKE4_EXCLUDE="ignore/me.pro foo/*"

eqmake4 [project_file] [parameters to qmake]
Wrapper for Qt4's qmake. If project_file is not specified, eqmake4 looks for one in the current directory (non-recursively). If multiple project files are found, then ${PN}.pro is used, if it exists, otherwise eqmake4 will not be able to continue.
All other arguments are appended unmodified to qmake command line.

For recursive build systems, i.e. those based on the subdirs template, you should run eqmake4 on the top-level project file only, unless you have a valid reason to do otherwise. During the building, qmake will be automatically re-invoked with the right arguments on every directory specified inside the top-level project file.

eqmake5 [arguments for qmake]
Wrapper for Qt5's qmake. All arguments are passed to qmake.
For recursive build systems, i.e. those based on the subdirs template, you should run eqmake5 on the top-level project file only, unless you have a valid reason to do otherwise. During the building, qmake will be automatically re-invoked with the right arguments on every directory specified inside the top-level project file.

AUTHORS
Davide Pesavento <pesa@gentoo.org>
MAINTAINERS
qt@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
qmake-utils.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/qmake-utils.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:01 GMT, October 11, 2020