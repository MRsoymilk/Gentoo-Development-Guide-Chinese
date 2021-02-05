CMAKE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
cmake.eclass - common ebuild functions for cmake-based packages
DESCRIPTION
The cmake eclass makes creating ebuilds for cmake-based packages much easier. It provides all inherited features (DOCS, HTML_DOCS, PATCHES) along with out-of-source builds (default), in-source builds and an implementation of the well-known use_enable function for CMake.
SUPPORTED EAPIS
7
FUNCTIONS
cmake_run_in <working dir> <run command>
Set the desired working dir for a function or command.
cmake_comment_add_subdirectory <subdirectory>
Comment out one or more add_subdirectory calls in CMakeLists.txt in the current directory
cmake_use_find_package <USE flag> <package name>
Based on use_enable. See ebuild(5).
`cmake_use_find_package foo LibFoo` echoes -DCMAKE_DISABLE_FIND_PACKAGE_LibFoo=OFF if foo is enabled and -DCMAKE_DISABLE_FIND_PACKAGE_LibFoo=ON if it is disabled. This can be used to make find_package optional.

cmake_src_prepare
Apply ebuild and user patches.
mycmakeargs
Optional cmake defines as a bash array. Should be defined before calling src_configure.
src_configure() {
        local mycmakeargs=(
                $(cmake_use_with openconnect)
        )

        cmake_src_configure
}
cmake_src_configure
General function for configuring with cmake. Default behaviour is to start an out-of-source build.
cmake_src_compile
General function for compiling with cmake. Automatically detects the build type. All arguments are passed to emake.
cmake_build
Function for building the package. Automatically detects the build type. All arguments are passed to emake.
cmake_src_test
Function for testing the package. Automatically detects the build type.
cmake_src_install
Function for installing the package. Automatically detects the build type.
ECLASS VARIABLES
BUILD_DIR ?= ${WORKDIR}/${P}_build
Build directory where all cmake processed files should be generated. For in-source build it's fixed to ${CMAKE_USE_DIR}. For out-of-source build it can be overridden, by default it uses ${WORKDIR}/${P}_build.
CMAKE_BINARY ?= cmake
Eclass can use different cmake binary than the one provided in by system.
CMAKE_BUILD_TYPE ?= Gentoo
Set to override default CMAKE_BUILD_TYPE. Only useful for packages known to make use of "if (CMAKE_BUILD_TYPE MATCHES xxx)". If about to be set - needs to be set before invoking cmake_src_configure. You usually do *NOT* want nor need to set it as it pulls CMake default build-type specific compiler flags overriding make.conf.
CMAKE_IN_SOURCE_BUILD
Set to enable in-source build.
CMAKE_MAKEFILE_GENERATOR ?= ninja
Specify a makefile generator to be used by cmake. At this point only "emake" and "ninja" are supported. The default is set to "ninja".
CMAKE_REMOVE_MODULES_LIST ?= "FindBLAS FindLAPACK"
Array of CMake modules that will be removed in $S during src_prepare, in order to force packages to use the system version. Set to "none" to disable removing modules entirely.
CMAKE_USE_DIR
Sets the directory where we are working with cmake, for example when application uses autotools and only one plugin needs to be done by cmake. By default it uses ${S}.
CMAKE_VERBOSE ?= ON
Set to OFF to disable verbose messages during compilation
CMAKE_WARN_UNUSED_CLI ?= yes
Warn about variables that are declared on the command line but not used. Might give false-positives. "no" to disable (default) or anything else to enable.
CMAKE_EXTRA_CACHE_FILE
Specifies an extra cache file to pass to cmake. This is the analog of EXTRA_ECONF for econf and is needed to pass TRY_RUN results when cross-compiling. Should be set by user in a per-package basis in /etc/portage/package.env.
CMAKE_QA_SRC_DIR_READONLY
After running cmake_src_prepare, sets ${S} to read-only. This is a user flag and should under _no circumstances_ be set in the ebuild. Helps in improving QA of build systems that write to source tree.
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
cmake.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/cmake.eclass
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
Time: 18:27:02 GMT, October 10, 2020