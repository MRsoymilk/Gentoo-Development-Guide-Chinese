MOZCONFIG-V6.52.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
mozconfig-v6.52.eclass - the new mozilla common configuration eclass for FF33 and newer, v6
DESCRIPTION
This eclass is used in mozilla ebuilds (firefox, thunderbird, seamonkey) to provide a single common place for the common mozilla engine compoments.
The eclass provides all common dependencies as well as common use flags.

Some use flags which may be optional in particular mozilla packages can be supported through setting eclass variables.

This eclass inherits mozconfig helper functions as defined in mozcoreconf-v3, and so ebuilds inheriting this eclass do not need to inherit that.

SUPPORTED EAPIS
5 6 7
FUNCTIONS
mozconfig_config
Set common configure options for mozilla packages. Call this within src_configure() phase, after mozconfig_init
Example:

inherit mozconfig-v6.46

src_configure() {         mozconfig_init
        mozconfig_config
# ... misc ebuild-unique settings via calls to # ... mozconfig_{annotate,use_with,use_enable} mozconfig_final }

mozconfig_install_prefs
Set preferences into the prefs.js file specified as a parameter to the function. This sets both some common prefs to all mozilla packages, and any prefs that may relate to the use flags administered by mozconfig_config().
Call this within src_install() phase, after copying the template prefs file (if any) from ${FILESDIR}

Example:

inherit mozconfig-v6.46

src_install() {         cp "${FILESDIR}"/gentoo-default-prefs.js "${BUILD_OBJ_DIR}/dist/bin/browser/defaults/preferences/all-gentoo.js" || die
        mozconfig_install_prefs "${BUILD_OBJ_DIR}/dist/bin/browser/defaults/preferences/all-gentoo.js"
... }

ECLASS VARIABLES
MOZCONFIG_OPTIONAL_WIFI
Set this variable before the inherit line, when an ebuild needs to provide optional necko-wifi support via IUSE="wifi". Currently this would include ebuilds for firefox, and potentially seamonkey.
Leave the variable UNSET if necko-wifi support should not be available. Set the variable to "enabled" if the use flag should be enabled by default. Set the variable to any value if the use flag should exist but not be default-enabled.

MOZCONFIG_OPTIONAL_JIT
Set this variable before the inherit line, when an ebuild needs to provide deterministic jit support via IUSE="jit". The upstream default will be used otherwise, which is generally to enable jit unless support for the platform is missing.
Set the variable to "enabled" if the use flag should be enabled by default. Set the variable to any value if the use flag should exist but not be default-enabled.

MOZCONFIG_OPTIONAL_GTK3
Set this variable before the inherit line, when an ebuild can provide optional gtk3 support via IUSE="force-gtk3". Currently this would include thunderbird and seamonkey in the future, once support is ready for testing.
Leave the variable UNSET if gtk3 support should not be optionally available. Set the variable to "enabled" if the use flag should be enabled by default. Set the variable to any value if the use flag should exist but not be default-enabled. If gtk+:3 is to be the standard toolkit, do not use this and instead use MOZCONFIG_OPTIONAL_GTK2ONLY.

MOZCONFIG_OPTIONAL_GTK2ONLY
Set this variable before the inherit line, when an ebuild can provide optional gtk2-only support via IUSE="gtk2".
Note that this option conflicts directly with MOZCONFIG_OPTIONAL_GTK3, both variables cannot be set at the same time and this variable will be ignored if MOZCONFIG_OPTIONAL_GTK3 is set.

Leave the variable UNSET if gtk2-only support should not be available. Set the variable to "enabled" if the use flag should be enabled by default. Set the variable to any value if the use flag should exist but not be default-enabled.

MOZCONFIG_OPTIONAL_QT5
Set this variable before the inherit line, when an ebuild can provide optional qt5 support via IUSE="qt5". Currently this would include ebuilds for firefox, but thunderbird and seamonkey could follow in the future.
Leave the variable UNSET if qt5 support should not be available. Set the variable to "enabled" if the use flag should be enabled by default. Set the variable to any value if the use flag should exist but not be default-enabled.

MAINTAINERS
mozilla team <mozilla@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
mozconfig-v6.52.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/mozconfig-v6.52.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
ECLASS VARIABLES
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:05 GMT, October 11, 2020
