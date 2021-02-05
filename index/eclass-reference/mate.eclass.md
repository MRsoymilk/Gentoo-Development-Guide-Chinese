MATE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
mate.eclass - Provides phases for MATE based packages.
DESCRIPTION
Exports portage base functions used by ebuilds written for packages using the MATE framework. Occassionally acts as a wrapper to gnome2 due to the fact that MATE is a GNOME fork. For additional functions, see gnome2-utils.eclass.
SUPPORTED EAPIS
6
FUNCTIONS
mate_py_cond_func_wrap
Wraps a function for conditional python use, to run for each python implementation in the build directory. This function should only be used if the ebuild also inherits the python-r1 eclass
ematedocize
A wrapper around mate-doc-common
want_mate_doc
Returns true/false based on whether eautoreconf should call ematedocize
mate_src_prepare
Call gnome2_src_prepare to handle environment setup and patching, then call eautoreconf if necessary
mate_src_configure
MATE specific configure handling Stub to gnome2_src_configure()
mate_src_install
MATE specific install. Stub to gnome2_src_install
mate_pkg_preinst
Finds Icons, GConf and GSettings schemas for later handling in pkg_postinst Stub to gnome2_pkg_preinst
mate_pkg_postinst
Handle scrollkeeper, GConf, GSettings, Icons, desktop and mime database updates. Stub to gnome2_pkg_postinst
mate_pkg_postrm
Handle scrollkeeper, GSettings, Icons, desktop and mime database updates. Stub to gnome2_pkg_postrm
ECLASS VARIABLES
MATE_LA_PUNT = ${MATE_LA_PUNT:-""}
Available values for MATE_LA_PUNT: - "no": will not clean any .la files - "yes": will run prune_libtool_files --modules - If it is not set, it will run prune_libtool_files MATE_LA_PUNT is a stub to GNOME2_LA_PUNT
MATE_FORCE_AUTORECONF ?= "false"
Available values for MATE_FORCE_AUTORECONF: - true: will always run eautoreconf - false: will default to automatic detect - If it is not set, it will default to false
AUTHORS
Authors: NP-Hardass <NP-Hardass@gentoo.org> based upon the gnome2
and autotools-utils eclasses
MAINTAINERS
mate@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
mate.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/mate.eclass
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