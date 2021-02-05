NINJA-UTILS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
ninja-utils.eclass - common bits to run dev-util/ninja builder
DESCRIPTION
This eclass provides a single function -- eninja -- that can be used to run the ninja builder alike emake. It does not define any dependencies, you need to depend on dev-util/ninja yourself. Since ninja is rarely used stand-alone, most of the time this eclass will be used indirectly by the eclasses for other build systems (CMake, Meson).
SUPPORTED EAPIS
2 4 5 6 7
FUNCTIONS
eninja [<args>...]
Call Ninja, passing the NINJAOPTS (or converted MAKEOPTS), followed by the supplied arguments. This function dies if ninja fails. Starting with EAPI 6, it also supports being called via 'nonfatal'.
ECLASS VARIABLES
NINJAOPTS
The default set of options to pass to Ninja. Similar to MAKEOPTS, supposed to be set in make.conf. If unset, eninja() will convert MAKEOPTS instead.
AUTHORS
Michał Górny <mgorny@gentoo.org>
Mike Gilbert <floppym@gentoo.org>
MAINTAINERS
Michał Górny <mgorny@gentoo.org>
Mike Gilbert <floppym@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
ninja-utils.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/ninja-utils.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020