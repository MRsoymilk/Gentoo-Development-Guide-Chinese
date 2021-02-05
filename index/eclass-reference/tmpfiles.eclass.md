TMPFILES.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
tmpfiles.eclass - Functions related to tmpfiles.d files
DESCRIPTION
This eclass provides functionality related to installing and creating volatile and temporary files based on configuration files$and locations defined at this URL:
https://www.freedesktop.org/software/systemd/man/tmpfiles.d.html

The dotmpfiles and newtmpfiles functions are used to install configuration files into /usr/lib/tmpfiles.d, then in pkg_postinst, the tmpfiles_process function must be called to process the newly installed tmpfiles.d entries.

The tmpfiles.d files can be used by service managers to recreate/clean up temporary directories on boot or periodically. Additionally, the pkg_postinst() call ensures that the directories are created on systems that do not support tmpfiles.d natively, without a need for explicit fallback.

SUPPORTED EAPIS
5 6 7
EXAMPLE
Typical usage of this eclass:
EAPI=6
inherit tmpfiles

...

src_install() {
        ...
        dotmpfiles "${FILESDIR}"/file1.conf "${FILESDIR}"/file2.conf
        newtmpfiles "${FILESDIR}"/file3.conf-${PV} file3.conf
        ...
}

pkg_postinst() {
        ...
        tmpfiles_process file1.conf file2.conf file3.conf
        ...
}

FUNCTIONS
dotmpfiles <tmpfiles.d_file> ...
Install one or more tmpfiles.d files into /usr/lib/tmpfiles.d.
newtmpfiles <old-name> <new-name>.conf
Install a tmpfiles.d file in /usr/lib/tmpfiles.d under a new name.
tmpfiles_process <filename> <filename> ...
Call a tmpfiles.d implementation to create new volatile and temporary files and directories.
AUTHORS
Mike Gilbert <floppym@gentoo.org>
William Hubbs <williamh@gentoo.org>
MAINTAINERS
Gentoo systemd project <systemd@gentoo.org>
William Hubbs <williamh@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
tmpfiles.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/tmpfiles.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
EXAMPLE
FUNCTIONS
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020