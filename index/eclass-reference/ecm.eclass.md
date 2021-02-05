ECM.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
ecm.eclass - Support eclass for packages that use KDE Frameworks with ECM.
DESCRIPTION
This eclass is intended to streamline the creation of ebuilds for packages that use cmake and KDE Frameworks' extra-cmake-modules, thereby following some of their packaging conventions. It is primarily intended for the three upstream release groups (Frameworks, Plasma, Applications) but also for any other package that follows similar conventions.
This eclass unconditionally inherits cmake.eclass and all its public variables and helper functions (not phase functions) may be considered as part of this eclass's API.

This eclass's phase functions are not intended to be mixed and matched, so if any phase functions are overridden the version here should also be called.

Porting from kde5.class - Convert all add_*_dep dependency functions to regular dependencies - Manually set LICENSE - Manually set SLOT - Rename vars and function names as needed, see kde5.eclass PORTING comments - Instead of FRAMEWORKS_MINIMAL, define KFMIN in ebuilds and use it for deps

SUPPORTED EAPIS
7
FUNCTIONS
ecm_punt_bogus_dep <prefix> <dependency>
Removes a specified dependency from a find_package call with multiple components.
ecm_pkg_pretend
Checks if the active compiler meets the minimum version requirements. phase function is only exported if KDE_GCC_MINIMAL is defined.
ecm_pkg_setup
Checks if the active compiler meets the minimum version requirements.
ecm_src_prepare
Wrapper for cmake_src_prepare with lots of extra logic for magic handling of linguas, tests, handbook etc.
ecm_src_configure
Wrapper for cmake_src_configure with extra logic for magic handling of handbook, tests etc.
ecm_src_compile
Wrapper for cmake_src_compile. Currently doesn't do anything extra, but is included as part of the API just in case it's needed in the future.
ecm_src_test
Wrapper for cmake_src_test with extra logic for magic handling of dbus and virtualx.
ecm_src_install
Wrapper for cmake_src_install. Currently doesn't do anything extra, but is included as part of the API just in case it's needed in the future.
ecm_pkg_preinst
Sets up environment variables required in ecm_pkg_postinst.
ecm_pkg_postinst
Updates the various XDG caches (icon, desktop, mime) if necessary.
ecm_pkg_postrm
Updates the various XDG caches (icon, desktop, mime) if necessary.
ECLASS VARIABLES
VIRTUALX_REQUIRED ?= manual
For proper description see virtualx.eclass manpage. Here we redefine default value to be manual, if your package needs virtualx for tests you should proceed with setting VIRTUALX_REQUIRED=test.
ECM_NONGUI
By default, for all CATEGORIES except kde-frameworks, assume we are building a GUI application. Add dependency on kde-frameworks/breeze-icons or kde-frameworks/oxygen-icons and run the xdg.eclass routines for pkg_preinst, pkg_postinst and pkg_postrm. If set to "true", do nothing.
ECM_KDEINSTALLDIRS ?= true
Assume the package is using KDEInstallDirs macro and switch KDE_INSTALL_USE_QT_SYS_PATHS to ON. If set to "false", do nothing.
ECM_DEBUG ?= true
Add "debug" to IUSE. If !debug, add -DQT_NO_DEBUG to CPPFLAGS. If set to "false", do nothing.
ECM_DESIGNERPLUGIN ?= false
If set to "true", add "designer" to IUSE to toggle build of designer plugins and add the necessary BDEPEND. If set to "false", do nothing.
ECM_EXAMPLES ?= false
By default unconditionally ignore a top-level examples subdirectory. If set to "true", add "examples" to IUSE to toggle adding that subdirectory.
ECM_HANDBOOK ?= false
Will accept "true", "false", "optional", "forceoptional". If set to "false", do nothing. Otherwise, add "+handbook" to IUSE, add the appropriate dependency, and let KF5DocTools generate and install the handbook from docbook file(s) found in ECM_HANDBOOK_DIR. However if !handbook, disable build of ECM_HANDBOOK_DIR in CMakeLists.txt. If set to "optional", build with -DCMAKE_DISABLE_FIND_PACKAGE_KF5DocTools=ON when !handbook. In case package requires KF5KDELibs4Support, see next: If set to "forceoptional", remove a KF5DocTools dependency from the root CMakeLists.txt in addition to the above.
ECM_HANDBOOK_DIR ?= doc
Specifies the directory containing the docbook file(s) relative to ${S} to be processed by KF5DocTools (kdoctools_install).
ECM_PO_DIRS ?= "po poqm"
Specifies directories of l10n files relative to ${S} to be processed by KF5I18n (ki18n_install). If IUSE nls exists and is disabled then disable build of these directories in CMakeLists.txt.
ECM_QTHELP
Default value for all CATEGORIES except kde-frameworks is "false". If set to "true", add "doc" to IUSE, add the appropriate dependency, let -DBUILD_QCH=ON generate and install Qt compressed help files when USE=doc. If set to "false", do nothing.
ECM_TEST
Will accept "true", "false", "optional", "forceoptional", "forceoptional-recursive". Default value is "false", except for CATEGORY=kde-frameworks where it is set to "true". If set to "false", do nothing. For any other value, add "test" to IUSE and DEPEND on dev-qt/qttest:5. If set to "optional", build with -DCMAKE_DISABLE_FIND_PACKAGE_Qt5Test=ON when USE=!test. If set to "forceoptional", punt Qt5Test dependency and ignore "autotests", "test", "tests" subdirs from top-level CMakeLists.txt when USE=!test. If set to "forceoptional-recursive", punt Qt5Test dependencies and make autotest(s), unittest(s) and test(s) subdirs from *any* CMakeLists.txt in ${S} and below conditional on BUILD_TESTING when USE=!test. This is always meant as a short-term fix and creates ${T}/${P}-tests-optional.patch to refine and submit upstream.
KFMIN
Minimum version of Frameworks to require. Default value for kde-frameworks is ${PV} and 5.64.0 baseline for everything else. This is not going to be changed unless we also bump EAPI, which usually implies (rev-)bumping. Version will later be used to differentiate between KF5/Qt5 and KF6/Qt6.
KDE_GCC_MINIMAL
Minimum version of active GCC to require. This is checked in ecm_pkg_pretend and ecm_pkg_setup.
MAINTAINERS
kde@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
ecm.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/ecm.eclass
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