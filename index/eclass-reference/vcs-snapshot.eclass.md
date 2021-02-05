VCS-SNAPSHOT.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
vcs-snapshot.eclass - support eclass for unpacking VCS snapshot tarballs
DESCRIPTION
THIS ECLASS IS NOT NECESSARY FOR MODERN GITHUB AND GITLAB SNAPSHOTS. THEIR DIRECTORY STRUCTURE IS ENTIRELY PREDICTABLE, SO UPDATE YOUR EBUILD TO USE /ARCHIVE/ URI AND SET S IF NECESSARY.
This eclass provides a convenience src_unpack() which does unpack all the tarballs in SRC_URI to locations matching their (local) names, discarding the original parent directory.

The typical use case are VCS tag snapshots coming from BitBucket (but not GitHub or GitLab). They have hash appended to the directory name which makes extracting them a painful experience. But if you are using a SRC_URI arrow to rename them (which quite likely you have to do anyway), vcs-snapshot will just extract them into matching directories.

Please note that this eclass handles only tarballs (.tar, .tar.gz, .tar.bz2 & .tar.xz). For any other file format (or suffix) it will fall back to regular unpack. Support for additional formats may be added in the future if necessary.

SUPPORTED EAPIS
0 1 2 3 4 5 6 7
EXAMPLE
EAPI=7
inherit vcs-snapshot

SRC_URI="
   https://bitbucket.org/foo/bar/get/${PV}.tar.bz2 -> ${P}.tar.bz2
   https://bitbucket.org/foo/bar-otherstuff/get/${PV}.tar.bz2
       -> ${P}-otherstuff.tar.bz2"
and however the tarballs were originally packed, all files will appear in ${WORKDIR}/${P} and ${WORKDIR}/${P}-otherstuff respectively.

FUNCTIONS
vcs-snapshot_src_unpack
Extract all the archives from ${A}. The .tar, .tar.gz, .tar.bz2 and .tar.xz archives will be unpacked to directories matching their local names. Other archive types will be passed down to regular unpack.
MAINTAINERS
mgorny@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
vcs-snapshot.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/vcs-snapshot.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
EXAMPLE
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020