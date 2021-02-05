CANNADIC.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
cannadic.eclass - Function for Canna compatible dictionaries
DESCRIPTION
The cannadic eclass is used for installation and setup of Canna compatible dictionaries within the Portage system.
FUNCTIONS
cannadic_pkg_setup
Sets up ${CANNADIC_CANNA_DIR}
cannadic-install
Installs dictionaries to ${CANNADIC_CANNA_DIR}
dicsdir-install
Installs dics.dir from ${DICSDIRFILE}
cannadic_src_install
Installs all dictionaries under ${WORKDIR} plus dics.dir and docs
update-cannadic-dir
Updates dics.dir for Canna Server, script for this part taken from Debian GNU/Linux

 compiles dics.dir files for Canna Server
 Copyright 2001 ISHIKAWA Mutsumi
 Licensed under the GNU General Public License, version 2.  See the file
 /usr/portage/license/GPL-2 or <http://www.gnu.org/copyleft/gpl.txt>.

cannadic_pkg_postinst
Updates dics.dir and print out notice after install
cannadic_pkg_postrm
Updates dics.dir and print out notice after uninstall
AUTHORS
Mamoru KOMACHI <usata@gentoo.org>
MAINTAINERS
cjk@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
cannadic.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/cannadic.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:02 GMT, October 11, 2020