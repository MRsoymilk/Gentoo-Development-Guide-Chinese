AUTOTOOLS-UTILS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
autotools-utils.eclass - common ebuild functions for autotools-based packages
DESCRIPTION
autotools-utils.eclass is autotools.eclass(5) and base.eclass(5) wrapper providing all inherited features along with econf arguments as Bash array, out of source build with overridable build dir location, static archives handling, libtool files removal.
Please note that autotools-utils does not support mixing of its phase functions with regular econf/emake calls. If necessary, please call autotools-utils_src_compile instead of the latter.

SUPPORTED EAPIS
4 5
EXAMPLE
Typical ebuild using autotools-utils.eclass:
EAPI="2"

inherit autotools-utils

DESCRIPTION="Foo bar application"
HOMEPAGE="http://example.org/foo/"
SRC_URI="mirror://sourceforge/foo/${P}.tar.bz2"

LICENSE="LGPL-2.1"
KEYWORDS=""
SLOT="0"
IUSE="debug doc examples qt4 static-libs tiff"

CDEPEND="
        media-libs/libpng:0
        qt4? (
                dev-qt/qtcore:4
                dev-qt/qtgui:4
        )
        tiff? ( media-libs/tiff:0 )
"
RDEPEND="${CDEPEND}
        !media-gfx/bar
"
DEPEND="${CDEPEND}
        doc? ( app-doc/doxygen )
"

# bug 123456
AUTOTOOLS_IN_SOURCE_BUILD=1

DOCS=(AUTHORS ChangeLog README "Read me.txt" TODO)

PATCHES=(
        "${FILESDIR}/${P}-gcc44.patch" # bug 123458
        "${FILESDIR}/${P}-as-needed.patch"
        "${FILESDIR}/${P}-unbundle_libpng.patch"
)

src_configure() {
        local myeconfargs=(
                $(use_enable debug)
                $(use_with qt4)
                $(use_enable threads multithreading)
                $(use_with tiff)
        )
        autotools-utils_src_configure
}

src_compile() {
        autotools-utils_src_compile
        use doc && autotools-utils_src_compile docs
}

src_install() {
        use doc && HTML_DOCS=("${BUILD_DIR}/apidocs/html/")
        autotools-utils_src_install
        if use examples; then
                dobin "${BUILD_DIR}"/foo_example{1,2,3} \
                        || die 'dobin examples failed'
        fi
}

FUNCTIONS
autotools-utils_src_prepare
The src_prepare function.
Supporting PATCHES array and user patches. See base.eclass(5) for reference.

autotools-utils_src_configure
The src_configure function. For out of source build it creates build directory and runs econf there. Configuration parameters defined in myeconfargs are passed here to econf. Additionally following USE flags are known:
IUSE="static-libs" passes --enable-shared and either --disable-static/--enable-static to econf respectively.

myeconfargs
Optional econf arguments as Bash array. Should be defined before calling src_configure.
src_configure() {
        local myeconfargs=(
                --disable-readline
                --with-confdir="/etc/nasty foo confdir/"
                $(use_enable debug cnddebug)
                $(use_enable threads multithreading)
        )
        autotools-utils_src_configure
}
autotools-utils_src_compile
The autotools src_compile function, invokes emake in specified BUILD_DIR.
autotools-utils_src_install
The autotools src_install function. Runs emake install, unconditionally removes unnecessary static libs (based on shouldnotlink libtool property) and removes unnecessary libtool files when static-libs USE flag is defined and unset.
DOCS and HTML_DOCS arrays are supported. See base.eclass(5) for reference.

autotools-utils_src_test
The autotools src_test function. Runs emake check in build directory.
ECLASS VARIABLES
AUTOTOOLS_AUTORECONF
Set to a non-empty value before calling inherit to enable running autoreconf in src_prepare() and adding autotools dependencies.
This is usually necessary when using live sources or applying patches modifying configure.ac or Makefile.am files. Note that in the latter case setting this variable is obligatory even though the eclass will work without it (to add the necessary dependencies).

The eclass will try to determine the correct autotools to run including a few external tools: gettext, glib-gettext, intltool, gtk-doc, gnome-doc-prepare. If your tool is not supported, please open a bug and we'll add support for it.

Note that dependencies are added for autoconf, automake and libtool only. If your package needs one of the external tools listed above, you need to add appropriate packages to DEPEND yourself.

BUILD_DIR
Build directory, location where all autotools generated files should be placed. For out of source builds it defaults to ${WORKDIR}/${P}_build.
This variable has been called AUTOTOOLS_BUILD_DIR formerly. It is set under that name for compatibility.

AUTOTOOLS_IN_SOURCE_BUILD
Set to enable in-source build.
ECONF_SOURCE
Specify location of autotools' configure script. By default it uses ${S}.
DOCS
Array containing documents passed to dodoc command.
In EAPIs 4+, can list directories as well.

Example:

DOCS=( NEWS README )
HTML_DOCS
Array containing documents passed to dohtml command.
Example:

HTML_DOCS=( doc/html/ )
PATCHES
PATCHES array variable containing all various patches to be applied.
Example:

PATCHES=( "${FILESDIR}"/${P}-mypatch.patch )
AUTOTOOLS_PRUNE_LIBTOOL_FILES
Sets the mode of pruning libtool files. The values correspond to prune_libtool_files parameters, with leading dashes stripped.
Defaults to pruning the libtool files when static libraries are not installed or can be linked properly without them. Libtool files for modules (plugins) will be kept in case plugin loader needs them.

If set to 'modules', the .la files for modules will be removed as well. This is often the preferred option.

If set to 'all', all .la files will be removed unconditionally. This option is discouraged and shall be used only if 'modules' does not remove the files.

If set to 'none', no .la files will be pruned ever. Use in corner cases only.

MAINTAINERS
Maciej Mrozowski <reavertm@gentoo.org>
Michał Górny <mgorny@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
autotools-utils.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/autotools-utils.eclass
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
Time: 00:27:04 GMT, October 11, 2020