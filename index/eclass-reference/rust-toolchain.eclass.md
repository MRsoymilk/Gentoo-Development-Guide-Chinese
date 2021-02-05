RUST-TOOLCHAIN.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
rust-toolchain.eclass - helps map gentoo arches to rust ABIs
DESCRIPTION
This eclass contains a src_unpack default phase function, and helper functions, to aid in proper rust-ABI handling for various gentoo arches.
SUPPORTED EAPIS
6 7
FUNCTIONS
rust_abi [CHOST-value]
Outputs the Rust ABI name from a CHOST value, uses CHOST in the environment if none is specified.
rust_all_abis
Outputs a list of all the enabled Rust ABIs
rust_arch_uri <rust-ABI> <base-uri> [alt-distfile-basename]
Output the URI for use in SRC_URI, combining $RUST_TOOLCHAIN_BASEURL and the URI suffix provided in ARG2 with the rust ABI in ARG1, and optionally renaming to the distfile basename specified in ARG3.
rust_all_arch_uris <base-uri> [alt-distfile-basename]
Outputs the URIs for SRC_URI to help fetch dependencies, using a base URI provided as an argument. Optionally allows for distfile renaming via a specified basename.
ECLASS VARIABLES
RUST_TOOLCHAIN_BASEURL ?= https://static.rust-lang.org/dist/
This variable specifies the base URL used by the rust_arch_uri and rust_all_arch_uris functions when generating the URI output list.
MAINTAINERS
Rust Project <rust@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
rust-toolchain.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/rust-toolchain.eclass
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
Time: 00:27:04 GMT, October 11, 2020