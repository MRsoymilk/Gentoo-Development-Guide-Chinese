QMAIL.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
qmail.eclass - common qmail functions
FUNCTIONS
primes <min> <max>
Prints a list of primes between min and max inclusive Note: this functions gets very slow when used with large numbers.
is_prima <number>
Checks wether a number is a prime number
dosupervise <service> [<runfile> <logfile>]
Install runfiles for services and logging to supervise directory
qmail_set_cc
The following commands patch the conf-{cc,ld} files to use the user's specified CFLAGS and LDFLAGS. These rather complex commands are needed because a user supplied patch might apply changes to these files, too. See bug #165981.
qmail_src_postunpack
Unpack common config files, and set built configuration (CFLAGS, LDFLAGS, etc)
MAINTAINERS
qmail-bugs@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
qmail.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/qmail.eclass
Index
NAME
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:01 GMT, October 11, 2020
