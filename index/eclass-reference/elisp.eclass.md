ELISP.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
elisp.eclass - Eclass for Emacs Lisp packages
DESCRIPTION
This eclass is designed to install elisp files of Emacs related packages into the site-lisp directory. The majority of elisp packages will only need to define the standard ebuild variables (like SRC_URI) and optionally SITEFILE for successful installation.

Emacs support for other than pure elisp packages is handled by elisp-common.eclass where you won't have a dependency on Emacs itself. All elisp-* functions are documented there.

If the package's source is a single (in whatever way) compressed elisp file with the file name ${P}.el, then this eclass will move ${P}.el to ${PN}.el in src_unpack().

SUPPORTED EAPIS
4 5 6 7
FUNCTIONS
elisp_pkg_setup
Test if the eselected Emacs version is sufficient to fulfil the version requirement of the NEED_EMACS variable.
elisp_src_unpack
Unpack the sources; also handle the case of a single *.el file in WORKDIR for packages distributed that way.
elisp_src_prepare
Apply any patches listed in ELISP_PATCHES. Patch files are searched for in the current working dir, WORKDIR, and FILESDIR.
elisp_src_configure
Do nothing, because Emacs packages seldomly bring a full build system.
elisp_src_compile
Call elisp-compile to byte-compile all Emacs Lisp (*.el) files. If ELISP_TEXINFO lists any Texinfo sources, call makeinfo to generate GNU Info files from them.
elisp_src_install
Call elisp-install to install all Emacs Lisp (*.el and *.elc) files. If the SITEFILE variable specifies a site-init file, install it with elisp-site-file-install. Also install any GNU Info files listed in ELISP_TEXINFO and documentation listed in the DOCS variable.
elisp_pkg_postinst
Call elisp-site-regen, in order to collect the site initialisation for all installed Emacs Lisp packages in the site-gentoo.el file.
elisp_pkg_postrm
Call elisp-site-regen, in order to collect the site initialisation for all installed Emacs Lisp packages in the site-gentoo.el file.
ECLASS VARIABLES
NEED_EMACS
If you need anything different from Emacs 23, use the NEED_EMACS variable before inheriting elisp.eclass. Set it to the version your package uses and the dependency will be adjusted.
ELISP_PATCHES
Space separated list of patches to apply after unpacking the sources. Patch files are searched for in the current working dir, WORKDIR, and FILESDIR. This variable is semi-deprecated, preferably use the PATCHES array instead if the EAPI supports it.
ELISP_REMOVE
Space separated list of files to remove after unpacking the sources.
SITEFILE
Name of package's site-init file. The filename must match the shell pattern "[1-8][0-9]*-gentoo.el"; numbers below 10 and above 89 are reserved for internal use. "50${PN}-gentoo.el" is a reasonable choice in most cases.
ELISP_TEXINFO
Space separated list of Texinfo sources. Respective GNU Info files will be generated in src_compile() and installed in src_install().
AUTHORS
Matthew Kennedy <mkennedy@gentoo.org>
Jeremy Maitin-Shepard <jbms@attbi.com>
Christian Faulhammer <fauli@gentoo.org>
Ulrich Müller <ulm@gentoo.org>
MAINTAINERS
Gentoo GNU Emacs project <gnu-emacs@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
elisp.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/elisp.eclass
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