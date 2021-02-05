BASH-COMPLETION-R1.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
bash-completion-r1.eclass - A few quick functions to install bash-completion files
SUPPORTED EAPIS
0 1 2 3 4 5 6 7
EXAMPLE
EAPI=5

src_configure() {
        econf \
        --with-bash-completion-dir="$(get_bashcompdir)"
}

src_install() {
        default

        newbashcomp contrib/${PN}.bash-completion ${PN}
}
FUNCTIONS
get_bashcompdir
Get the bash-completion completions directory.
dobashcomp <file> [...]
Install bash-completion files passed as args. Has EAPI-dependant failure behavior (like doins).
newbashcomp <file> <newname>
Install bash-completion file under a new name. Has EAPI-dependant failure behavior (like newins).
bashcomp_alias <basename> <alias>...
Alias <basename> completion to one or more commands (<alias>es).
MAINTAINERS
mgorny@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
bash-completion-r1.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/bash-completion-r1.eclass
Index
NAME
SUPPORTED EAPIS
EXAMPLE
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020