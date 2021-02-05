PHP-EXT-PECL-R3.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
php-ext-pecl-r3.eclass - A uniform way to install PECL extensions
DESCRIPTION
This eclass should be used by all dev-php/pecl-* ebuilds as a uniform way of installing PECL extensions. For more information about PECL, see https://pecl.php.net/
FUNCTIONS
php-ext-pecl-r3_src_install
Install a standard PECL package. First we delegate to php-ext-source-r3.eclass, and then we attempt to install examples found in a standard location.
php-ext-pecl-r3_src_test
Run tests delivered with the PECL package. Phpize will have generated a run-tests.php file to be executed by `make test`. We only need to force the test suite to run in non-interactive mode.
ECLASS VARIABLES
PHP_EXT_PECL_PKG = "${PN/pecl-/}"
Set in ebuild before inheriting this eclass if the tarball name differs from ${PN/pecl-/} so that SRC_URI and HOMEPAGE get set correctly by the eclass.
Setting this variable manually also affects PHP_EXT_NAME and ${S} unless you override those in ebuild. If that is not desired, please use PHP_EXT_PECL_FILENAME instead.

PHP_EXT_PECL_FILENAME
Set in ebuild before inheriting this eclass if the tarball name differs from "${PN/pecl-/}-${PV}.tgz" so that SRC_URI gets set correctly by the eclass.
Unlike PHP_EXT_PECL_PKG, setting this variable does not affect HOMEPAGE, PHP_EXT_NAME or ${S}.

MAINTAINERS
Gentoo PHP team <php-bugs@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
php-ext-pecl-r3.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/php-ext-pecl-r3.eclass
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
