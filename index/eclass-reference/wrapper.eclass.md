WRAPPER.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
wrapper.eclass - create a shell wrapper script
FUNCTIONS
make_wrapper <wrapper> <target> [chdir] [libpaths] [installpath]
Create a shell wrapper script named wrapper in installpath (defaults to the bindir) to execute target (default of wrapper) by first optionally setting LD_LIBRARY_PATH to the colon-delimited libpaths followed by optionally changing directory to chdir.
MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
wrapper.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/wrapper.eclass
Index
NAME
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020
