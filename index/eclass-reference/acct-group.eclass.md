ACCT-GROUP.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
acct-group.eclass - Eclass used to create and maintain a single group entry
DESCRIPTION
This eclass represents and creates a single group entry. The name of the group is derived from ${PN}, while (preferred) GID needs to be specified via ACCT_GROUP_ID. Packages (and users) needing the group in question should depend on the package providing it.
Example: If your package needs group 'foo', you create 'acct-group/foo' package and add an ebuild with the following contents:

EAPI=7
inherit acct-group
ACCT_GROUP_ID=200
Then you add appropriate dependency to your package. The dependency type(s) should be: - DEPEND (+ RDEPEND) if the group is already needed at build time, - RDEPEND if it is needed at install time (e.g. you 'fowners' files
  in pkg_preinst) or run time.

SUPPORTED EAPIS
7
FUNCTIONS
acct-group_pkg_pretend
Performs sanity checks for correct eclass usage, and early-checks whether requested GID can be enforced.
acct-group_src_install
Installs sysusers.d file for the group.
acct-group_pkg_preinst
Creates the group if it does not exist yet.
ECLASS VARIABLES
ACCT_GROUP_ID (REQUIRED)
Preferred GID for the new group. This variable is obligatory, and its value must be unique across all group packages.
Overlays should set this to -1 to dynamically allocate GID. Using -1 in ::gentoo is prohibited by policy.

ACCT_GROUP_ENFORCE_ID
If set to a non-null value, the eclass will require the group to have specified GID. If the group already exists with another GID, or the GID is taken by another group, the install will fail.
AUTHORS
Michael Orlitzky <mjo@gentoo.org>
Michał Górny <mgorny@gentoo.org>
MAINTAINERS
Michał Górny <mgorny@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
acct-group.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/acct-group.eclass
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
Time: 12:27:03 GMT, October 10, 2020
