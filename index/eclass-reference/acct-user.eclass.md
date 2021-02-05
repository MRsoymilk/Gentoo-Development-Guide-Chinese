ACCT-USER.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
acct-user.eclass - Eclass used to create and maintain a single user entry
DESCRIPTION
This eclass represents and creates a single user entry. The name of the user is derived from ${PN}, while (preferred) UID needs to be specified via ACCT_USER_ID. Additional variables are provided to override the default home directory, shell and add group membership. Packages needing the user in question should depend on the package providing it.
The ebuild needs to call acct-user_add_deps after specifying ACCT_USER_GROUPS.

Example: If your package needs user 'foo' belonging to same-named group, you create 'acct-user/foo' package and add an ebuild with the following contents:

EAPI=7
inherit acct-user
ACCT_USER_ID=200
ACCT_USER_GROUPS=( foo )
acct-user_add_deps
Then you add appropriate dependency to your package. The dependency type(s) should be: - DEPEND (+ RDEPEND) if the user is already needed at build time, - RDEPEND if it is needed at install time (e.g. you 'fowners' files
  in pkg_preinst) or run time.

SUPPORTED EAPIS
7
FUNCTIONS
acct-user_add_deps
Generate appropriate RDEPEND from ACCT_USER_GROUPS. This must be called if ACCT_USER_GROUPS are set.
acct-user_pkg_pretend
Performs sanity checks for correct eclass usage, and early-checks whether requested UID can be enforced.
acct-user_src_install
Installs a keep-file into the user's home directory to ensure it is owned by the package, and sysusers.d file.
acct-user_pkg_preinst
Creates the user if it does not exist yet. Sets permissions of the home directory in install image.
acct-user_pkg_postinst
Updates user properties if necessary. This needs to be done after new home directory is installed.
acct-user_pkg_prerm
Ensures that the user account is locked out when it is removed.
ECLASS VARIABLES
ACCT_USER_ID (REQUIRED)
Preferred UID for the new user. This variable is obligatory, and its value must be unique across all user packages.
Overlays should set this to -1 to dynamically allocate UID. Using -1 in ::gentoo is prohibited by policy.

ACCT_USER_ENFORCE_ID
If set to a non-null value, the eclass will require the user to have specified UID. If the user already exists with another UID, or the UID is taken by another user, the install will fail.
ACCT_USER_SHELL ?= -1
The shell to use for the user. If not specified, a 'nologin' variant for the system is used.
ACCT_USER_HOME ?= /dev/null
The home directory for the user. If not specified, /dev/null is used. The directory will be created with appropriate permissions if it does not exist. When updating, existing home directory will not be moved.
ACCT_USER_HOME_OWNER
The ownership to use for the home directory, in chown ([user][:group]) syntax. Defaults to the newly created user, and its primary group.
ACCT_USER_HOME_PERMS ?= 0755
The permissions to use for the home directory, in chmod (octal or verbose) form.
ACCT_USER_GROUPS (REQUIRED)
List of groups the user should belong to. This must be a bash array. The first group specified is the user's primary group, while the remaining groups (if any) become supplementary groups.
AUTHORS
Michael Orlitzky <mjo@gentoo.org>
Michał Górny <mgorny@gentoo.org>
MAINTAINERS
Michał Górny <mgorny@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
acct-user.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/acct-user.eclass
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
Time: 12:27:05 GMT, October 10, 2020