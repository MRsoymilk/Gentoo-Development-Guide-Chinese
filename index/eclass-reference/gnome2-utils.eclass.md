GNOME2-UTILS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
gnome2-utils.eclass - Auxiliary functions commonly used by Gnome packages.
DESCRIPTION
This eclass provides a set of auxiliary functions needed by most Gnome packages. It may be used by non-Gnome packages as needed for handling various Gnome stack related functions such as:
 * GSettings schemas management
 * GConf schemas management
 * scrollkeeper (old Gnome help system) management
SUPPORTED EAPIS
0 1 2 3 4 5 6 7
FUNCTIONS
gnome2_environment_reset
Reset various variables inherited from root's evironment to a reasonable default for ebuilds to help avoid access violations and test failures.
gnome2_gconf_savelist
Find the GConf schemas that are about to be installed and save their location in the GNOME2_ECLASS_SCHEMAS environment variable. This function should be called from pkg_preinst.
gnome2_gconf_install
Applies any schema files installed by the current ebuild to Gconf's database using gconftool-2. This function should be called from pkg_postinst.
gnome2_gconf_uninstall
Removes schema files previously installed by the current ebuild from Gconf's database.
gnome2_omf_fix
Workaround applied to Makefile rules in order to remove redundant calls to scrollkeeper-update and sandbox violations. This function should be called from src_prepare.
gnome2_scrollkeeper_savelist
Find the scrolls that are about to be installed and save their location in the GNOME2_ECLASS_SCROLLS environment variable. This function should be called from pkg_preinst.
gnome2_scrollkeeper_update
Updates the global scrollkeeper database. This function should be called from pkg_postinst and pkg_postrm.
gnome2_schemas_savelist
Find if there is any GSettings schema to install and save the list in GNOME2_ECLASS_GLIB_SCHEMAS variable. This is only necessary for eclass implementations that call gnome2_schemas_update conditionally. This function should be called from pkg_preinst.
gnome2_schemas_update
Updates GSettings schemas. This function should be called from pkg_postinst and pkg_postrm.
gnome2_gdk_pixbuf_savelist
Find if there is any gdk-pixbuf loader to install and save the list in GNOME2_ECLASS_GDK_PIXBUF_LOADERS variable. This function should be called from pkg_preinst.
gnome2_gdk_pixbuf_update
Updates gdk-pixbuf loader cache if GNOME2_ECLASS_GDK_PIXBUF_LOADERS has some. This function should be called from pkg_postinst and pkg_postrm.
gnome2_query_immodules_gtk2
Updates gtk2 immodules/gdk-pixbuf loaders listing.
gnome2_query_immodules_gtk3
Updates gtk3 immodules/gdk-pixbuf loaders listing.
gnome2_giomodule_cache_update
Updates glib's gio modules cache. This function should be called from pkg_postinst and pkg_postrm.
gnome2_disable_deprecation_warning
Disable deprecation warnings commonly found in glib based packages. Should be called from src_prepare.
gnome2_icon_savelist
Find the icons that are about to be installed and save their location in the GNOME2_ECLASS_ICONS environment variable. This is only necessary for eclass implementations that call gnome2_icon_cache_update conditionally. This function should be called from pkg_preinst.
gnome2_icon_cache_update
Updates Gtk+ icon cache files under /usr/share/icons. Deprecated. Please use xdg_icon_cache_update from xdg-utils.eclass
MAINTAINERS
gnome@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
gnome2-utils.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/gnome2-utils.eclass
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
Time: 00:27:02 GMT, October 11, 2020