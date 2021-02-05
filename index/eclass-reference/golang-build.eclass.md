GOLANG-BUILD.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
golang-build.eclass - Eclass for compiling go packages.
DESCRIPTION
This eclass provides default src_compile, src_test and src_install functions for software written in the Go programming language.
SUPPORTED EAPIS
5 6 7
ECLASS VARIABLES
EGO_BUILD_FLAGS
This allows you to pass build flags to the Go compiler. These flags are common to the "go build" and "go install" commands used below. Please emerge dev-lang/go and run "go help build" for the documentation for these flags.
Example:

EGO_BUILD_FLAGS="-ldflags 
EGO_PN (REQUIRED)
This is the import path for the go package(s) to build. Please emerge dev-lang/go and read "go help importpath" for syntax.
Example:

EGO_PN=github.com/user/package
MAINTAINERS
William Hubbs <williamh@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
golang-build.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/golang-build.eclass
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
Time: 00:27:03 GMT, October 11, 2020