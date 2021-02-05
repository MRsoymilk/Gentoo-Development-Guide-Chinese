XORG-3.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
xorg-3.eclass - Reduces code duplication in the modularized X11 ebuilds.
DESCRIPTION
This eclass makes trivial X ebuilds possible for apps, drivers, and more. Many things that would normally be done in various functions can be accessed by setting variables instead, such as patching, running eautoreconf, passing options to configure and installing docs.
All you need to do in a basic ebuild is inherit this eclass and set DESCRIPTION, KEYWORDS and RDEPEND/DEPEND. If your package is hosted with the other X packages, you don't need to set SRC_URI. Pretty much everything else should be automatic.

SUPPORTED EAPIS
7
FUNCTIONS
xorg-3_pkg_setup
Setup prefix compat
xorg-3_src_unpack
Simply unpack source code.
xorg-3_reconf_source
Run eautoreconf if necessary, and run elibtoolize.
xorg-3_src_prepare
Prepare a package after unpacking, performing all X-related tasks.
xorg-3_font_configure
If a font package, perform any necessary configuration steps
xorg-3_flags_setup
Set up CFLAGS for a debug build
xorg-3_src_configure
Perform any necessary pre-configuration steps, then run configure
xorg-3_src_compile
Compile a package, performing all X-related tasks.
xorg-3_src_install
Install a built package to ${D}, performing any necessary steps.
xorg-3_pkg_postinst
Run X-specific post-installation tasks on the live filesystem. The only task right now is some setup for font packages.
xorg-3_pkg_postrm
Run X-specific post-removal tasks on the live filesystem. The only task right now is some cleanup for font packages.
remove_font_metadata
Don't let the package install generated font files that may overlap with other packages. Instead, they're generated in pkg_postinst().
create_fonts_scale
Create fonts.scale file, used by the old server-side fonts subsystem.
create_fonts_dir
Create fonts.dir file, used by the old server-side fonts subsystem.
ECLASS VARIABLES
XORG_MULTILIB ?= "no"
If set to 'yes', the multilib support for package will be enabled. Set before inheriting this eclass.
XORG_EAUTORECONF ?= "no"
If set to 'yes' and configure.ac exists, eautoreconf will run. Set before inheriting this eclass.
XORG_BASE_INDIVIDUAL_URI = "https://www.x.org/releases/individual"}
Set up SRC_URI for individual modular releases. If set to an empty string, no SRC_URI will be provided by the eclass.
XORG_MODULE ?= "auto"
The subdirectory to download source from. Possible settings are app, doc, data, util, driver, font, lib, proto, xserver. Set above the inherit to override the default autoconfigured module.
XORG_PACKAGE_NAME ?= ${PN}
For git checkout the git repository might differ from package name. This variable can be used for proper directory specification
XORG_TARBALL_SUFFIX ?= "bz2"
Most X11 projects provide tarballs as tar.bz2 or tar.xz. This eclass defaults to bz2.
XORG_STATIC ?= "yes"
Enables static-libs useflag. Set to no, if your package gets:
QA: configure: WARNING: unrecognized options: --disable-static

XORG_DRI ?= "no"
Possible values are "always" or the value of the useflag DRI capabilities are required for. Default value is "no"
Eg. XORG_DRI="opengl" will pull all dri dependent deps for opengl useflag

XORG_DOC ?= "no"
Possible values are "always" or the value of the useflag doc packages are required for. Default value is "no"
Eg. XORG_DOC="manual" will pull all doc dependent deps for manual useflag

AUTHORS
Author: Tomáš Chvátal <scarabeus@gentoo.org>
Author: Donnie Berkholz <dberkholz@gentoo.org>
Author: Matt Turner <mattst88@gentoo.org>
MAINTAINERS
x11@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
xorg-3.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/xorg-3.eclass
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
Time: 00:27:05 GMT, October 11, 2020