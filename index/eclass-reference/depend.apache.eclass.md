DEPEND.APACHE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
depend.apache.eclass - Functions to allow ebuilds to depend on apache
DESCRIPTION
This eclass handles depending on apache in a sane way and provides information about where certain binaries and configuration files are located.
To make use of this eclass simply call one of the need/want_apache functions described below. Make sure you use the need/want_apache call after you have defined DEPEND and RDEPEND. Also note that you can not rely on the automatic RDEPEND=DEPEND that portage does if you use this eclass.

See Bug 107127 for more information.

SUPPORTED EAPIS
0 2 3 4 5 6
EXAMPLE
Here is an example of an ebuild depending on apache:

DEPEND="virtual/Perl-CGI"
RDEPEND="${DEPEND}"
need_apache2
Another example which demonstrates non-standard IUSE options for optional apache support:

DEPEND="server? ( virtual/Perl-CGI )"
RDEPEND="${DEPEND}"
want_apache2 server

pkg_setup() {
        depend.apache_pkg_setup server
}
FUNCTIONS
depend.apache_pkg_setup [myiuse]
An ebuild calls this in pkg_setup() to initialize variables for optional apache-2.x support. If the myiuse parameter is not given it defaults to apache2.
want_apache [myiuse]
An ebuild calls this to get the dependency information for optional apache support. If the myiuse parameter is not given it defaults to apache2. An ebuild should additionally call depend.apache_pkg_setup() in pkg_setup() with the same myiuse parameter.
want_apache2 [myiuse]
An ebuild calls this to get the dependency information for optional apache-2.x support. If the myiuse parameter is not given it defaults to apache2. An ebuild should additionally call depend.apache_pkg_setup() in pkg_setup() with the same myiuse parameter.
want_apache2_2 [myiuse]
An ebuild calls this to get the dependency information for optional apache-2.2.x support. If the myiuse parameter is not given it defaults to apache2. An ebuild should additionally call depend.apache_pkg_setup() in pkg_setup() with the same myiuse parameter.
want_apache2_4 [myiuse]
An ebuild calls this to get the dependency information for optional apache-2.4.x support. If the myiuse parameter is not given it defaults to apache2. An ebuild should additionally call depend.apache_pkg_setup() in pkg_setup() with the same myiuse parameter.
need_apache
An ebuild calls this to get the dependency information for apache.
need_apache2
An ebuild calls this to get the dependency information for apache-2.x.
need_apache2_2
An ebuild calls this to get the dependency information for apache-2.2.x.
need_apache2_4
An ebuild calls this to get the dependency information for apache-2.4.x.
has_apache
An ebuild calls this to get runtime variables for an indirect apache dependency without USE-flag, in which case want_apache does not work. DO NOT call this function in global scope.
has_apache_threads [myflag]
An ebuild calls this to make sure thread-safety is enabled if apache has been built with a threaded MPM. If the myflag parameter is not given it defaults to threads.
has_apache_threads_in <myforeign> [myflag]
An ebuild calls this to make sure thread-safety is enabled in a foreign package if apache has been built with a threaded MPM. If the myflag parameter is not given it defaults to threads.
ECLASS VARIABLES
APACHE_VERSION
Stores the version of apache we are going to be ebuilding. This variable is set by the want/need_apache functions.
APXS
Path to the apxs tool. This variable is set by the want/need_apache functions.
APACHE_BIN
Path to the apache binary. This variable is set by the want/need_apache functions.
APACHE_CTL
Path to the apachectl tool. This variable is set by the want/need_apache functions.
APACHE_BASEDIR
Path to the server root directory. This variable is set by the want/need_apache functions (EAPI=0 through 5) or depend.apache_pkg_setup (EAPI=6 and later).
APACHE_CONFDIR
Path to the configuration file directory. This variable is set by the want/need_apache functions.
APACHE_MODULES_CONFDIR
Path where module configuration files are kept. This variable is set by the want/need_apache functions.
APACHE_VHOSTS_CONFDIR
Path where virtual host configuration files are kept. This variable is set by the want/need_apache functions.
APACHE_MODULESDIR
Path where we install modules. This variable is set by the want/need_apache functions (EAPI=0 through 5) or depend.apache_pkg_setup (EAPI=6 and later).
APACHE_DEPEND = "www-servers/apache"
Dependencies for Apache
APACHE2_DEPEND = "=www-servers/apache-2*"
Dependencies for Apache 2.x
APACHE2_2_DEPEND = "=www-servers/apache-2.2*"
Dependencies for Apache 2.2.x
APACHE2_4_DEPEND = "=www-servers/apache-2.4*"
Dependencies for Apache 2.4.x
MAINTAINERS
apache-devs@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
depend.apache.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/depend.apache.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
EXAMPLE
FUNCTIONS
ECLASS VARIABLES
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020