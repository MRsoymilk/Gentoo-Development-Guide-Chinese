KERNEL-2.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
kernel-2.eclass - Eclass for kernel packages
DESCRIPTION
This is the kernel.eclass rewrite for a clean base regarding the 2.6 series of kernel with back-compatibility for 2.4 Please direct your bugs to the current eclass maintainer :) added functionality: unipatch                - a flexible, singular method to extract, add and remove patches.
SUPPORTED EAPIS
2 3 4 5 6
FUNCTIONS
debug-print-kernel2-variables
this function exists only to help debug kernel-2.eclass if you are adding new functionality in, put a call to it at the start of src_unpack, or during SRC_URI/dep generation.
handle_genpatches [--set-unipatch-list]
add genpatches to list of patches to apply if wanted
detect_version
this function will detect and set - OKV: Original Kernel Version (2.6.0/2.6.0-test11) - KV: Kernel Version (2.6.0-gentoo/2.6.0-test11-gentoo-r1) - EXTRAVERSION: The additional version appended to OKV (-gentoo/-gentoo-r1)
kernel_is <conditional version | version>
user for comparing kernel versions or just identifying a version e.g kernel_is 2 4 e.g kernel_is ge 4.8.11 Note: duplicated in linux-info.eclass
kernel_is_2_4
return true if kernel is version 2.4
kernel_is_2_6
return true if kernel is version 2.6
kernel_header_destdir
return header destination directory
cross_pre_c_headers
set use if necessary for cross compile support
env_setup_xmakeopts
set the ARCH/CROSS_COMPILE when cross compiling
unpack_2_4
unpack and generate .config for 2.4 kernels
unpack_2_6
unpack and generate .config for 2.6 kernels
universal_unpack
unpack kernel sources
unpack_set_extraversion
handle EXTRAVERSION
unpack_fix_install_path
Should be done after patches have been applied Otherwise patches that modify the same area of Makefile will fail
compile_headers
header compilation
compile_headers_tweak_config
some targets can be very very picky, so let's finesse the .config based upon any info we may have
install_universal
Fix permissions in tarball
install_headers
Install headers
install_sources
Install sources
preinst_headers
Headers preinst steps
postinst_sources
Sources post installation function. see inline comments
setup_headers
Determine if ${PN} supports arch
unipatch <list of patches to apply>
Universal function that will apply patches to source
getfilevar <variable> <configfile>
pulled from linux-info
detect_arch
This function sets ARCH_URI and ARCH_PATCH with the neccessary info for the arch sepecific compatibility patchsets.
headers___fix
Voodoo to partially fix broken upstream headers. note: do not put inline/asm/volatile together (breaks "inline asm volatile")
kernel-2_src_unpack
unpack sources, handle genpatches, deblob
kernel-2_src_prepare
Apply any user patches
kernel-2_src_compile
conpile headers or run deblob script
kernel-2_src_test
if you leave it to the default src_test, it will run make to find whether test/check targets are present; since "make test" actually produces a few support files, they are installed even though the package is binchecks-restricted.
Avoid this altogether by making the function moot.

kernel-2_pkg_preinst
if ETYPE = headers, call preinst_headers
kernel-2_src_install
Install headers or sources dependant on ETYPE
kernel-2_pkg_postinst
call postinst_sources for ETYPE = sources
kernel-2_pkg_setup
check for supported kernel version, die if ETYPE is unknown, call setup_headers if necessary
kernel-2_pkg_postrm
Notify the user that after a depclean, there may be sources left behind that need to be manually cleaned
ECLASS VARIABLES
K_USEPV
When setting the EXTRAVERSION variable, it should add PV to the end. this is useful for things like wolk. IE: EXTRAVERSION would be something like : -wolk-4.19-r1
K_NOSETEXTRAVERSION
if this is set then EXTRAVERSION will not be automatically set within the kernel Makefile
K_NOUSENAME
if this is set then EXTRAVERSION will not include the first part of ${PN} in EXTRAVERSION
K_NOUSEPR
if this is set then EXTRAVERSION will not include the anything based on ${PR}.
K_PREPATCHED
if the patchset is prepatched (ie: mm-sources, ck-sources, ac-sources) it will use PR (ie: -r5) as the patchset version for and not use it as a true package revision
K_EXTRAEINFO
this is a new-line seperated list of einfo displays in postinst and can be used to carry additional postinst messages
K_EXTRAELOG
same as K_EXTRAEINFO except using elog instead of einfo
K_EXTRAEWARN
same as K_EXTRAEINFO except using ewarn instead of einfo
K_SYMLINK
if this is set, then forcably create symlink anyway
K_BASE_VER
for git-sources, declare the base version this patch is based off of.
K_DEFCONFIG
Allow specifying a different defconfig target. If length zero, defaults to "defconfig".
K_WANT_GENPATCHES
Apply genpatches to kernel source. Provide any combination of "base", "extras" or "experimental".
K_EXP_GENPATCHES_PULL
If set, we pull "experimental" regardless of the USE FLAG but expect the ebuild maintainer to use K_EXP_GENPATCHES_LIST.
K_EXP_GENPATCHES_NOUSE
If set, no USE flag will be provided for "experimental"; as a result the user cannot choose to apply those patches.
K_EXP_GENPATCHES_LIST
A list of patches to pick from "experimental" to apply when the USE flag is unset and K_EXP_GENPATCHES_PULL is set.
K_FROM_GIT
If set, this variable signals that the kernel sources derives from a git tree and special handling will be applied so that any patches that are applied will actually apply.
K_GENPATCHES_VER
The version of the genpatches tarball(s) to apply. A value of "5" would apply genpatches-2.6.12-5 to my-sources-2.6.12.ebuild
K_SECURITY_UNSUPPORTED
If set, this kernel is unsupported by Gentoo Security to the current eclass maintainer :)
K_DEBLOB_AVAILABLE
A value of "0" will disable all of the optional deblob code. If empty, will be set to "1" if deblobbing is possible. Test ONLY for "1".
K_DEBLOB_TAG
This will be the version of deblob script. It's a upstream SVN tag such asw -gnu or -gnu1.
K_PREDEBLOBBED
This kernel was already deblobbed elsewhere. If false, either optional deblobbing will be available or the license will note the inclusion of linux-firmware code.
K_LONGTERM
If set, the eclass will search for the kernel source in the long term directories on the upstream servers as the location has been changed by upstream
H_SUPPORTEDARCH
this should be a space separated list of ARCH's which can be supported by the headers ebuild
UNIPATCH_LIST
space delimetered list of patches to be applied to the kernel
UNIPATCH_EXCLUDE
An addition var to support exlusion based completely on "<passedstring>*" and not "<passedno#>_*" this should _NOT_ be used from the ebuild as this is reserved for end users passing excludes from the cli
UNIPATCH_DOCS
space delimemeted list of docs to be installed to the doc dir
UNIPATCH_STRICTORDER
if this is set places patches into directories of order, so they are applied in the order passed Changing any other variable in this eclass is not supported; you can request for additional variables to be added by contacting the current maintainer. If you do change them, there is a chance that we will not fix resulting bugs; that of course does not mean we're not willing to help.
AUTHORS
John Mylchreest <johnm@gentoo.org>
Mike Pagano <mpagano@gentoo.org>
<so many, many others, please add yourself>
MAINTAINERS
Gentoo Kernel project <kernel@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
kernel-2.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/kernel-2.eclass
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