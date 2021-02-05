JAVA-VM-2.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
java-vm-2.eclass - Java Virtual Machine eclass
DESCRIPTION
This eclass provides functionality which assists with installing virtual machines, and ensures that they are recognized by java-config.
SUPPORTED EAPIS
5 6
FUNCTIONS
java-vm-2_pkg_setup
default pkg_setup
Initialize vm handle.

java-vm-2_pkg_postinst
default pkg_postinst
Set the generation-2 system VM, if it isn't set or the setting is invalid. Also update mime database.

java-vm-2_pkg_prerm
default pkg_prerm
Warn user if removing system-vm.

java-vm-2_pkg_postrm
default pkg_postrm
Update mime database.

get_system_arch
Get Java specific arch name.
NOTE the mips and sparc values are best guesses. Oracle uses sparcv9 but does OpenJDK use sparc64? We don't support OpenJDK on sparc or any JVM on mips though so it doesn't matter much.

set_java_env
Installs a vm env file. DEPRECATED, use java-vm_install-env instead.
java-vm_install-env
Installs a Java VM environment file. The source can be specified but defaults to ${FILESDIR}/${VMHANDLE}.env.sh.

Environment variables within this file will be resolved. You should escape the $ when referring to variables that should be resolved later such as ${JAVA_HOME}. Subshells may be used but avoid using double quotes. See icedtea-bin.env.sh for a good example.

java-vm_set-pax-markings
Set PaX markings on all JDK/JRE executables to allow code-generation on the heap by the JIT compiler.
The markings need to be set prior to the first invocation of the the freshly built / installed VM. Be it before creating the Class Data Sharing archive or generating cacerts. Otherwise a PaX enabled kernel will kill the VM. Bug #215225 #389751

  Parameters:
    $1 - JDK/JRE base directory.

  Examples:
    java-vm_set-pax-markings "${S}"
    java-vm_set-pax-markings "${ED}"/opt/${P}
java-vm_revdep-mask
Installs a revdep-rebuild control file which SEARCH_DIR_MASK set to the path where the VM is installed. Prevents pointless rebuilds - see bug #177925. Also gives a notice to the user.
  Parameters:
    $1 - Path of the VM (defaults to /opt/${P} if not set)

  Examples:
    java-vm_revdep-mask
    java-vm_revdep-mask /path/to/jdk/

java-vm_sandbox-predict
Install a sandbox control file. Specified paths won't cause a sandbox violation if opened read write but no write takes place. See bug 388937#c1
  Examples:
    java-vm_sandbox-predict /dev/random /proc/self/coredump_filter
ECLASS VARIABLES
JAVA_VM_BUILD_ONLY = "${JAVA_VM_BUILD_ONLY:-FALSE}"
Set to YES to mark a vm as build-only.
MAINTAINERS
java@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
java-vm-2.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/java-vm-2.eclass
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
Time: 00:27:05 GMT, October 11, 2020