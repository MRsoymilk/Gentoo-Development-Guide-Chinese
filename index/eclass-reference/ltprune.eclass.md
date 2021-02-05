LTPRUNE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
ltprune.eclass - Smart .la file pruning
DESCRIPTION
A function to locate and remove unnecessary .la files.
Discouraged. Whenever possible, please use much simpler:

find "${ED}" -name '*.la' -delete || die
SUPPORTED EAPIS
0 1 2 3 4 5 6
FUNCTIONS
prune_libtool_files [--all|--modules]
Locate unnecessary libtool files (.la) and libtool static archives (.a) and remove them from installation image.
By default, .la files are removed whenever the static linkage can either be performed using pkg-config or doesn't introduce additional flags.

If '--modules' argument is passed, .la files for modules (plugins) are removed as well. This is usually useful when the package installs plugins and the plugin loader does not use .la files.

If '--all' argument is passed, all .la files are removed without performing any heuristic on them. You shouldn't ever use that, and instead report a bug in the algorithm instead.

The .a files are only removed whenever corresponding .la files state that they should not be linked to, i.e. whenever these files correspond to plugins.

Note: if your package installs both static libraries and .pc files which use variable substitution for -l flags, you need to add pkg-config to your DEPEND.

MAINTAINERS
Michał Górny <mgorny@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
ltprune.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/ltprune.eclass
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
Time: 00:27:04 GMT, October 11, 2020