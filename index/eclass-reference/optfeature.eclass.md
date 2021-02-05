OPTFEATURE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
optfeature.eclass - Advertise optional functionality that might be useful to users
FUNCTIONS
optfeature <short description> <package atom to match> [other atoms]
Print out a message suggesting an optional package (or packages) not currently installed which provides the described functionality.
The following snippet would suggest app-misc/foo for optional foo support, app-misc/bar or app-misc/baz[bar] for optional bar support and either both app-misc/a and app-misc/b or app-misc/c for alphabet support.

optfeature "foo support" app-misc/foo
optfeature "bar support" app-misc/bar app-misc/baz[bar]
optfeature "alphabet support" "app-misc/a app-misc/b" app-misc/c
MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
optfeature.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/optfeature.eclass
Index
NAME
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020