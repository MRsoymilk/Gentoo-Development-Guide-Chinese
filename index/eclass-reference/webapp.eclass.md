WEBAPP.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
webapp.eclass - functions for installing applications to run under a web server
DESCRIPTION
The webapp eclass contains functions to handle web applications with webapp-config. Part of the implementation of GLEP #11
FUNCTIONS
need_httpd
Call this function AFTER your ebuilds DEPEND line if any of the available webservers are able to run this application.
need_httpd_cgi
Call this function AFTER your ebuilds DEPEND line if any of the available CGI-capable webservers are able to run this application.
need_httpd_fastcgi
Call this function AFTER your ebuilds DEPEND line if any of the available FastCGI-capabale webservers are able to run this application.
webapp_configfile <file> [more files ...]
Mark a file config-protected for a web-based application.
webapp_hook_script <file>
Install a script that will run after a virtual copy is created, and before a virtual copy has been removed.
webapp_postinst_txt <lang> <file>
Install a text file containing post-installation instructions.
webapp_postupgrade_txt <lang> <file>
Install a text file containing post-upgrade instructions.
webapp_serverowned [-R] <file> [more files ...]
Identify a file which must be owned by the webserver's user:group settings. The ownership of the file is NOT set until the application is installed using the webapp-config tool. If -R is given directories are handled recursively.
webapp_server_configfile <server> <file> [new name]
Install a configuration file for the webserver. You need to specify a webapp-config supported <server>. if no new name is given `basename $2' is used by default. Note: this function will automagically prepend $1 to the front of your config file's name.
webapp_sqlscript <db> <file> [version]
Install a SQL script that creates/upgrades a database schema for the web application. Currently supported database engines are mysql and postgres. If a version is given the script should upgrade the database schema from the given version to $PVR.
webapp_src_preinst
You need to call this function in src_install() BEFORE anything else has run. For now we just create required webapp-config directories.
webapp_pkg_setup
The default pkg_setup() for this eclass. This will gather required variables from webapp-config and check if there is an application installed to `${ROOT%/}/var/www/localhost/htdocs/${PN}/' if USE=vhosts is not set.
You need to call this function BEFORE anything else has run in your custom pkg_setup().

webapp_src_install
This is the default src_install(). For now, we just make sure that root owns everything, and that there are no setuid files.
You need to call this function AFTER everything else has run in your custom src_install().

webapp_pkg_postinst
The default pkg_postinst() for this eclass. This installs the web application to `${ROOT%/}/var/www/localhost/htdocs/${PN}/' if USE=vhosts is not set. Otherwise display a short notice how to install this application with webapp-config.
You need to call this function AFTER everything else has run in your custom pkg_postinst().

webapp_pkg_prerm
This is the default pkg_prerm() for this eclass. If USE=vhosts is not set remove all installed copies of this web application. Otherwise instruct the user to manually remove those copies. See bug #136959.
ECLASS VARIABLES
WEBAPP_DEPEND = ">=app-admin/webapp-config-1.50.15"
An ebuild should use WEBAPP_DEPEND if a custom DEPEND needs to be built, most notably in combination with WEBAPP_OPTIONAL.
WEBAPP_NO_AUTO_INSTALL
An ebuild sets this to `yes' if an automatic installation and/or upgrade is not possible. The ebuild should overwrite pkg_postinst() and explain the reason for this BEFORE calling webapp_pkg_postinst().
WEBAPP_OPTIONAL
An ebuild sets this to `yes' to make webapp support optional, in which case you also need to take care of USE-flags and dependencies.
MAINTAINERS
web-apps@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
webapp.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/webapp.eclass
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
Time: 00:27:01 GMT, October 11, 2020