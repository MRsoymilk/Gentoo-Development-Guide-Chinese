KERNEL-INSTALL.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
kernel-install.eclass - Installation mechanics for Distribution Kernels
DESCRIPTION
This eclass provides the logic needed to test and install different kinds of Distribution Kernel packages, including both kernels built from source and distributed as binaries. The eclass relies on the ebuild installing a subset of built kernel tree into /usr/src/linux-${PV} containing the kernel image in its standard location and System.map.
The eclass exports src_test, pkg_postinst and pkg_postrm. Additionally, the inherited mount-boot eclass exports pkg_pretend. It also stubs out pkg_preinst and pkg_prerm defined by mount-boot.

SUPPORTED EAPIS
7
FUNCTIONS
kernel-install_build_initramfs <output> <version>
Build an initramfs for the kernel. <output> specifies the absolute path where initramfs will be created, while <version> specifies the kernel version, used to find modules.
kernel-install_get_image_path
Get relative kernel image path specific to the current ${ARCH}.
kernel-install_install_kernel <version> <image> <system.map>
Install kernel using installkernel tool. <version> specifies the kernel version, <image> full path to the image, <system.map> full path to System.map.
kernel-install_update_symlink <target> <version>
Update the kernel source symlink at <target> (full path) with a link to <target>-<version> if it's either missing or pointing out to an older version of this package.
kernel-install_get_qemu_arch
Get appropriate qemu suffix for the current ${ARCH}.
kernel-install_create_init <filename>
Create minimal /sbin/init
kernel-install_create_qemu_image <filename>
Create minimal qemu raw image
kernel-install_test <version> <image> <modules>
Test that the kernel can successfully boot a minimal system image in qemu. <version> is the kernel version, <image> path to the image, <modules> path to module tree.
kernel-install_pkg_pretend
Check for missing optional dependencies and output warnings.
kernel-install_src_test
Boilerplate function to remind people to call the tests.
kernel-install_pkg_preinst
Stub out mount-boot.eclass.
kernel-install_pkg_postinst
Build an initramfs for the kernel, install it and update the /usr/src/linux symlink.
kernel-install_pkg_prerm
Stub out mount-boot.eclass.
kernel-install_pkg_postrm
No-op at the moment. Will be used to remove obsolete kernels in the future.
kernel-install_pkg_config
Rebuild the initramfs and reinstall the kernel.
ECLASS VARIABLES
KV_LOCALVERSION
A string containing the kernel LOCALVERSION, e.g. '-gentoo'. Needs to be set only when installing binary kernels, kernel-build.eclass obtains it from kernel config.
AUTHORS
Michał Górny <mgorny@gentoo.org>
MAINTAINERS
Distribution Kernel Project <dist-kernel@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
kernel-install.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/kernel-install.eclass
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
Time: 00:27:02 GMT, October 11, 2020