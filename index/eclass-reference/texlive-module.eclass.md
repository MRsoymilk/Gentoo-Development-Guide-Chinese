TEXLIVE-MODULE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
texlive-module.eclass - Provide generic install functions so that modular texlive's texmf ebuild will only have to inherit this eclass
DESCRIPTION
Purpose: Provide generic install functions so that modular texlive's texmf ebuilds will only have to inherit this eclass. Ebuilds have to provide TEXLIVE_MODULE_CONTENTS variable that contains the list of packages that it will install. (See below)
For TeX Live versions prior to 2009, the ebuild was supposed to unpack the texmf and texmf-dist directories to ${WORKDIR} (which is what the default src_unpack does). Starting from TeX Live 2009, the eclass provides a src_unpack function taking care of unpacking and relocating the files that need it.

It inherits texlive-common. Patching is supported via the PATCHES bash array.

SUPPORTED EAPIS
7
FUNCTIONS
texlive-module_src_unpack
Only for TeX Live 2009 and later. After unpacking, the files that need to be relocated are moved accordingly.
texlive-module_add_format
Creates/appends to a format.${PN}.cnf file for fmtutil. It parses the AddFormat directive of tlpobj files to create it. This will make fmtutil generate the formats when asked and allow the remaining src_compile phase to build the formats.
texlive-module_make_language_def_lines
Creates a language.${PN}.def entry to put in /etc/texmf/language.def.d. It parses the AddHyphen directive of tlpobj files to create it.
texlive-module_make_language_dat_lines
Creates a language.${PN}.dat entry to put in /etc/texmf/language.dat.d. It parses the AddHyphen directive of tlpobj files to create it.
texlive-module_synonyms_to_language_lua_line
Helper function for texlive-module_make_language_lua_lines to generate a correctly formatted synonyms entry for language.dat.lua.
texlive-module_make_language_lua_lines
Only valid for TeXLive 2010 and later. Creates a language.${PN}.dat.lua entry to put in /etc/texmf/language.dat.lua.d. It parses the AddHyphen directive of tlpobj files to create it.
texlive-module_src_compile
exported function: Generates the config files that are to be installed in /etc/texmf; texmf-update script will take care of merging the different config files for different packages in a single one used by the whole tex installation.
Once the config files are generated, we build the format files using fmtutil (provided by texlive-core). The compiled format files will be sent to texmf-var/web2c, like fmtutil defaults to but with some trick to stay in the sandbox.

texlive-module_src_install
exported function: Installs texmf and config files to the system.
texlive-module_pkg_postinst
exported function: Run texmf-update to ensure the tex installation is consistent with the installed texmf trees.
texlive-module_pkg_postrm
exported function: Run texmf-update to ensure the tex installation is consistent with the installed texmf trees.
ECLASS VARIABLES
TEXLIVE_MODULE_CONTENTS (REQUIRED)
The list of packages that will be installed. This variable will be expanded to SRC_URI: foo -> texlive-module-foo-${PV}.tar.xz
TEXLIVE_MODULE_DOC_CONTENTS (REQUIRED)
The list of packages that will be installed if the doc useflag is enabled. Expansion to SRC_URI is the same as for TEXLIVE_MODULE_CONTENTS.
TEXLIVE_MODULE_SRC_CONTENTS (REQUIRED)
The list of packages that will be installed if the source useflag is enabled. Expansion to SRC_URI is the same as for TEXLIVE_MODULE_CONTENTS.
TEXLIVE_MODULE_BINSCRIPTS
A space separated list of files that are in fact scripts installed in the texmf tree and that we want to be available directly. They will be installed in /usr/bin.
TEXLIVE_MODULE_BINLINKS
A space separated list of links to add for BINSCRIPTS. The systax is: foo:bar to create a symlink bar -> foo.
TL_MODULE_INFORMATION
Information to display about the package. e.g. for enabling/disabling a feature
TEXLIVE_MODULE_OPTIONAL_ENGINE
A space separated list of Tex engines that can be made optional. e.g. "luatex luajittex"
AUTHORS
Original Author: Alexis Ballier <aballier@gentoo.org>
MAINTAINERS
tex@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
texlive-module.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/texlive-module.eclass
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