ALTERNATIVES.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
alternatives.eclass - Creates symlink to the latest version of multiple slotted packages.
DESCRIPTION
When a package is SLOT'ed, very often we need to have a symlink to the latest version. However, depending on the order the user has merged them, more often than not, the symlink maybe clobbered by the older versions.
This eclass provides a convenience function that needs to be given a list of alternatives (descending order of recent-ness) and the symlink. It will choose the latest version it can find installed and create the desired symlink.

There are two ways to use this eclass. First is by declaring two variables $SOURCE and $ALTERNATIVES where $SOURCE is the symlink to be created and $ALTERNATIVES is a list of alternatives. Second way is the use the function alternatives_makesym() like the example below.

EXAMPLE
pkg_postinst() {
    alternatives_makesym "/usr/bin/python" "/usr/bin/python2.3" "/usr/bin/python2.2" }
The above example will create a symlink at /usr/bin/python to either /usr/bin/python2.3 or /usr/bin/python2.2. It will choose python2.3 over python2.2 if both exist.

Alternatively, you can use this function:

pkg_postinst() {
   alternatives_auto_makesym "/usr/bin/python" "/usr/bin/python[0-9].[0-9]" }

This will use bash pathname expansion to fill a list of alternatives it can link to. It is probably more robust against version upgrades. You should consider using this unless you are want to do something special.

FUNCTIONS
alternatives_auto_makesym
automatic deduction based on a symlink and a regex mask
alernatives-pkg_postinst
The alternatives pkg_postinst, this function will be exported
alternatives_pkg_postrm
The alternatives pkg_postrm, this function will be exported
ECLASS VARIABLES
SOURCE
The symlink to be created
ALTERNATIVES
The list of alternatives
AUTHORS
Original author: Alastair Tse <liquidx@gentoo.org> (03 Oct 2003)
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
alternatives.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/alternatives.eclass
Index
NAME
DESCRIPTION
EXAMPLE
FUNCTIONS
ECLASS VARIABLES
AUTHORS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:01 GMT, October 11, 2020