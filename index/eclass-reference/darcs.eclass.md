DARCS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
darcs.eclass - This eclass provides functions for fetch and unpack darcs repositories
DESCRIPTION
This eclass provides the generic darcs fetching functions.
Define the EDARCS_REPOSITORY variable at least. The ${S} variable is set to ${WORKDIR}/${P}.

FUNCTIONS
darcs_patchcount
Internal function to determine amount of patches in repository.
darcs_fetch
Internal function is called from darcs_src_unpack
darcs_src_unpack
src_upack function
ECLASS VARIABLES
EDARCS_DARCS_CMD ?= darcs
Path to darcs binary.
EDARCS_GET_CMD ?= "get --lazy"
First fetch darcs command.
EDARCS_UPDATE_CMD ?= pull
Repo update darcs command.
EDARCS_OPTIONS ?= --set-scripts-executable
Options to pass to both the "get" and "update" commands
EDARCS_TOP_DIR ?= ${PORTAGE_ACTUAL_DISTDIR-${DISTDIR}}/darcs-src
Where the darcs repositories are stored/accessed
EDARCS_REPOSITORY
The URI to the repository.
EDARCS_OFFLINE ?= ${EVCS_OFFLINE}
Set this variable to a non-empty value to disable the automatic updating of a darcs repository. This is intended to be set outside the darcs source tree by users. Defaults to EVCS_OFFLINE value.
EDARCS_CLEAN
Set this to something to get a clean copy when updating (removes the working directory, then uses EDARCS_GET_CMD to re-download it.)
AUTHORS
Original Author: Jeffrey Yasskin <jyasskin@mail.utexas.edu>

              <rphillips@gentoo.org> (tla eclass author)
Andres Loeh <kosmikus@gentoo.org> (darcs.eclass author)
Alexander Vershilov <alexander.vershilov@gmail.com> (various contributions)
MAINTAINERS
"Gentoo's Haskell Language team" <haskell@gentoo.org>
Sergei Trofimovich <slyfox@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
darcs.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/darcs.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020