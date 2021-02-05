GOLANG-BASE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
golang-base.eclass - Eclass that provides base functions for Go packages.
DESCRIPTION
This eclass provides base functions for software written in the Go programming language; it also provides the build-time dependency on dev-lang/go.
SUPPORTED EAPIS
5 6 7
FUNCTIONS
ego_pn_check
Make sure EGO_PN has a value.
get_golibdir
Return the non-prefixed library directory where Go packages should be installed
get_golibdir_gopath
Return the library directory where Go packages should be installed This is the prefixed version which should be included in GOPATH
golang_install_pkgs
Install Go packages. This function assumes that $cwd is a Go workspace.
ECLASS VARIABLES
EGO_PN (REQUIRED)
This is the import path for the go package to build. Please emerge dev-lang/go and read "go help importpath" for syntax.
Example:

EGO_PN=github.com/user/package
MAINTAINERS
William Hubbs <williamh@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
golang-base.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/golang-base.eclass
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
Time: 00:27:03 GMT, October 11, 2020
