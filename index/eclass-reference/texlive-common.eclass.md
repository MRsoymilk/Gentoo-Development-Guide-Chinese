TEXLIVE-COMMON.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
texlive-common.eclass - Provide various functions used by both texlive-core and texlive modules
DESCRIPTION
Purpose: Provide various functions used by both texlive-core and texlive modules.
Note that this eclass *must* not assume the presence of any standard tex too

SUPPORTED EAPIS
7
FUNCTIONS
texlive-common_handle_config_files
Has to be called in src_install after having installed the files in ${D} This function will move the relevant files to /etc/texmf and symling them from their original location. This is to allow easy update of texlive's configuration
texlive-common_is_file_present_in_texmf
Return if a file is present in the texmf tree Call it from the directory containing texmf and texmf-dist
texlive-common_do_symlinks <src> <dest>
Mimic the install_link function of texlinks
Should have the same behavior as the one in /usr/bin/texlinks except that it is under the control of the package manager Note that $1 corresponds to $src and $2 to $dest in this function ( Arguments are switched because texlinks main function sends them switched ) This function should not be called from an ebuild, prefer etexlinks that will also do the fmtutil file parsing.

etexlinks <file>
Mimic texlinks on a fmtutil format file
$1 has to be a fmtutil format file like fmtutil.cnf etexlinks foo will install the symlinks that texlinks --cnffile foo would have created. We cannot use texlinks with portage as it is not DESTDIR aware. (It would not fail but will not create the symlinks if the target is not in the same dir as the source) Also, as this eclass must not depend on a tex distribution to be installed we cannot use texlinks from here.

dobin_texmf_scripts <file1> [file2] ...
Symlinks a script from the texmf tree to /usr/bin. Requires permissions to be correctly set for the file that it will point to.
etexmf-update
Runs texmf-update if it is available and prints a warning otherwise. This function helps in factorizing some code. Useful in ebuilds' pkg_postinst and pkg_postrm phases.
efmtutil-sys
Runs fmtutil-sys if it is available and prints a warning otherwise. This function helps in factorizing some code. Used in ebuilds' pkg_postinst to force a rebuild of TeX formats.
AUTHORS
Original Author: Alexis Ballier <aballier@gentoo.org>
MAINTAINERS
tex@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
texlive-common.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/texlive-common.eclass
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