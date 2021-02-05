POSTGRES.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
postgres.eclass - An eclass for PostgreSQL-related packages
DESCRIPTION
This eclass provides common utility functions that many PostgreSQL-related packages perform, such as checking that the currently selected PostgreSQL slot is within a range, adding a system user to the postgres system group, and generating dependencies.
SUPPORTED EAPIS
5 6 7
FUNCTIONS
postgres_check_slot
Verify that the currently selected PostgreSQL slot is set to one of the slots defined in POSTGRES_COMPAT. Automatically dies unless a POSTGRES_COMPAT slot is selected. Should be called in pkg_pretend().
postgres_new_user [user [(uid|-1) [(shell|-1) [(homedir|-1) [groups]]]]]
Creates the "postgres" system group and user -- which is separate from the database user -- and, optionally, the developer defined user. There are no required parameters.
When given a user to create, it'll be created with the next available uid, default shell set to /bin/false, default homedir is /dev/null, and added to the "postgres" system group. You can use "-1" to skip any parameter except user or groups.

postgres_pkg_setup
Initialize environment variable(s) according to the best installed version of PostgreSQL that is also in POSTGRES_COMPAT. This is required if pkg_setup() is declared in the ebuild. Exports PG_SLOT, PG_CONFIG, and PKG_CONFIG_PATH.
ECLASS VARIABLES
POSTGRES_COMPAT
A Bash array containing a list of compatible PostgreSQL slots as defined by the developer. If declared, must be declared before inheriting this eclass. Example:
POSTGRES_COMPAT=( 9.2 9.3 9.4 9.5 9.6 10 )
POSTGRES_COMPAT=( 9.{2,3} 9.{4..6} 10 ) # Same as previous
POSTGRES_DEP = "dev-db/postgresql:="
An automatically generated dependency string suitable for use in DEPEND and RDEPEND declarations.
POSTGRES_USEDEP
Add the 2-Style and/or 4-Style use dependencies without brackets to be used for POSTGRES_DEP. If declared, must be declared before inheriting this eclass.
POSTGRES_REQ_USE
An automatically generated REQUIRED_USE-compatible string built upon POSTGRES_COMPAT. REQUIRED_USE="... ${POSTGRES_REQ_USE}" is only required if the package must build against one of the PostgreSQL slots declared in POSTGRES_COMPAT.
MAINTAINERS
PostgreSQL <pgsql-bugs@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
postgres.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/postgres.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
ECLASS VARIABLES
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:01 GMT, October 11, 2020
