MULTILIB-BUILD.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
multilib-build.eclass - flags and utility functions for building multilib packages
DESCRIPTION
The multilib-build.eclass exports USE flags and utility functions necessary to build packages for multilib in a clean and uniform manner.
Please note that dependency specifications for multilib-capable dependencies shall use the USE dependency string in ${MULTILIB_USEDEP} to properly request multilib enabled.

SUPPORTED EAPIS
4 5 6 7
FUNCTIONS
multilib_get_enabled_abis
Return the ordered list of enabled ABIs if multilib builds are enabled. The best (most preferred) ABI will come last.
If multilib is disabled, the default ABI will be returned in order to enforce consistent testing with multilib code.

multilib_get_enabled_abi_pairs
Return the ordered list of enabled <use-flag>.<ABI> pairs if multilib builds are enabled. The best (most preferred) ABI will come last.
If multilib is disabled, the default ABI will be returned along with empty <use-flag>.

multilib_foreach_abi <argv>...
If multilib support is enabled, sets the toolchain up for each supported ABI along with the ABI variable and correct BUILD_DIR, and runs the given commands with them.
If multilib support is disabled, it just runs the commands. No setup is done.

multilib_parallel_foreach_abi <argv>...
If multilib support is enabled, sets the toolchain up for each supported ABI along with the ABI variable and correct BUILD_DIR, and runs the given commands with them.
If multilib support is disabled, it just runs the commands. No setup is done.

This function used to run multiple commands in parallel. Now it's just a deprecated alias to multilib_foreach_abi.

multilib_for_best_abi <argv>...
Runs the given command with setup for the 'best' (usually native) ABI.
multilib_check_headers
Check whether the header files are consistent between ABIs.
This function needs to be called after each ABI's installation phase. It obtains the header file checksums and compares them with previous runs (if any). Dies if header files differ.

multilib_copy_sources
Create a single copy of the package sources for each enabled ABI.
The sources are always copied from initial BUILD_DIR (or S if unset) to ABI-specific build directory matching BUILD_DIR used by multilib_foreach_abi().

multilib_prepare_wrappers [<install-root>]
Perform the preparation of all kinds of wrappers for the current ABI. This function shall be called once per each ABI, after installing the files to be wrapped.
Takes an optional custom <install-root> from which files will be used. If no root is specified, uses ${ED}.

The files to be wrapped are specified using separate variables, e.g. MULTILIB_WRAPPED_HEADERS. Those variables shall not be changed between the successive calls to multilib_prepare_wrappers and multilib_install_wrappers.

After all wrappers are prepared, multilib_install_wrappers shall be called to commit them to the installation tree.

multilib_install_wrappers [<install-root>]
Install the previously-prepared wrappers. This function shall be called once, after all wrappers were prepared.
Takes an optional custom <install-root> to which the wrappers will be installed. If no root is specified, uses ${ED}. There is no need to use the same root as when preparing the wrappers.

The files to be wrapped are specified using separate variables, e.g. MULTILIB_WRAPPED_HEADERS. Those variables shall not be changed between the calls to multilib_prepare_wrappers and multilib_install_wrappers.

multilib_is_native_abi
Determine whether the currently built ABI is the profile native. Return true status (0) if that is true, otherwise false (1).
multilib_build_binaries
Deprecated synonym for multilib_is_native_abi
multilib_native_use_with <flag> [<opt-name> [<opt-value>]]
Output --with configure option alike use_with if USE <flag> is enabled and executables are being built (multilib_is_native_abi is true). Otherwise, outputs --without configure option. Arguments are the same as for use_with in the EAPI.
multilib_native_use_enable <flag> [<opt-name> [<opt-value>]]
Output --enable configure option alike use_enable if USE <flag> is enabled and executables are being built (multilib_is_native_abi is true). Otherwise, outputs --disable configure option. Arguments are the same as for use_enable in the EAPI.
multilib_native_enable <opt-name> [<opt-value>]
Output --enable configure option if executables are being built (multilib_is_native_abi is true). Otherwise, output --disable configure option.
multilib_native_with <opt-name> [<opt-value>]
Output --with configure option if executables are being built (multilib_is_native_abi is true). Otherwise, output --without configure option.
multilib_native_usex <flag> [<true1> [<false1> [<true2> [<false2>]]]]
Output the concatenation of <true1> (or 'yes' if unspecified) and <true2> if USE <flag> is enabled and executables are being built (multilib_is_native_abi is true). Otherwise, output the concatenation of <false1> (or 'no' if unspecified) and <false2>. Arguments are the same as for usex in the EAPI.
Note: in EAPI 4 you need to inherit eutils to use this function.

ECLASS VARIABLES
MULTILIB_COMPAT
List of multilib ABIs supported by the ebuild. If unset, defaults to all ABIs supported by the eclass.
This variable is intended for use in prebuilt multilib packages that can provide binaries only for a limited set of ABIs. If ABIs need to be limited due to a bug in source code, package.use.mask is to be used instead. Along with MULTILIB_COMPAT, KEYWORDS should contain '-*'.

Note that setting this variable effectively disables support for all other ABIs, including other architectures. For example, specifying abi_x86_{32,64} disables support for MIPS as well.

The value of MULTILIB_COMPAT determines the value of IUSE. If set, it also enables REQUIRED_USE constraints.

Example use:

# Upstream provides binaries for x86 & amd64 only
MULTILIB_COMPAT=( abi_x86_{32,64} )
MULTILIB_USEDEP (GENERATED BY ECLASS)
The USE-dependency to be used on dependencies (libraries) needing to support multilib as well.
Example use:

RDEPEND="dev-libs/libfoo[${MULTILIB_USEDEP}]
net-libs/libbar[ssl,${MULTILIB_USEDEP}]"
MULTILIB_ABI_FLAG (GENERATED BY ECLASS)
The complete ABI name. Resembles the USE flag name.
This is set within multilib_foreach_abi(), multilib_parallel_foreach_abi() and multilib-minimal sub-phase functions.

It may be null (empty) when the build is done on ABI not controlled by a USE flag (e.g. on non-multilib arch or when using multilib portage). The build will always be done for a single ABI then.

Example value:

abi_x86_64
MULTILIB_WRAPPED_HEADERS
A list of headers to wrap for multilib support. The listed headers will be moved to a non-standard location and replaced with a file including them conditionally to current ABI.
This variable has to be a bash array. Paths shall be relative to installation root (${ED}), and name regular files. Recursive wrapping is not supported.

Please note that header wrapping is *discouraged*. It is preferred to install all headers in a subdirectory of libdir and use pkg-config to locate the headers. Some C preprocessors will not work with wrapped headers.

Example:

MULTILIB_WRAPPED_HEADERS=(
/usr/include/foobar/config.h
)
MULTILIB_CHOST_TOOLS
A list of tool executables to preserve for each multilib ABI. The listed executables will be renamed to ${CHOST}-${basename}, and the native variant will be symlinked to the generic name.
This variable has to be a bash array. Paths shall be relative to installation root (${ED}), and name regular files or symbolic links to regular files. Recursive wrapping is not supported.

If symbolic link is passed, both symlink path and symlink target will be changed. As a result, the symlink target is expected to be wrapped as well (either by listing in MULTILIB_CHOST_TOOLS or externally).

Please note that tool wrapping is *discouraged*. It is preferred to install pkg-config files for each ABI, and require reverse dependencies to use that.

Packages that search for tools properly (e.g. using AC_PATH_TOOL macro) will find the wrapper executables automatically. Other packages will need explicit override of tool paths.

Example:

MULTILIB_CHOST_TOOLS=(
/usr/bin/foo-config
)
AUTHORS
Author: Michał Górny <mgorny@gentoo.org>
MAINTAINERS
gx86-multilib team <multilib@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
multilib-build.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/multilib-build.eclass
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