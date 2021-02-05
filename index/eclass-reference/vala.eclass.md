VALA.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
vala.eclass - Sets up the environment for using a specific version of vala.
DESCRIPTION
This eclass sets up commonly used environment variables for using a specific version of dev-lang/vala to configure and build a package. It is needed for packages whose build systems assume the existence of certain unversioned vala executables, pkgconfig files, etc., which Gentoo does not provide.
This eclass provides one phase function: src_prepare.

SUPPORTED EAPIS
1 2 3 4 5 6 7
FUNCTIONS
vala_api_versions
Outputs a list of vala API versions from VALA_MAX_API_VERSION down to VALA_MIN_API_VERSION.
vala_depend
Outputs a ||-dependency string on vala from VALA_MAX_API_VERSION down to VALA_MIN_API_VERSION
vala_best_api_version
Returns the highest installed vala API version satisfying VALA_MAX_API_VERSION, VALA_MIN_API_VERSION, and VALA_USE_DEPEND.
vala_src_prepare [--ignore-use] [--vala-api-version api_version]
Sets up the environment variables and pkgconfig files for the specified API version, or, if no version is specified, for the highest installed vala API version satisfying VALA_MAX_API_VERSION, VALA_MIN_API_VERSION, and VALA_USE_DEPEND. Is a no-op if called without --ignore-use when USE=-vala. Dies if the USE check is passed (or ignored) and a suitable vala version is not available.
ECLASS VARIABLES
VALA_MIN_API_VERSION = ${VALA_MIN_API_VERSION:-0.36}
Minimum vala API version (e.g. 0.36).
VALA_MAX_API_VERSION = ${VALA_MAX_API_VERSION:-0.50}
Maximum vala API version (e.g. 0.36).
VALA_USE_DEPEND
USE dependencies that vala must be built with (e.g. vapigen).
AUTHORS
Alexandre Rostovtsev <tetromino@gentoo.org>
MAINTAINERS
gnome@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
vala.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/vala.eclass
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