SAVEDCONFIG.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
savedconfig.eclass - common API for saving/restoring complex configuration files
DESCRIPTION
It is not uncommon to come across a package which has a very fine grained level of configuration options that go way beyond what USE flags can properly describe. For this purpose, a common API of saving and restoring the configuration files was developed so users can modify these config files and the ebuild will take it into account as needed.
Typically you can create your own configuration files quickly by doing:

1. Build the package with FEATURES=noclean USE=savedconfig.

2. Go into the build dir and edit the relevant configuration system (e.g. `make menuconfig` or `nano config-header.h`). You can look at the files in /etc/portage/savedconfig/ to see what files get loaded/restored.

3. Copy the modified configuration files out of the workdir and to the paths in /etc/portage/savedconfig/.

4. Emerge the package with just USE=savedconfig to get the custom build.

SUPPORTED EAPIS
5 6 7
FUNCTIONS
save_config <config files to save>
Use this function to save the package's configuration file into the right location. You may specify any number of configuration files, but just make sure you call save_config with all of them at the same time in order for things to work properly.
restore_config <config files to restore>
Restores the package's configuration file probably with user edits. You can restore a single file or a whole bunch, just make sure you call restore_config with all of the files to restore at the same time.
Config files can be laid out as:

${PORTAGE_CONFIGROOT}/etc/portage/savedconfig/${CTARGET}/${CATEGORY}/${PF}
${PORTAGE_CONFIGROOT}/etc/portage/savedconfig/${CHOST}/${CATEGORY}/${PF}
${PORTAGE_CONFIGROOT}/etc/portage/savedconfig/${CATEGORY}/${PF}
${PORTAGE_CONFIGROOT}/etc/portage/savedconfig/${CTARGET}/${CATEGORY}/${P}
${PORTAGE_CONFIGROOT}/etc/portage/savedconfig/${CHOST}/${CATEGORY}/${P}
${PORTAGE_CONFIGROOT}/etc/portage/savedconfig/${CATEGORY}/${P}
${PORTAGE_CONFIGROOT}/etc/portage/savedconfig/${CTARGET}/${CATEGORY}/${PN}
${PORTAGE_CONFIGROOT}/etc/portage/savedconfig/${CHOST}/${CATEGORY}/${PN}
${PORTAGE_CONFIGROOT}/etc/portage/savedconfig/${CATEGORY}/${PN}
MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
savedconfig.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/savedconfig.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:05 GMT, October 11, 2020