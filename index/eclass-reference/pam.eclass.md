PAM.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
pam.eclass - Handles pam related tasks
DESCRIPTION
This eclass contains functions to install pamd configuration files and pam modules.
FUNCTIONS
dopamd <file> [more files]
Install pam auth config file in /etc/pam.d
newpamd <old name> <new name>
Install pam file <old name> as <new name> in /etc/pam.d
dopamsecurity <section> <file> [more files]
Installs the config files in /etc/security/<section>/
newpamsecurity <section> <old name> <new name>
Installs the config file <old name> as <new name> in /etc/security/<section>/
getpam_mod_dir
Returns the pam modules' directory for current implementation
pammod_hide_symbols
Hide all non-PAM-used symbols from the module; this function creates a simple ld version script that hides all the symbols that are not necessary for PAM to load the module, then uses append-flags to make sure that it gets used.
dopammod <file> [more files]
Install pam module file in the pam modules' dir for current implementation
newpammod <old name> <new name>
Install pam module file <old name> as <new name> in the pam modules' dir for current implementation
pamd_mimic_system <pamd file> [auth levels]
This function creates a pamd file which mimics system-auth file for the given levels in the /etc/pam.d directory.
pamd_mimic <stack> <pamd file> [auth levels]
This function creates a pamd file which mimics the given stack for the given levels in the /etc/pam.d directory.
cleanpamd <pamd file>
Cleans a pam.d file from modules that might not be present on the system where it's going to be installed
pam_epam_expand <pamd file>
Steer clear, deprecated, don't use, bad experiment
AUTHORS
Diego Pettenò <flameeyes@gentoo.org>
MAINTAINERS
Mikle Kolyada <zlogene@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
pam.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/pam.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:02 GMT, October 11, 2020
