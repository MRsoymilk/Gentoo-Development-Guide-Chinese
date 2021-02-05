MULTILIB.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
multilib.eclass - This eclass is for all functions pertaining to handling multilib configurations.
DESCRIPTION
This eclass is for all functions pertaining to handling multilib configurations.
FUNCTIONS
has_multilib_profile
Return true if the current profile is a multilib profile and lists more than one abi in ${MULTILIB_ABIS}. When has_multilib_profile returns true, that profile should enable the 'multilib' use flag. This is so you can DEPEND on a package only for multilib or not multilib.
get_libdir
This function simply returns the desired lib directory. With portage 2.0.51, we now have support for installing libraries to lib32/lib64 to accomidate the needs of multilib systems. It's no longer a good idea to assume all libraries will end up in lib. Replace any (sane) instances where lib is named directly with $(get_libdir) if possible.
Jeremy Huddleston <eradicator@gentoo.org> (23 Dec 2004):
  Added support for ${ABI} and ${DEFAULT_ABI}.  If they're both not set,
  fall back on old behavior.  Any profile that has these set should also
  depend on a newer version of portage (not yet released) which uses these
  over CONF_LIBDIR in econf, dolib, etc...

Return value: the libdir for the selected ABI

get_abi_CFLAGS [ABI]
Alias for 'get_abi_var CFLAGS'
get_abi_LDFLAGS [ABI]
Alias for 'get_abi_var LDFLAGS'
get_abi_CHOST [ABI]
Alias for 'get_abi_var CHOST'
get_abi_CTARGET [ABI]
Alias for 'get_abi_var CTARGET'
get_abi_FAKE_TARGETS [ABI]
Alias for 'get_abi_var FAKE_TARGETS'
get_abi_LIBDIR [ABI]
Alias for 'get_abi_var LIBDIR'
get_install_abis
Return a list of the ABIs we want to install for with the last one in the list being the default.
get_all_abis
Return a list of the ABIs supported by this profile. the last one in the list being the default.
get_all_libdirs
Returns a list of all the libdirs used by this profile. This includes those that might not be touched by the current ebuild and always includes "lib".
is_final_abi
Return true if ${ABI} is the last ABI on our list (or if we're not using the new multilib configuration. This can be used to determine if we're in the last (or only) run through src_{unpack,compile,install}
number_abis
echo the number of ABIs we will be installing for
get_exeext
Returns standard executable program suffix (null, .exe, etc.) for the current platform identified by CHOST.
Example:
    get_exeext
    Returns: null string (almost everywhere) || .exe (mingw*) || ...

get_libname [version]
Returns libname with proper suffix {.so,.dylib,.dll,etc} and optionally supplied version for the current platform identified by CHOST.
Example:
    get_libname ${PV}
    Returns: .so.${PV} (ELF) || .${PV}.dylib (MACH) || ...

get_modname
Returns modulename with proper suffix {.so,.bundle,etc} for the current platform identified by CHOST.
Example:
    libfoo$(get_modname)
    Returns: libfoo.so (ELF) || libfoo.bundle (MACH) || ...

multilib_toolchain_setup
Hide multilib details here for packages which are forced to be compiled for a specific ABI when run on another ABI (like x86-specific packages on amd64)
MAINTAINERS
amd64@gentoo.org
toolchain@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
multilib.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/multilib.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:02 GMT, October 11, 2020
