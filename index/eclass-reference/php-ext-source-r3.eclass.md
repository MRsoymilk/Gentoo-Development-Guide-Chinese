PHP-EXT-SOURCE-R3.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
php-ext-source-r3.eclass - Compile and install standalone PHP extensions.
DESCRIPTION
A unified interface for compiling and installing standalone PHP extensions.
SUPPORTED EAPIS
6 7
FUNCTIONS
php-ext-source-r3_src_prepare
Runs the default src_prepare() for PATCHES/eapply_user() support (optional), and for each PHP slot, makes a copy of sources, initializes the environment, and calls php-ext-source-r3_phpize().
php-ext-source-r3_phpize
Subject to PHP_EXT_SKIP_PHPIZE, this function runs phpize and autoreconf in a manner that avoids warnings.
php-ext-source-r3_src_configure
Takes care of standard configure for PHP extensions (modules).
php-ext-source-r3_src_compile
Compile a standard standalone PHP extension.
php-ext-source-r3_src_install
Install a standard standalone PHP extension. Uses einstalldocs() to support the DOCS variable/array.
php-ext-source-r3_src_test
Run tests delivered with the standalone PHP extension. Phpize will have generated a run-tests.php file to be executed by `make test`. We only need to force the test suite to run in non-interactive mode.
php_get_slots
Get a list of PHP slots contained in both the ebuild's USE_PHP and the user's PHP_TARGETS.
php_init_slot_env <slot>
Takes a slot name, and initializes some global variables to values corresponding to that slot. For example, it sets the path to the "php" and "phpize" binaries, which will differ for each slot. This function is intended to be called while looping through a list of slots obtained from php_get_slots().
Calling this function will change the working directory to the temporary build directory for the given slot.

php-ext-source-r3_createinifiles
Builds INI files for every enabled slot and SAPI.
php-ext-source-r3_addtoinifiles <setting-or-section-name> [setting-value] [message]
Add settings to every php.ini file installed by this extension. You can also add new [Section]s -- see the example below.
Add some settings for the extension:

php-ext-source-r3_addtoinifiles "zend_optimizer.optimization_level" "15"
php-ext-source-r3_addtoinifiles "zend_optimizer.enable_loader" "0"
php-ext-source-r3_addtoinifiles "zend_optimizer.disable_licensing" "0"

Adding values to a section in php.ini file installed by the extension:

php-ext-source-r3_addtoinifiles "[Debugger]"
php-ext-source-r3_addtoinifiles "debugger.enabled" "on"
php-ext-source-r3_addtoinifiles "debugger.profiler_enabled" "on"
ECLASS VARIABLES
PHP_EXT_NAME (REQUIRED)
The extension name. This must be set, otherwise the eclass dies. Only automagically set by php-ext-pecl-r3.eclass, so unless your ebuild inherits that eclass, you must set this manually before inherit.
PHP_EXT_INI = "yes"
Controls whether or not to add a line to php.ini for the extension. Defaults to "yes" and should not be changed in most cases.
PHP_EXT_ZENDEXT = "no"
Controls whether the extension is a ZendEngine extension or not. Defaults to "no". If you don't know what this is, you don't need it.
USE_PHP (REQUIRED)
Lists the PHP slots compatible the extension is compatible with. Example:
USE_PHP="php5-6 php7-0"
PHP_EXT_OPTIONAL_USE
If set, all of the dependencies added by this eclass will be conditional on USE=${PHP_EXT_OPTIONAL_USE}. This is needed when ebuilds have to inherit this eclass unconditionally, but only actually use it when (for example) the user has USE=php.
PHP_EXT_S = "${S}"
The relative location of the temporary build directory for the PHP extension within the source package. This is useful for packages that bundle the PHP extension. Defaults to ${S}.
PHP_EXT_SAPIS = "apache2 cli cgi fpm embed phpdbg"
A list of SAPIs for which we will install this extension. Formerly called PHPSAPILIST. The default includes every SAPI currently used in the tree.
PHP_INI_NAME ?= ${PHP_EXT_NAME}
An optional file name of the saved ini file minis the ini extension This allows ordering of extensions such that one is loaded before or after another. Defaults to the PHP_EXT_NAME. Example (produces 40-foo.ini file):
PHP_INI_NAME="40-foo"
PHP_EXT_NEEDED_USE
A list of USE flags to append to each PHP target selected as a valid USE-dependency string. The value should be valid for all targets so USE defaults may be necessary. Example:
PHP_EXT_NEEDED_USE="mysql?,pdo,pcre(+)"
The PHP dependencies will result in:

php_targets_php7-0? ( dev-lang/php:7.0[mysql?,pdo,pcre(+)] )
PHP_EXT_SKIP_PHPIZE
By default, we run "phpize" in php-ext-source-r3_src_prepare(). Set PHP_EXT_SKIP_PHPIZE="yes" in your ebuild if you do not want to run phpize (and the autoreconf that becomes necessary afterwards).
PHP_EXT_SKIP_PATCHES
By default, we run default_src_prepare to PHP_EXT_S. Set PHP_EXT_SKIP_PATCHES="yes" in your ebuild if you want to apply patches yourself.
PHP_EXT_ECONF_ARGS
Set this in the ebuild to pass additional configure options to econf. Formerly called my_conf. Either a string or an array of --flag=value parameters is supported.
MAINTAINERS
Gentoo PHP team <php-bugs@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
php-ext-source-r3.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/php-ext-source-r3.eclass
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
Time: 00:27:02 GMT, October 11, 2020