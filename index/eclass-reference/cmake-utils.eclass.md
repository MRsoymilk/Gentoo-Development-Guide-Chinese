CMAKE-UTILS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
cmake-utils.eclass - common ebuild functions for cmake-based packages
DESCRIPTION
DEPRECATED: This no longer receives any changes. Everyone must port to cmake.eclass. The cmake-utils eclass makes creating ebuilds for cmake-based packages much easier. It provides all inherited features (DOCS, HTML_DOCS, PATCHES) along with out-of-source builds (default), in-source builds and an implementation of the well-known use_enable and use_with functions for CMake.
SUPPORTED EAPIS
5 6 7
FUNCTIONS
cmake_comment_add_subdirectory <subdirectory>
Comment out one or more add_subdirectory calls in CMakeLists.txt in the current directory
comment_add_subdirectory <subdirectory>
Comment out an add_subdirectory call in CMakeLists.txt in the current directory Banned in EAPI 6 and later - use cmake_comment_add_subdirectory instead.
cmake-utils_use_with <USE flag> [flag name]
Based on use_with. See ebuild(5).
`cmake-utils_use_with foo FOO` echoes -DWITH_FOO=ON if foo is enabled and -DWITH_FOO=OFF if it is disabled.

cmake-utils_use_enable <USE flag> [flag name]
Based on use_enable. See ebuild(5).
`cmake-utils_use_enable foo FOO` echoes -DENABLE_FOO=ON if foo is enabled and -DENABLE_FOO=OFF if it is disabled.

cmake-utils_use_find_package <USE flag> <package name>
Based on use_enable. See ebuild(5).
`cmake-utils_use_find_package foo LibFoo` echoes -DCMAKE_DISABLE_FIND_PACKAGE_LibFoo=OFF if foo is enabled and -DCMAKE_DISABLE_FIND_PACKAGE_LibFoo=ON if it is disabled. This can be used to make find_package optional.

cmake_use_find_package <USE flag> <package name>
Alias for cmake-utils_use_find_package.
cmake-utils_use_disable <USE flag> [flag name]
Based on inversion of use_enable. See ebuild(5).
`cmake-utils_use_enable foo FOO` echoes -DDISABLE_FOO=OFF if foo is enabled and -DDISABLE_FOO=ON if it is disabled.

cmake-utils_use_no <USE flag> [flag name]
Based on use_disable. See ebuild(5).
`cmake-utils_use_no foo FOO` echoes -DNO_FOO=OFF if foo is enabled and -DNO_FOO=ON if it is disabled.

cmake-utils_use_want <USE flag> [flag name]
Based on use_enable. See ebuild(5).
`cmake-utils_use_want foo FOO` echoes -DWANT_FOO=ON if foo is enabled and -DWANT_FOO=OFF if it is disabled.

cmake-utils_use_build <USE flag> [flag name]
Based on use_enable. See ebuild(5).
`cmake-utils_use_build foo FOO` echoes -DBUILD_FOO=ON if foo is enabled and -DBUILD_FOO=OFF if it is disabled.

cmake-utils_use_has <USE flag> [flag name]
Based on use_enable. See ebuild(5).
`cmake-utils_use_has foo FOO` echoes -DHAVE_FOO=ON if foo is enabled and -DHAVE_FOO=OFF if it is disabled.

cmake-utils_use_use <USE flag> [flag name]
Based on use_enable. See ebuild(5).
`cmake-utils_use_use foo FOO` echoes -DUSE_FOO=ON if foo is enabled and -DUSE_FOO=OFF if it is disabled.

cmake-utils_use <USE flag> [flag name]
Based on use_enable. See ebuild(5).
`cmake-utils_use foo FOO` echoes -DFOO=ON if foo is enabled and -DFOO=OFF if it is disabled.

cmake-utils_useno <USE flag> [flag name]
Based on use_enable. See ebuild(5).
`cmake-utils_useno foo NOFOO` echoes -DNOFOO=OFF if foo is enabled and -DNOFOO=ON if it is disabled.

cmake-utils_src_prepare
Apply ebuild and user patches.
mycmakeargs
Optional cmake defines as a bash array. Should be defined before calling src_configure.
src_configure() {
        local mycmakeargs=(
                $(cmake-utils_use_with openconnect)
        )

        cmake-utils_src_configure
}
cmake-utils_src_configure
General function for configuring with cmake. Default behaviour is to start an out-of-source build.
cmake-utils_src_compile
General function for compiling with cmake. Automatically detects the build type. All arguments are passed to emake.
cmake-utils_src_make
Function for building the package. Automatically detects the build type. All arguments are passed to emake.
cmake-utils_src_test
Function for testing the package. Automatically detects the build type.
cmake-utils_src_install
Function for installing the package. Automatically detects the build type.
ECLASS VARIABLES
BUILD_DIR
Build directory where all cmake processed files should be generated. For in-source build it's fixed to ${CMAKE_USE_DIR}. For out-of-source build it can be overridden, by default it uses ${WORKDIR}/${P}_build.
This variable has been called CMAKE_BUILD_DIR formerly. It is set under that name for compatibility.

CMAKE_BINARY ?= cmake
Eclass can use different cmake binary than the one provided in by system.
CMAKE_BUILD_TYPE ?= Gentoo
Set to override default CMAKE_BUILD_TYPE. Only useful for packages known to make use of "if (CMAKE_BUILD_TYPE MATCHES xxx)". If about to be set - needs to be set before invoking cmake-utils_src_configure. You usualy do *NOT* want nor need to set it as it pulls CMake default build-type specific compiler flags overriding make.conf.
CMAKE_IN_SOURCE_BUILD
Set to enable in-source build.
CMAKE_MAKEFILE_GENERATOR
Specify a makefile generator to be used by cmake. At this point only "emake" and "ninja" are supported. In EAPI 7 and above, the default is set to "ninja", whereas in EAPIs below 7, it is set to "emake".
CMAKE_MIN_VERSION ?= 3.9.6
Specify the minimum required CMake version.
CMAKE_REMOVE_MODULES ?= yes
Do we want to remove anything? yes or whatever else for no
CMAKE_REMOVE_MODULES_LIST ?= "FindBLAS FindLAPACK"
Space-separated list of CMake modules that will be removed in $S during src_prepare, in order to force packages to use the system version.
CMAKE_USE_DIR
Sets the directory where we are working with cmake. For example when application uses autotools and only one plugin needs to be done by cmake. By default it uses ${S}.
CMAKE_VERBOSE ?= ON
Set to OFF to disable verbose messages during compilation
CMAKE_WARN_UNUSED_CLI
Warn about variables that are declared on the command line but not used. Might give false-positives. "no" to disable (default) or anything else to enable.
CMAKE_EXTRA_CACHE_FILE
Specifies an extra cache file to pass to cmake. This is the analog of EXTRA_ECONF for econf and is needed to pass TRY_RUN results when cross-compiling. Should be set by user in a per-package basis in /etc/portage/package.env.
CMAKE_UTILS_QA_SRC_DIR_READONLY
After running cmake-utils_src_prepare, sets ${S} to read-only. This is a user flag and should under _no circumstances_ be set in the ebuild. Helps in improving QA of build systems that write to source tree.
AUTHORS
Tomáš Chvátal <scarabeus@gentoo.org>
Maciej Mrozowski <reavertm@gentoo.org>
(undisclosed contributors)
Original author: Zephyrus (zephyrus@mirach.it)
MAINTAINERS
kde@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
cmake-utils.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/cmake-utils.eclass
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