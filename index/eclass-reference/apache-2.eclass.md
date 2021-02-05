APACHE-2.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
apache-2.eclass - Provides a common set of functions for apache-2.x ebuilds
DESCRIPTION
This eclass handles apache-2.x ebuild functions such as LoadModule generation and inter-module dependency checking.
SUPPORTED EAPIS
5 6 7
FUNCTIONS
GENTOO_DEVELOPER
This variable needs to be set in the ebuild and contains the name of the gentoo developer who created the patch tarball
GENTOO_PATCHSTAMP
This variable needs to be set in the ebuild and contains the date the patch tarball was created at in YYYYMMDD format
GENTOO_PATCH_A = "${GENTOO_PATCHNAME}-${GENTOO_PATCHSTAMP}.tar.bz2"
This variable should contain the entire filename of patch tarball. Defaults to the name of the patchset, with a datestamp.
IUSE_MPMS_FORK
This variable needs to be set in the ebuild and contains a list of forking (i.e. non-threaded) MPMs
IUSE_MPMS_THREAD
This variable needs to be set in the ebuild and contains a list of threaded MPMs
IUSE_MODULES
This variable needs to be set in the ebuild and contains a list of available built-in modules
MODULE_DEPENDS
This variable needs to be set in the ebuild and contains a space-separated list of dependency tokens each with a module and the module it depends on separated by a colon
setup_mpm
This internal function makes sure that only one of APACHE2_MPMS was selected or a default based on USE=threads is selected if APACHE2_MPMS is empty
MODULE_CRITICAL
This variable needs to be set in the ebuild and contains a space-separated list of modules critical for the default apache. A user may still disable these modules for custom minimal installation at their own risk.
check_module_critical
This internal function warns the user about modules critical for the default apache configuration.
setup_modules
This internal function selects all built-in modules based on USE flags and APACHE2_MODULES USE_EXPAND flags
MODULE_DEFINES
This variable needs to be set in the ebuild and contains a space-separated list of tokens each mapping a module to a runtime define which can be specified in APACHE2_OPTS in /etc/conf.d/apache2 to enable this particular module.
generate_load_module
This internal function generates the LoadModule lines for httpd.conf based on the current module selection and MODULE_DEFINES
check_upgrade
This internal function checks if the previous configuration file for built-in modules exists in ROOT and prevents upgrade in this case. Users are supposed to convert this file to the new APACHE2_MODULES USE_EXPAND variable and remove it afterwards.
apache-2_pkg_setup
This function selects built-in modules, the MPM and other configure options, creates the apache user and group and informs about CONFIG_SYSVIPC being needed (we don't depend on kernel sources and therefore cannot check).
apache-2_src_prepare
This function applies patches, configures a custom file-system layout and rebuilds the configure scripts.
apache-2_src_configure
This function adds compiler flags and runs econf and emake based on MY_MPM and MY_CONF
apache-2_src_install
This function runs `emake install' and generates, installs and adapts the gentoo specific configuration files found in the tarball
apache-2_pkg_postinst
This function creates test certificates if SSL is enabled and installs the default index.html to /var/www/localhost if it does not exist. We do this here because the default webroot is a copy of the files that exist elsewhere and we don't want them to be managed/removed by portage when apache is upgraded.
ECLASS VARIABLES
GENTOO_PATCHNAME = "gentoo-${PF}"
This internal variable contains the prefix for the patch tarball. Defaults to the full name and version (including revision) of the package. If you want to override this in an ebuild, use: ORIG_PR="(revision of Gentoo stuff you want)" GENTOO_PATCHNAME="gentoo-${PN}-${PV}${ORIG_PR:+-${ORIG_PR}}"
GENTOO_PATCHDIR = "${WORKDIR}/${GENTOO_PATCHNAME}"
This internal variable contains the working directory where patches and config files are located. Defaults to the patchset name appended to the working directory.
MY_MPM
This internal variable contains the selected MPM after a call to setup_mpm()
MY_CONF
This internal variable contains the econf options for the current module selection after a call to setup_modules()
MY_MODS
This internal variable contains a sorted, space separated list of currently selected modules after a call to setup_modules()
MAINTAINERS
polynomial-c@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
apache-2.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/apache-2.eclass
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