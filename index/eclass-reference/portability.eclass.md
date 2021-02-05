PORTABILITY.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
portability.eclass - This eclass is created to avoid using non-portable GNUisms inside ebuilds
FUNCTIONS
treecopy <orig1> [orig2 orig3 ....] <dest>
mimic cp --parents copy, but working on BSD userland as well
seq [min] <max> [step]
compatibility function that mimes seq command if not available
Return value: sequence from min to max regardless of seq command being present on system

dlopen_lib
Gets the linker flag to link to dlopen() function
Return value: linker flag if needed

get_bmake
Gets the name of the BSD-ish make command (bmake from NetBSD)
This will return make (provided by system packages) for BSD userlands, or bsdmake for Darwin userlands and pmake for the rest of userlands, both of which are provided by sys-devel/pmake package.

Note: the bsdmake for Darwin userland is with compatibility with MacOSX default name.

Return value: system version of make

get_mounts
Portable method of getting mount names and points. Returns as "point node fs options" Remember to convert 040 back to a space.
Return value: table of mounts in form "point node fs opts"

AUTHORS
Diego Pettenò <flameeyes@gentoo.org>
MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
portability.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/portability.eclass
Index
NAME
FUNCTIONS
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:01 GMT, October 11, 2020
