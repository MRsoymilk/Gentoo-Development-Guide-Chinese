GKRELLM-PLUGIN.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
gkrellm-plugin.eclass - Provides src_install used by (almost) all gkrellm plugins
DESCRIPTION
- Sets up default dependencies - Provides a common src_install method to avoid code duplication
Changelog:
  03 January 2018: David Seifert <soap@gentoo.org>
    - Port to EAPI 6, remove built_with_use, simplify a lot
  12 March 2007: Jim Ramsay <lack@gentoo.org>
    - Added server plugin support
  09 March 2007: Jim Ramsay <lack@gentoo.org>
    - Initial commit

SUPPORTED EAPIS
6
FUNCTIONS
gkrellm-plugin_src_install
Install the plugins and call einstalldocs
ECLASS VARIABLES
PLUGIN_SO
The name of the plugin's .so file which will be installed in the plugin dir. Defaults to "${PN}$(get_modname)". Has to be a bash array.
PLUGIN_SERVER_SO
The name of the plugin's server plugin $(get_modname) portion. Unset by default. Has to be a bash array.
PLUGIN_DOCS
An optional list of docs to be installed, in addition to the default DOCS variable which is respected too. Has to be a bash array.
AUTHORS
Original author: Jim Ramsay

  EAPI 6 author: David Seifert
MAINTAINERS
maintainer-needed@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
gkrellm-plugin.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/gkrellm-plugin.eclass
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