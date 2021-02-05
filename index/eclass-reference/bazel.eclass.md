BAZEL.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
bazel.eclass - Utility functions for packages using Bazel Build
DESCRIPTION
A utility eclass providing functions to run the Bazel Build system.
This eclass does not export any phase functions.

SUPPORTED EAPIS
7
FUNCTIONS
bazel_get_flags
Obtain and print the bazel flags for target and host *FLAGS.
To add more flags to this, append the flags to the appropriate variable before calling this function

bazel_setup_bazelrc
Creates the bazelrc with common options that will be passed to bazel. This will be called by ebazel automatically so does not need to be called from the ebuild.
ebazel [<args>...]
Run bazel with the bazelrc and output_base.
output_base will be specific to $BUILD_DIR (if unset, $S). bazel_setup_bazelrc will be called and the created bazelrc will be passed to bazel.

Will automatically die if bazel does not exit cleanly.

bazel_load_distfiles <distfiles>...
Populate the bazel distdir to fetch from since it cannot use the network. Bazel looks in distdir but will only look for the original filename, not the possibly renamed one that portage downloaded. If the line has -> we to rename it back. This also handles use-conditionals that SRC_URI does.
Example:

bazel_external_uris="http://a/file-2.0.tgz
    python? ( http://b/1.0.tgz -> foo-1.0.tgz )"
SRC_URI="http://c/${PV}.tgz
    ${bazel_external_uris}"

src_unpack() {
    unpack ${PV}.tgz
    bazel_load_distfiles "${bazel_external_uris}"
}
AUTHORS
Jason Zaman <perfinion@gentoo.org>
MAINTAINERS
Jason Zaman <perfinion@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
bazel.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/bazel.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020