MOZLINGUAS-V2.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
mozlinguas-v2.eclass - Handle language packs for mozilla products
DESCRIPTION
Sets IUSE according to MOZ_LANGS (language packs available). Also exports src_unpack, src_compile and src_install for use in ebuilds, and provides supporting functions for langpack generation and installation.
SUPPORTED EAPIS
2 3 4 5 6
FUNCTIONS
mozlinguas_src_unpack
Unpack xpi language packs according to the user's LINGUAS settings
mozlinguas_mozconfig
if applicable, add the necessary flag to .mozconfig to support the generation of locales. Note that this function requires mozconfig_annontate to already be declared via an inherit of mozconfig or mozcoreconf.
mozlinguas_src_compile
if applicable, build the selected locales.
mozlinguas_xpistage_langpacks
Add extra langpacks to the xpi-stage dir for prebuilt plugins
First argument is the path to the extension Second argument is the prefix of the source (same as first if unspecified) Remaining arguments are the modules in the extension that are localized
 (basename of first if unspecified)

Example - installing extra langpacks for lightning: src_install() {         ... # general installation steps
        mozlinguas_xpistage_langpacks   "${BUILD_OBJ_DIR}"/dist/xpi-stage/lightning     "${WORKDIR}"/lightning lightning calendar
... # proceed with installation from the xpi-stage dir }

mozlinguas-v2_src_install
Install xpi language packs according to the user's L10N settings NOTE - uses ${BUILD_OBJ_DIR} or PWD if unset, for source-generated langpacks
ECLASS VARIABLES
MOZ_LANGS ?= ()
Array containing the list of language pack xpis available for this release. The list can be updated with scripts/get_langs.sh from the mozilla overlay.
MOZ_PV ?= "${PV}"
Ebuild package version converted to equivalent upstream version. Defaults to ${PV}, and should be overridden for alphas, betas, and RCs
MOZ_PN ?= "${PN}"
Ebuild package name converted to equivalent upstream name. Defaults to ${PN}, and should be overridden for binary ebuilds.
MOZ_P ?= "${MOZ_PN}-${MOZ_PV}"
Ebuild package name + version converted to upstream equivalent. Defaults to ${MOZ_PN}-${MOZ_PV}
MOZ_FTP_URI ?= ""
The ftp URI prefix for the release tarballs and language packs.
MOZ_HTTP_URI ?= ""
The http URI prefix for the release tarballs and language packs.
MOZ_LANGPACK_HTTP_URI ?= ${MOZ_HTTP_URI}
An alternative http URI if it differs from official mozilla URI. Defaults to whatever MOZ_HTTP_URI was set to.
MOZ_LANGPACK_PREFIX ?= "${MOZ_PV}/linux-i686/xpi/"
The relative path till the lang code in the langpack file URI. Defaults to ${MOZ_PV}/linux-i686/xpi/
MOZ_LANGPACK_SUFFIX ?= ".xpi"
The suffix after the lang code in the langpack file URI. Defaults to '.xpi'
MOZ_LANGPACK_UNOFFICIAL ?= ""
The status of the langpack, used to differentiate within Manifests and on Gentoo mirrors as to when the langpacks are generated officially by Mozilla or if they were generated unofficially by others (ie the Gentoo mozilla team). When this var is set, the distfile will have a .unofficial.xpi suffix.
MOZ_GENERATE_LANGPACKS ?= ""
This flag specifies whether or not the langpacks should be generated directly during the build process, rather than being downloaded and installed from upstream pre-built extensions. Primarily it supports pre-release builds. Defaults to empty.
MOZ_L10N_SOURCEDIR ?= "${WORKDIR}/l10n-sources"
The path that l10n sources can be found at, once unpacked. Defaults to ${WORKDIR}/l10n-sources
MOZ_L10N_URI_PREFIX ?= ""
The full URI prefix of the distfile for each l10n locale. The AB_CD and MOZ_L10N_URI_SUFFIX will be appended to this to complete the SRC_URI when MOZ_GENERATE_LANGPACKS is set. If empty, nothing will be added to SRC_URI. Defaults to empty.
MOZ_L10N_URI_SUFFIX ?= ".tar.xz"
The suffix of l10n source distfiles. Defaults to '.tar.xz'
MOZ_FORCE_UPSTREAM_L10N ?= ""
Set this to use upstream langpaks even if the package normally shouldn't (ie it is an alpha or beta package)
MOZ_INSTALL_L10N_XPIFILE ?= ""
Install langpacks as .xpi file instead of unpacked directory. Leave unset to install unpacked
AUTHORS
Nirbheek Chauhan <nirbheek@gentoo.org>
Ian Stakenvicius <axs@gentoo.org>
MAINTAINERS
mozilla@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
mozlinguas-v2.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/mozlinguas-v2.eclass
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
Time: 00:27:04 GMT, October 11, 2020