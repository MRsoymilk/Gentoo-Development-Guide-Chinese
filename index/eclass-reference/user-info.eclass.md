USER-INFO.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
user-info.eclass - Read-only access to user and group information
FUNCTIONS
egetent <database> <key>
Small wrapper for getent (Linux), nidump (< Mac OS X 10.5), dscl (Mac OS X 10.5), and pw (FreeBSD) used in enewuser()/enewgroup().
Supported databases: group passwd

egetusername <uid>
Gets the username for given UID.
egetgroupname <gid>
Gets the group name for given GID.
egethome <user>
Gets the home directory for the specified user.
egetshell <user>
Gets the shell for the specified user.
egetcomment <user>
Gets the comment field for the specified user.
egetgroups <user>
Gets all the groups user belongs to. The primary group is returned first, then all supplementary groups. Groups are ','-separated.
MAINTAINERS
base-system@gentoo.org (Linux)
Michał Górny <mgorny@gentoo.org> (NetBSD)
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
user-info.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/user-info.eclass
Index
NAME
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:02 GMT, October 11, 2020