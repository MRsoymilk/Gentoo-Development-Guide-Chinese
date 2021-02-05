VDR-PLUGIN-2.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
vdr-plugin-2.eclass - common vdr plugin ebuild functions
DESCRIPTION
Eclass for easing maintenance of vdr plugin ebuilds
SUPPORTED EAPIS
5 6 7
FUNCTIONS
fix_vdr_libsi_include
Plugins failed on compile with wrong path of libsi includes, this can be fixed by 'function + space separated list of files'
Example:

fix_vdr_libsi_include bla.c foo.c
vdr_remove_i18n_include
Compile will fail if plugin still use the old i18n language handling, most parts are fixed by vdr-plugin-2.eclass internal functions itself. Remove unneeded i18.n includes from files, if they are still wrong there, this can be fixed by 'function + space separated list of files"
Example:

vdr_remove_i18n_include bla.n foo.n
ECLASS VARIABLES
VDR_CONFD_FILE
A plugin config file can be specified through the $VDR_CONFD_FILE variable, it defaults to ${FILESDIR}/confd. Each config file will be installed as e.g. ${D}/etc/conf.d/vdr.${VDRPLUGIN}
VDR_RCADDON_FILE
Installing rc-addon files is basically the same as for plugin config files (see above), it's just using the $VDR_RCADDON_FILE variable instead. The default value when $VDR_RCADDON_FILE is undefined is: ${FILESDIR}/rc-addon.sh and will be installed as ${VDR_RC_DIR}/plugin-${VDRPLUGIN}.sh
The rc-addon files will be sourced by the startscript when the specific plugin has been enabled. rc-addon files may be used to prepare everything that is necessary for the plugin start/stop, like passing extra command line options and so on.

NOTE: rc-addon files must be valid shell scripts!

GENTOO_VDR_CONDITIONAL
This is a hack for ebuilds like vdr-xineliboutput that want to conditionally install a vdr-plugin
PO_SUBDIR
By default, translation are found in"${S}"/po but this default can be overridden by defining PO_SUBDIR.
Example:

PO_SUBDIR="bla foo/bla"
AUTHORS
Matthias Schwarzott <zzam@gentoo.org>
Joerg Bornkessel <hd_brummy@gentoo.org>
Christian Ruppert <idl0r@gentoo.org>
(undisclosed contributors)
MAINTAINERS
Gentoo VDR Project <vdr@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
vdr-plugin-2.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/vdr-plugin-2.eclass
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
Time: 00:27:04 GMT, October 11, 2020
