MOZCORECONF-V5.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
mozcoreconf-v5.eclass - core options and configuration functions for mozilla
DESCRIPTION
inherit mozconfig-v6.* or above for mozilla configuration support

FUNCTIONS
mozconfig_annotate
add an annotated line to .mozconfig
Example: mozconfig_annotate "building on ultrasparc" --enable-js-ultrasparc => ac_add_options --enable-js-ultrasparc # building on ultrasparc

mozconfig_use_enable
add a line to .mozconfig based on a USE-flag
Example: mozconfig_use_enable truetype freetype2 => ac_add_options --enable-freetype2 # +truetype

mozconfig_use_with
add a line to .mozconfig based on a USE-flag
Example: mozconfig_use_with kerberos gss-api /usr/$(get_libdir) => ac_add_options --with-gss-api=/usr/lib # +kerberos

mozconfig_use_extension
enable or disable an extension based on a USE-flag
Example: mozconfig_use_extension gnome gnomevfs => ac_add_options --enable-extensions=gnomevfs

mozconfig_init
Initialize mozilla configuration and populate with core settings. This should be called in src_configure before any other mozconfig_* functions.
mozconfig_final
Apply EXTRA_ECONF values to .mozconfig Display a table describing all configuration options paired with reasons, then clean up extensions list. This should be called in src_configure at the end of all other mozconfig_* functions.
ECLASS VARIABLES
MOZILLA_FIVE_HOME
This is an eclass-generated variable that defines the rpath that the mozilla product will be installed in. Read-only
MAINTAINERS
Mozilla team <mozilla@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
mozcoreconf-v5.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/mozcoreconf-v5.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
ECLASS VARIABLES
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020