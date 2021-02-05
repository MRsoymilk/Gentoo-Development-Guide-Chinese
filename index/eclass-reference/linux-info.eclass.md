LINUX-INFO.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
linux-info.eclass - eclass used for accessing kernel related information
DESCRIPTION
This eclass is used as a central eclass for accessing kernel related information for source or binary already installed. It is vital for linux-mod.eclass to function correctly, and is split out so that any ebuild behaviour "templates" are abstracted out using additional eclasses.
"kernel config" in this file means: The .config of the currently installed sources is used as the first preference, with a fall-back to bundled config (/proc/config.gz) if available.

Before using any of the config-handling functions in this eclass, you must ensure that one of the following functions has been called (in order of preference), otherwise you will get bugs like #364041): linux-info_pkg_setup linux-info_get_any_version get_version get_running_version

FUNCTIONS
set_arch_to_kernel
Set the env ARCH to match what the kernel expects.
set_arch_to_portage
Set the env ARCH to match what portage expects.
getfilevar <variable> <configfile>
It detects the value of the variable defined in the file configfile. This is done by including the configfile, and printing the variable with Make. It WILL break if your makefile has missing dependencies!
Return value: the value of the variable

getfilevar_noexec <variable> <configfile>
It detects the value of the variable defined in the file configfile. This is done with sed matching an expression only. If the variable is defined, you will run into problems. See getfilevar for those cases.
Return value: the value of the variable

linux_config_src_exists
It returns true if .config exists in a build directory otherwise false
Return value: true or false

linux_config_bin_exists
It returns true if .config exists in /proc, otherwise false
Return value: true or false

linux_config_exists
It returns true if .config exists otherwise false
This function MUST be checked before using any of the linux_chkconfig_* functions.

Return value: true or false

linux_config_path
Echo the name of the config file to use. If none are found, then return false.
require_configured_kernel
This function verifies that the current kernel is configured (it checks against the existence of .config) otherwise it dies.
linux_chkconfig_present <option>
It checks that CONFIG_<option>=y or CONFIG_<option>=m is present in the current kernel .config If linux_config_exists returns false, the results of this are UNDEFINED. You MUST call linux_config_exists first.
Return value: true or false

linux_chkconfig_module <option>
It checks that CONFIG_<option>=m is present in the current kernel .config If linux_config_exists returns false, the results of this are UNDEFINED. You MUST call linux_config_exists first.
Return value: true or false

linux_chkconfig_builtin <option>
It checks that CONFIG_<option>=y is present in the current kernel .config If linux_config_exists returns false, the results of this are UNDEFINED. You MUST call linux_config_exists first.
Return value: true or false

linux_chkconfig_string <option>
It prints the CONFIG_<option> value of the current kernel .config (it requires a configured kernel). If linux_config_exists returns false, the results of this are UNDEFINED. You MUST call linux_config_exists first.
Return value: CONFIG_<option>

kernel_is [-lt -gt -le -ge -eq] <major_number> [minor_number patch_number]
It returns true when the current kernel version satisfies the comparison against the passed version. -eq is the default comparison.
For Example where KV = 2.6.9
kernel_is 2 4   returns false
kernel_is 2     returns true
kernel_is 2 6   returns true
kernel_is 2 6 8 returns false
kernel_is 2 6 9 returns true
Return value: true or false

get_version
It gets the version of the kernel inside KERNEL_DIR and populates the KV_FULL variable (if KV_FULL is already set it does nothing).
The kernel version variables (KV_MAJOR, KV_MINOR, KV_PATCH, KV_EXTRA and KV_LOCAL) are also set.

The KV_DIR is set using the KERNEL_DIR env var, the KV_DIR_OUT is set using a valid KBUILD_OUTPUT (in a decreasing priority list, we look for the env var, makefile var or the symlink /lib/modules/${KV_MAJOR}.${KV_MINOR}.${KV_PATCH}${KV_EXTRA}/build).

get_running_version
It gets the version of the current running kernel and the result is the same as get_version() if the function can find the sources.
linux-info_get_any_version
This attempts to find the version of the sources, and otherwise falls back to the version of the running kernel.
check_kernel_built
This function verifies that the current kernel sources have been already prepared otherwise it dies.
check_modules_supported
This function verifies that the current kernel support modules (it checks CONFIG_MODULES=y) otherwise it dies.
check_extra_config
It checks the kernel config options specified by CONFIG_CHECK. It dies only when a required config option (i.e. the prefix ~ is not used) doesn't satisfy the directive. Ignored on non-Linux systems.
linux-info_pkg_setup
Force a get_version() call when inherited from linux-mod.eclass and then check if the kernel is configured to support the options specified in CONFIG_CHECK (if not null)
ECLASS VARIABLES
KERNEL_DIR
A string containing the directory of the target kernel sources. The default value is "/usr/src/linux"
CONFIG_CHECK
A string containing a list of .config options to check for before proceeding with the install.

  e.g.: CONFIG_CHECK="MTRR"

You can also check that an option doesn't exist by prepending it with an exclamation mark (!).


  e.g.: CONFIG_CHECK="!MTRR"

To simply warn about a missing option, prepend a '~'. It may be combined with '!'.

In general, most checks should be non-fatal. The only time fatal checks should be used is for building kernel modules or cases that a compile will fail without the option.

This is to allow usage of binary kernels, and minimal systems without kernel sources.

ERROR_<CFG>
A string containing the error message to display when the check against CONFIG_CHECK fails. <CFG> should reference the appropriate option used in CONFIG_CHECK.

  e.g.: ERROR_MTRR="MTRR exists in the .config but shouldn't!!"

KBUILD_OUTPUT
A string passed on commandline, or set from the kernel makefile. It contains the directory which is to be used as the kernel object directory.
KV_FULL
A read-only variable. It's a string containing the full kernel version. ie: 2.6.9-gentoo-johnm-r1
KV_MAJOR
A read-only variable. It's an integer containing the kernel major version. ie: 2
KV_MINOR
A read-only variable. It's an integer containing the kernel minor version. ie: 6
KV_PATCH
A read-only variable. It's an integer containing the kernel patch version. ie: 9
KV_EXTRA
A read-only variable. It's a string containing the kernel EXTRAVERSION. ie: -gentoo
KV_LOCAL
A read-only variable. It's a string containing the kernel LOCALVERSION concatenation. ie: -johnm
KV_DIR
A read-only variable. It's a string containing the kernel source directory, will be null if KERNEL_DIR is invalid.
KV_OUT_DIR
A read-only variable. It's a string containing the kernel object directory, will be KV_DIR unless KBUILD_OUTPUT is used. This should be used for referencing .config.
AUTHORS
Original author: John Mylchreest <johnm@gentoo.org>
MAINTAINERS
kernel@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
linux-info.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/linux-info.eclass
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
Time: 00:27:03 GMT, October 11, 2020