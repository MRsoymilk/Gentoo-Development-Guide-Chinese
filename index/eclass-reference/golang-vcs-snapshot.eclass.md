GOLANG-VCS-SNAPSHOT.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
golang-vcs-snapshot.eclass - support eclass for unpacking VCS snapshot tarballs for
SUPPORTED EAPIS
5 6 7
FUNCTIONS
golang-vcs-snapshot_src_unpack
Extract the first archive from ${A} to the appropriate location for GOPATH.
ECLASS VARIABLES
EGO_VENDOR
This variable contains a list of vendored packages. The items of this array are strings that contain the import path and the git commit hash for a vendored package. If the import path does not start with github.com, the third argument can be used to point to a github repository.
MAINTAINERS
William Hubbs <williamh@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
golang-vcs-snapshot.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/golang-vcs-snapshot.eclass
Index
NAME
SUPPORTED EAPIS
FUNCTIONS
ECLASS VARIABLES
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020