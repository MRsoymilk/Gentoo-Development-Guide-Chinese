LINUX-MOD.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
linux-mod.eclass - It provides the functionality required to install external modules against a kernel source tree.
DESCRIPTION
This eclass is used to interface with linux-info.eclass in such a way to provide the functionality and initial functions required to install external modules against a kernel source tree.
FUNCTIONS
use_m
It checks if the kernel version is greater than 2.6.5.
Return value: true or false

convert_to_m </path/to/the/file>
It converts a file (e.g. a makefile) to use M= instead of SUBDIRS=
set_kvobj
It sets the KV_OBJ variable.
linux-mod_pkg_setup
It checks the CONFIG_CHECK options (see linux-info.eclass(5)), verifies that the kernel is configured, verifies that the sources are prepared, verifies that the modules support is builtin in the kernel and sets the object extension KV_OBJ.
linux-mod_pkg_setup_binary
Perform all kernel option checks non-fatally, as the .config and /proc/config.gz might not be present. Do not do anything that requires kernel sources.
linux-mod_src_compile
It compiles all the modules specified in MODULE_NAMES. For each module the econf command is executed only if ECONF_PARAMS is defined, the name of the target is specified by BUILD_TARGETS while the options are in BUILD_PARAMS (all the modules share these variables). The compilation happens inside ${srcdir}.
Look at the description of these variables for more details.

linux-mod_src_install
It install the modules specified in MODULES_NAME. The modules should be inside the ${objdir} directory and they are installed inside /lib/modules/${KV_FULL}/${libdir}.
The modprobe.d configuration file is automatically generated if the MODULESD_<modulename>_* variables are defined. The only way to stop this process is by setting MODULESD_<modulename>_ENABLED=no. At the end the documentation specified via MODULESD_<modulename>_DOCS is also installed.

Look at the description of these variables for more details.

linux-mod_pkg_preinst
It checks what to do after having merged the package.
linux-mod_pkg_postinst
It executes /sbin/depmod and adds the package to the /var/lib/module-rebuild/moduledb database (if ${D}/lib/modules is created)"
linux-mod_pkg_postrm
It removes the package from the /var/lib/module-rebuild/moduledb database but it doens't call /sbin/depmod because the modules are still installed.
ECLASS VARIABLES
MODULES_OPTIONAL_USE
A string containing the USE flag to use for making this eclass optional The recommended non-empty value is 'modules'
MODULES_OPTIONAL_USE_IUSE_DEFAULT
A boolean to control the IUSE default state for the MODULES_OPTIONAL_USE USE flag. Default value is unset (false). True represented by 1 or 'on', other values including unset treated as false.
KERNEL_DIR
A string containing the directory of the target kernel sources. The default value is "/usr/src/linux"
ECONF_PARAMS
It's a string containing the parameters to pass to econf. If this is not set, then econf isn't run.
BUILD_PARAMS
It's a string with the parameters to pass to emake.
BUILD_TARGETS
It's a string with the build targets to pass to make. The default value is "clean module"
MODULE_NAMES
It's a string containing the modules to be built automatically using the default src_compile/src_install. It will only make ${BUILD_TARGETS} once in any directory.
The structure of each MODULE_NAMES entry is as follows:


  modulename(libdir:srcdir:objdir)

where:


  modulename = name of the module file excluding the .ko
  libdir     = place in system modules directory where module is installed (by default it's misc)
  srcdir     = place for ebuild to cd to before running make (by default it's ${S})
  objdir     = place the .ko and objects are located after make runs (by default it's set to srcdir)

To get an idea of how these variables are used, here's a few lines of code from around line 540 in this eclass:

einfo "Installing ${modulename} module" cd ${objdir} || die "${objdir} does not exist" insinto /lib/modules/${KV_FULL}/${libdir} doins ${modulename}.${KV_OBJ} || die "doins ${modulename}.${KV_OBJ} failed"

For example:
  MODULE_NAMES="module_pci(pci:${S}/pci:${S}) module_usb(usb:${S}/usb:${S})"

what this would do is


  cd "${S}"/pci
  make ${BUILD_PARAMS} ${BUILD_TARGETS}
  cd "${S}"
  insinto /lib/modules/${KV_FULL}/pci
  doins module_pci.${KV_OBJ}


  cd "${S}"/usb
  make ${BUILD_PARAMS} ${BUILD_TARGETS}
  cd "${S}"
  insinto /lib/modules/${KV_FULL}/usb
  doins module_usb.${KV_OBJ}

MODULESD_<modulename>_ENABLED
This is used to disable the modprobe.d file generation otherwise the file will be always generated (unless no MODULESD_<modulename>_* variable is provided). Set to "no" to disable the generation of the file and the installation of the documentation.
MODULESD_<modulename>_EXAMPLES
This is a bash array containing a list of examples which should be used. If you want us to try and take a guess set this to "guess".
For each array_component it's added an options line in the modprobe.d file


  options array_component

where array_component is "<modulename> options" (see modprobe.conf(5))

MODULESD_<modulename>_ALIASES
This is a bash array containing a list of associated aliases.
For each array_component it's added an alias line in the modprobe.d file


  alias array_component

where array_component is "wildcard <modulename>" (see modprobe.conf(5))

MODULESD_<modulename>_ADDITIONS
This is a bash array containing a list of additional things to add to the bottom of the file. This can be absolutely anything. Each entry is a new line.
MODULESD_<modulename>_DOCS
This is a string list which contains the full path to any associated documents for <modulename>. These files are installed in the live tree.
KV_OBJ
It's a read-only variable. It contains the extension of the kernel modules.
AUTHORS
John Mylchreest <johnm@gentoo.org>,
Stefan Schweizer <genstef@gentoo.org>
MAINTAINERS
kernel@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
linux-mod.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/linux-mod.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020