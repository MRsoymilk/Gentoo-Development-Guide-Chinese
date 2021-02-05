VCS-CLEAN.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
vcs-clean.eclass - helper functions to remove VCS directories
FUNCTIONS
ecvs_clean [list of dirs]
Remove CVS directories and .cvs* files recursively. Useful when a source tarball contains internal CVS directories. Defaults to ${PWD}.
esvn_clean [list of dirs]
Remove .svn directories recursively. Useful when a source tarball contains internal Subversion directories. Defaults to ${PWD}.
egit_clean [list of dirs]
Remove .git* directories recursively. Useful when a source tarball contains internal Git directories. Defaults to ${PWD}.
AUTHORS
Benedikt Böhm <hollow@gentoo.org>
MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
vcs-clean.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/vcs-clean.eclass
Index
NAME
FUNCTIONS
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020
