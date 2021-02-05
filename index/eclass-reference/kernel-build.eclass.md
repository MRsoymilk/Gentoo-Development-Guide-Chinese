KERNEL-BUILD.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
kernel-build.eclass - Build mechanics for Distribution Kernels
DESCRIPTION
This eclass provides the logic to build a Distribution Kernel from source and install it. Post-install and test logic is inherited from kernel-install.eclass.
The ebuild must take care of unpacking the kernel sources, copying an appropriate .config into them (e.g. in src_prepare()) and setting correct S. The eclass takes care of respecting savedconfig, building the kernel and installing it along with its modules and subset of sources needed to build external modules.

SUPPORTED EAPIS
7
FUNCTIONS
kernel-build_src_configure
Prepare the toolchain for building the kernel, get the default .config or restore savedconfig, and get build tree configured for modprep.
kernel-build_src_compile
Compile the kernel sources.
kernel-build_src_test
Test the built kernel via qemu. This just wraps the logic from kernel-install.eclass with the correct paths.
kernel-build_src_install
Install the built kernel along with subset of sources into /usr/src/linux-${PV}. Install the modules. Save the config.
kernel-build_pkg_postinst
Combine postinst from kernel-install and savedconfig eclasses.
AUTHORS
Michał Górny <mgorny@gentoo.org>
MAINTAINERS
Distribution Kernel Project <dist-kernel@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
kernel-build.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/kernel-build.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:02 GMT, October 11, 2020