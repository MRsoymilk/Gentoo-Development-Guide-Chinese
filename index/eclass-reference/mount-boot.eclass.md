MOUNT-BOOT.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
mount-boot.eclass - functions for packages that install files into /boot
DESCRIPTION
This eclass is really only useful for bootloaders.
If the live system has a separate /boot partition configured, then this function tries to ensure that it's mounted in rw mode, exiting with an error if it can't. It does nothing if /boot isn't a separate partition.

MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
mount-boot.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/mount-boot.eclass
Index
NAME
DESCRIPTION
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020
