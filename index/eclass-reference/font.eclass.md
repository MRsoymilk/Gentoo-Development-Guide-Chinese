FONT.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
font.eclass - Eclass to make font installation uniform
SUPPORTED EAPIS
5 6 7
FUNCTIONS
font_xfont_config
Generate Xorg font files (mkfontscale/mkfontdir).
font_fontconfig
Install fontconfig conf files given in FONT_CONF.
font_cleanup_dirs
Remove font directories containing only generated files.
font_pkg_setup
The font pkg_setup function. Collision protection
font_src_install
The font src_install function.
_update_fontcache
Updates fontcache if !prefix and media-libs/fontconfig installed
font_pkg_postinst
The font pkg_postinst function.
font_pkg_postrm
The font pkg_postrm function.
ECLASS VARIABLES
FONT_SUFFIX = ${FONT_SUFFIX:-} (REQUIRED)
Space delimited list of font suffixes to install.
FONT_S
Directory containing the fonts. If unset, ${S} is used instead. Can also be an array of several directories.
FONT_PN = ${FONT_PN:-${PN}}
Font name (ie. last part of FONTDIR).
FONTDIR = ${FONTDIR:-/usr/share/fonts/${FONT_PN}}
Full path to installation directory.
FONT_CONF = ( "" )
Array containing fontconfig conf files to install.
DOCS = ${DOCS:-}
Space delimited list of docs to install. We always install these: COPYRIGHT README{,.txt} NEWS AUTHORS BUGS ChangeLog FONTLOG.txt
MAINTAINERS
fonts@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
font.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/font.eclass
Index
NAME
SUPPORTED EAPIS
FUNCTIONS
ECLASS VARIABLES
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020