UDEV.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
udev.eclass - Default eclass for determining udev directories.
DESCRIPTION
Default eclass for determining udev directories.
SUPPORTED EAPIS
0 1 2 3 4 5 6 7
EXAMPLE
inherit udev

# Example of the eclass usage:
RDEPEND="virtual/udev"
DEPEND="${RDEPEND}"

src_configure() {
econf --with-rulesdir="$(get_udevdir)"/rules.d
}

src_install() {
default
# udev_dorules contrib/99-foomatic
# udev_newrules contrib/98-foomatic 99-foomatic
}
FUNCTIONS
udev_get_udevdir
Use the short version $(get_udevdir) instead!
get_udevdir
Output the path for the udev directory (not including ${D}). This function always succeeds, even if udev is not installed. The fallback value is set to /lib/udev
udev_dorules <rule> [...]
Install udev rule(s). Uses doins, thus it is fatal in EAPI 4 and non-fatal in earlier EAPIs.
udev_newrules <oldname> <newname>
Install udev rule with a new name. Uses newins, thus it is fatal in EAPI 4 and non-fatal in earlier EAPIs.
udev_reload
Run udevadm control --reload to refresh rules and databases
MAINTAINERS
udev-bugs@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
udev.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/udev.eclass
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
