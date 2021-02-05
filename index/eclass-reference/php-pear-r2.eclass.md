PHP-PEAR-R2.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
php-pear-r2.eclass - Provides means for an easy installation of PEAR packages.
DESCRIPTION
This eclass provides means for an easy installation of PEAR packages. For more information on PEAR, see https://pear.php.net/ Note that this eclass doesn't handle dependencies of PEAR packages on purpose; please use (R)DEPEND to define them correctly!
SUPPORTED EAPIS
6 7
FUNCTIONS
php-pear-r2_install_packagexml
Copies the package2.xml or package.xml file and, optionally, the channel.xml file to a Gentoo-specific location so that pkg_postinst can install the package to the local PEAR database
php-pear-r2_src_install
Takes care of standard install for PEAR packages. Override src_install if the package installs more than "${PHP_PEAR_PKG_NAME}.php" or "${PHP_PEAR_PKG_NAME%%_*}/" as a directory
php-pear-r2_pkg_postinst
Register package with the local PEAR database.
php-pear-r2_pkg_postrm
Deregister package from the local PEAR database
ECLASS VARIABLES
PHP_PEAR_PKG_NAME ?= ${PN/PEAR-/}
Set this if the PEAR package name differs from ${PN/PEAR-/} (generally shouldn't be the case).
PEAR_PV ?= ${PV}
Set in ebuild if the ${PV} breaks SRC_URI for alpha/beta/rc versions
PHP_PEAR_DOMAIN ?= pear.php.net
Set in ebuild to the domain name of the channel if not pear.php.net When the domain is not pear.php.net, setting the SRC_URI is required
PHP_PEAR_CHANNEL
Set in ebuild to the path of channel.xml file which is necessary for 3rd party pear channels (besides pear.php.net) to be added to PEAR Default is unset to do nothing
AUTHORS
Author: Brian Evans <grknight@gentoo.org>
MAINTAINERS
Gentoo PHP Team <php-bugs@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
php-pear-r2.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/php-pear-r2.eclass
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
Time: 00:27:02 GMT, October 11, 2020