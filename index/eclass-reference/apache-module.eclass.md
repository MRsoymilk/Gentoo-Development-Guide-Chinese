APACHE-MODULE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
apache-module.eclass - Provides a common set of functions for apache modules
DESCRIPTION
This eclass handles apache modules in a sane way.
To make use of this eclass simply call one of the need/want_apache functions described in depend.apache.eclass. Make sure you use the need/want_apache call after you have defined DEPEND and RDEPEND. Also note that you can not rely on the automatic RDEPEND=DEPEND that portage does if you use this eclass.

See Bug 107127 for more information.

EXAMPLE
Here is a simple example of an ebuild for mod_foo:

APACHE2_MOD_CONF="42_mod_foo"
APACHE2_MOD_DEFINE="FOO"
need_apache2
A more complicated example for a module with non-standard locations:

APXS2_S="${S}/apache22/src"
APACHE2_MOD_FILE="${APXS2_S}/${PN}.so"
APACHE2_MOD_CONF="42_${PN}"
APACHE2_MOD_DEFINE="FOO"
DOCFILES="docs/*.html"
need_apache2_2
A basic module configuration which just loads the module into apache:

<IfDefine FOO>
LoadModule foo_module modules/mod_foo.so
</IfDefine>
FUNCTIONS
APXS2_S
Path to temporary build directory. (Defaults to `${S}/src' if it exists, `${S}' otherwise)
APXS2_ARGS
Arguments to pass to the apxs tool. (Defaults to `-c ${PN}.c')
APACHE2_EXECFILES
List of files that will be installed into ${APACHE_MODULE_DIR} beside ${APACHE2_MOD_FILE}. In addition, this function also sets the executable permission on those files.
APACHE2_MOD_CONF
Module configuration file installed by src_install (minus the .conf suffix and relative to ${FILESDIR}).
APACHE2_MOD_DEFINE
Name of define (e.g. FOO) to use in conditional loading of the installed module/its config file, multiple defines should be space separated.
APACHE2_MOD_FILE
Name of the module that src_install installs minus the .so suffix. (Defaults to `${APXS2_S}/.libs/${PN}.so')
APACHE2_VHOST_CONF
Virtual host configuration file installed by src_install (minus the .conf suffix and relative to ${FILESDIR}).
DOCFILES
If the exported src_install() is being used, and ${DOCFILES} is non-zero, some sed-fu is applied to split out html documentation (if any) from normal documentation, and dodoc'd or dohtml'd.
apache-module_src_compile
The default action is to call ${APXS} with the value of ${APXS2_ARGS}. If a module requires a different build setup than this, use ${APXS} in your own src_compile routine.
apache-module_src_install
This installs the files into apache's directories. The module is installed from a directory chosen as above (apache_cd_dir). In addition, this function can also set the executable permission on files listed in ${APACHE2_EXECFILES}. The configuration file name is listed in ${APACHE2_MOD_CONF} without the .conf extensions, so if you configuration is 55_mod_foo.conf, APACHE2_MOD_CONF would be 55_mod_foo. ${DOCFILES} contains the list of files you want filed as documentation.
apache-module_pkg_postinst
This prints out information about the installed module and how to enable it.
MAINTAINERS
apache-devs@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
apache-module.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/apache-module.eclass
Index
NAME
DESCRIPTION
EXAMPLE
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020