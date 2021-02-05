EUTILS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
eutils.eclass - many extra (but common) functions that are used in ebuilds
DESCRIPTION
The eutils eclass contains a suite of functions that complement the ones that ebuild.sh already contain. The idea is that the functions are not required in all ebuilds but enough utilize them to have a common home rather than having multiple ebuilds implementing the same thing.
Due to the nature of this eclass, some functions may have maintainers different from the overall eclass!

This eclass is DEPRECATED and must not be inherited by any new ebuilds or eclasses. Use the more specific split eclasses instead, or native package manager functions when available.

SUPPORTED EAPIS
0 1 2 3 4 5 6 7
FUNCTIONS
emktemp [temp dir]
Cheap replacement for when coreutils (and thus mktemp) does not exist on the user's system.
use_if_iuse <flag>
Return true if the given flag is in USE and IUSE.
Note that this function should not be used in the global scope.

usex <USE flag> [true output] [false output] [true suffix] [false suffix]
Proxy to declare usex for package managers or EAPIs that do not provide it and use the package manager implementation when available (i.e. EAPI >= 5). If USE flag is set, echo [true output][true suffix] (defaults to "yes"), otherwise echo [false output][false suffix] (defaults to "no").
einstalldocs
Install documentation using DOCS and HTML_DOCS, in EAPIs that do not provide this function. When available (i.e., in EAPI 6 or later), the package manager implementation should be used instead.
If DOCS is declared and non-empty, all files listed in it are installed. The files must exist, otherwise the function will fail. In EAPI 4 and 5, DOCS may specify directories as well; in earlier EAPIs using directories is unsupported.

If DOCS is not declared, the files matching patterns given in the default EAPI implementation of src_install will be installed. If this is undesired, DOCS can be set to empty value to prevent any documentation from being installed.

If HTML_DOCS is declared and non-empty, all files and/or directories listed in it are installed as HTML docs (using dohtml).

Both DOCS and HTML_DOCS can either be an array or a whitespace- separated list. Whenever directories are allowed, '<directory>/.' may be specified in order to install all files within the directory without creating a sub-directory in docdir.

Passing additional options to dodoc and dohtml is not supported. If you needed such a thing, you need to call those helpers explicitly.

in_iuse <flag>
Determines whether the given flag is in IUSE. Strips IUSE default prefixes as necessary. In EAPIs where it is available (i.e., EAPI 6 or later), the package manager implementation should be used instead.
Note that this function must not be used in the global scope.

eqawarn [message]
Proxy to ewarn for package managers that don't provide eqawarn and use the PM implementation if available. Reuses PORTAGE_ELOG_CLASSES as set by the dev profile.
MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
eutils.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/eutils.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:01 GMT, October 11, 2020
