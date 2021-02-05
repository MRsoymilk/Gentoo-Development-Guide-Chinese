README.GENTOO-R1.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
readme.gentoo-r1.eclass - install a doc file shown via elog messages
DESCRIPTION
An eclass for installing a README.gentoo doc file recording tips shown via elog messages. With this eclass, those elog messages will only be shown at first package installation and a file for later reviewing will be installed under /usr/share/doc/${PF}
You need to call readme.gentoo_create_doc in src_install phase and readme.gentoo_print_elog in pkg_postinst

SUPPORTED EAPIS
4 5 6 7
FUNCTIONS
readme.gentoo_create_doc
Create doc file with ${DOC_CONTENTS} variable (preferred) and, if not set, look for "${FILESDIR}/README.gentoo" contents. You can use ${FILESDIR}/README.gentoo-${SLOT} also. Usually called at src_install phase.
readme.gentoo_print_elog
Print elog messages with "${T}"/README.gentoo contents. They will be shown only when package is installed at first time. Usually called at pkg_postinst phase.
If you want to show them always, please set FORCE_PRINT_ELOG to a non empty value in your ebuild before this function is called. This can be useful when, for example, DOC_CONTENTS is modified, then, you can rely on specific REPLACING_VERSIONS handling in your ebuild to print messages when people update from versions still providing old message.

ECLASS VARIABLES
DISABLE_AUTOFORMATTING
If non-empty, DOC_CONTENTS information will be strictly respected, not getting it automatically formatted by fmt. If empty, it will rely on fmt for formatting and 'echo -e' options to tweak lines a bit.
FORCE_PRINT_ELOG
If non-empty this variable forces elog messages to be printed.
README_GENTOO_SUFFIX ?= ""
If you want to specify a suffix for README.gentoo file please export it.
AUTHORS
Author: Pacho Ramos <pacho@gentoo.org>
MAINTAINERS
Pacho Ramos <pacho@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
readme.gentoo-r1.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/readme.gentoo-r1.eclass
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
Time: 00:27:01 GMT, October 11, 2020