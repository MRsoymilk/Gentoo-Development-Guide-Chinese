DOTNET.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
dotnet.eclass - common settings and functions for mono and dotnet related packages
DESCRIPTION
The dotnet eclass contains common environment settings that are useful for dotnet packages. Currently, it provides no functions, just exports MONO_SHARED_DIR and sets LC_ALL in order to prevent errors during compilation of dotnet packages.
SUPPORTED EAPIS
1 2 3 4 5 6 7
FUNCTIONS
dotnet_pkg_setup
This function set FRAMEWORK.
exbuild
Run xbuild with Release configuration and configurated FRAMEWORK.
egacinstall
Install package to GAC.
dotnet_multilib_comply
multilib comply
ECLASS VARIABLES
USE_DOTNET
Use flags added to IUSE
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
dotnet.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/dotnet.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
ECLASS VARIABLES
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020
