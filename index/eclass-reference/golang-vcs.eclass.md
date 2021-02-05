GOLANG-VCS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
golang-vcs.eclass - Eclass for fetching and unpacking go repositories.
DESCRIPTION
This eclass is written to ease the maintenance of live ebuilds of software written in the Go programming language.
SUPPORTED EAPIS
5 6 7
ECLASS VARIABLES
EGO_PN (REQUIRED)
This is the import path for the go package(s). Please emerge dev-lang/go and read "go help importpath" for syntax.
Example:

EGO_PN="github.com/user/package"
EGO_PN="github.com/user1/package1 github.com/user2/package2"
EGO_STORE_DIR
Storage directory for Go sources.
This is intended to be set by the user in make.conf. Ebuilds must not set it.

EGO_STORE_DIR=${DISTDIR}/go-src

EVCS_OFFLINE
If non-empty, this variable prevents any online operations.
EVCS_UMASK
Set this variable to a custom umask. This is intended to be set by users. By setting this to something like 002, it can make life easier for people who do development as non-root (but are in the portage group) and use FEATURES=userpriv.
MAINTAINERS
William Hubbs <williamh@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
golang-vcs.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/golang-vcs.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
ECLASS VARIABLES
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:05 GMT, October 11, 2020