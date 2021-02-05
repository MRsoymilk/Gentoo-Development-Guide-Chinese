LIBRETRO-CORE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
libretro-core.eclass - Simplify libretro core ebuilds
DESCRIPTION
The libretro eclass is designed to streamline the construction of ebuilds for Libretro core ebuilds.
Libretro cores can be found under https://github.com/libretro/

They all use the same basic make based build system, are located in the same github account, and do not release named or numbered versions (so ebuild versions for git commits are keys). This eclass covers those commonalities reducing much duplication between the ebuilds.

SUPPORTED EAPIS
6 7
EXAMPLE
EAPI=7

LIBRETRO_CORE_NAME="2048"
LIBRETRO_COMMIT_SHA="45655d3662e4cbcd8afb28e2ee3f5494a75888de"
KEYWORDS="~amd64 ~x86"
inherit libretro-core

DESCRIPTION="Port of 2048 puzzle game to the libretro API"
LICENSE="Unlicense"
SLOT="0"
FUNCTIONS
libretro-core_src_unpack
The libretro-core src_unpack function which is exported.
This function retrieves the remote Libretro core info files.

libretro-core_src_prepare
The libretro-core src_prepare function which is exported.
This function prepares the source by making custom modifications.

myemakeargs
Optional emake arguments as a bash array. Should be defined before calling src_compile.
src_compile() {
local myemakeargs=(
        $(usex neon "HAVE_NEON=1" "")
)
libretro-core_src_compile
}
libretro-core_src_compile
The libretro-core src_compile function which is exported.
This function compiles the shared library for this Libretro core.

LIBRETRO_CORE_LIB_FILE
Absolute path of this Libretro core's shared library. src_install.
src_install() {
        local LIBRETRO_CORE_LIB_FILE="${S}/somecore_libretro.so"

        libretro-core_src_install
}
libretro-core_src_install
The libretro-core src_install function which is exported.
This function installs the shared library for this Libretro core.

ECLASS VARIABLES
LIBRETRO_CORE_NAME (REQUIRED)
Name of this Libretro core. The libretro-core_src_install() phase function will install the shared library "${S}/${LIBRETRO_CORE_NAME}_libretro.so" as a Libretro core. Defaults to the name of the current package with the "libretro-" prefix excluded and hyphens replaced with underscores (e.g. genesis_plus_gx for libretro-genesis-plus-gx)
LIBRETRO_COMMIT_SHA
Commit SHA used for SRC_URI will die if not set in <9999 ebuilds. Needs to be set before inherit.
LIBRETRO_REPO_NAME ?= "libretro/libretro-${LIBRETRO_CORE_NAME}" (REQUIRED)
Contains the real repo name of the core formatted as "repouser/reponame". Needs to be set before inherit. Otherwise defaults to "libretro/${PN}"
AUTHORS
Cecil Curry <leycec@gmail.com>
Craig Andrews <candrews@gentoo.org>
MAINTAINERS
candrews@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
libretro-core.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/libretro-core.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
EXAMPLE
FUNCTIONS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020