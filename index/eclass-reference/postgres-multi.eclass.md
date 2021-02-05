POSTGRES-MULTI.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
postgres-multi.eclass - An eclass to build PostgreSQL-related packages against multiple slots
DESCRIPTION
postgres-multi enables ebuilds, particularly PostgreSQL extensions, to build and install for one or more PostgreSQL slots as specified by POSTGRES_TARGETS use flags.
SUPPORTED EAPIS
5 6 7
FUNCTIONS
postgres-multi_foreach <command> [arg ...]
Run the given command in the package's build directory for each PostgreSQL slot in the intersect of POSTGRES_TARGETS and POSTGRES_COMPAT. The PG_CONFIG and PKG_CONFIG_PATH environment variables are updated on each iteration to point to the matching pg_config command and pkg-config metadata files, respectively, for the current slot. Any appearance of @PG_SLOT@ in the command or arguments will be substituted with the slot (e.g., 9.5) of the current iteration.
postgres-multi_forbest <command> [arg ...]
Run the given command in the package's build directory for the highest slot in the intersect of POSTGRES_COMPAT and POSTGRES_TARGETS. The PG_CONFIG and PKG_CONFIG_PATH environment variables are set to the matching pg_config command and pkg-config metadata files, respectively. Any appearance of @PG_SLOT@ in the command or arguments will be substituted with the matching slot (e.g., 9.5).
postgres-multi_pkg_setup
Initialize internal environment variable(s). This is required if pkg_setup() is declared in the ebuild.
postgres-multi_src_prepare
Calls eapply_user then copies ${S} into a build directory for each intersect of POSTGRES_TARGETS and POSTGRES_COMPAT.
postgres-multi_src_compile
Runs `emake' in each build directory
postgres-multi_src_install
Runs `emake install DESTDIR="${D}"' in each build directory.
postgres-multi_src_test
Runs `emake installcheck' in each build directory.
ECLASS VARIABLES
POSTGRES_COMPAT (REQUIRED)
A Bash array containing a list of compatible PostgreSQL slots as defined by the developer. Must be declared before inheriting this eclass. Example:
POSTGRES_COMPAT=( 9.2 9.3 9.4 9.5 9.6 10 )
POSTGRES_COMPAT=( 9.{2,3} 9.{4..6} 10 ) # Same as previous
MAINTAINERS
PostgreSQL <pgsql-bugs@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
postgres-multi.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/postgres-multi.eclass
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