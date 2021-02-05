GNOME2.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
gnome2.eclass - Provides phases for Gnome/Gtk+ based packages.
DESCRIPTION
Exports portage base functions used by ebuilds written for packages using the GNOME framework. For additional functions, see gnome2-utils.eclass.
SUPPORTED EAPIS
4 5 6
FUNCTIONS
gnome2_src_unpack
Stub function for old EAPI.
gnome2_src_prepare
Prepare environment for build, fix build of scrollkeeper documentation, run elibtoolize.
gnome2_src_configure
Gnome specific configure handling
gnome2_src_compile
Only default src_compile for now
gnome2_src_install
Gnome specific install. Handles typical GConf and scrollkeeper setup in packages and removal of .la files if requested
gnome2_pkg_preinst
Finds Icons, GConf and GSettings schemas for later handling in pkg_postinst
gnome2_pkg_postinst
Handle scrollkeeper, GConf, GSettings, Icons, desktop and mime database updates.
gnome2_pkg_postrm
Handle scrollkeeper, GSettings, Icons, desktop and mime database updates.
ECLASS VARIABLES
GNOME2_EAUTORECONF = ${GNOME2_EAUTORECONF:-""}
Run eautoreconf instead of only elibtoolize
DOCS
String containing documents passed to dodoc command for eapi4. In eapi5 we rely on einstalldocs (from eutils.eclass) and for newer EAPIs we follow PMS spec.
ELTCONF = ${ELTCONF:-""}
Extra options passed to elibtoolize
G2CONF
Extra configure opts passed to econf. Deprecated, pass extra arguments to gnome2_src_configure. Banned in eapi6 and newer.
GCONF_DEBUG
Whether to handle debug or not. Some gnome applications support various levels of debugging (yes, no, minimum, etc), but using --disable-debug also removes g_assert which makes debugging harder. This variable should be set to yes for such packages for the eclass to handle it properly. It will enable minimal debug with USE=-debug. Note that this is most commonly found in configure.ac as GNOME_DEBUG_CHECK.
Banned since eapi6 as upstream is moving away from this obsolete macro in favor of autoconf-archive macros, that do not expose this issue (bug #270919)

GNOME2_LA_PUNT
For eapi4 it sets if we should delete ALL or none of the .la files For eapi5 and newer it relies on prune_libtool_files (from eutils.eclass) for this. Available values for GNOME2_LA_PUNT: - "no": will not clean any .la files - "yes": will run prune_libtool_files --modules - If it is not set, it will run prune_libtool_files
MAINTAINERS
gnome@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
gnome2.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/gnome2.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
ECLASS VARIABLES
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:01 GMT, October 11, 2020