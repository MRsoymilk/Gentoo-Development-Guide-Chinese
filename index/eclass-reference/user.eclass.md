USER.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
user.eclass - user management in ebuilds
DESCRIPTION
The user eclass contains a suite of functions that allow ebuilds to quickly make sure users in the installed system are sane.
FUNCTIONS
enewuser <user> [-F] [-M] [uid] [shell] [homedir] [groups]
Same as enewgroup, you are not required to understand how to properly add a user to the system. The only required parameter is the username. Default uid is (pass -1 for this) next available, default shell is /bin/false, default homedir is /dev/null, and there are no default groups.
If -F is passed, enewuser will always enforce specified UID and fail if it can not be assigned. If -M is passed, enewuser does not create the home directory if it does not exist.

enewgroup <group> [gid]
This function does not require you to understand how to properly add a group to the system. Just give it a group name to add and enewgroup will do the rest. You may specify the gid for the group or allow the group to allocate the next available one.
If -F is passed, enewgroup will always enforce specified GID and fail if it can not be assigned.

esethome <user> <homedir>
Update the home directory in a platform-agnostic way. Required parameters is the username and the new home directory. Specify -1 if you want to set home to the enewuser default of /dev/null. If the new home directory does not exist, it is created. Any previously existing home directory is NOT moved.
esetshell <user> <shell>
Update the shell in a platform-agnostic way. Required parameters is the username and the new shell. Specify -1 if you want to set shell to platform-specific nologin.
esetcomment <user> <comment>
Update the comment field in a platform-agnostic way. Required parameters is the username and the new comment.
esetgroups <user> <groups>
Update the group field in a platform-agnostic way. Required parameters is the username and the new list of groups, primary group first.
MAINTAINERS
base-system@gentoo.org (Linux)
Michał Górny <mgorny@gentoo.org> (NetBSD)
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
user.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/user.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:01 GMT, October 11, 2020
