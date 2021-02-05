CRON.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
cron.eclass - Some functions for cron
DESCRIPTION
Purpose: The main motivation for this eclass was to simplify the jungle known as src_install() in cron ebuilds. Using these functions also ensures that permissions are *always* reset, preventing the accidental installation of files with wrong perms.
NOTE on defaults: the default settings in the below functions were chosen based on the most common setting among cron ebuilds.

Please assign any bugs regarding this eclass to cron-bugs@gentoo.org.

FUNCTIONS
docrondir [ dir ] [ perms ]
Creates crontab directory
Both arguments are optional. Everything after 'dir' is considered
  the permissions (same format as insopts).

ex: docrondir /some/dir -m 0770 -o root -g cron
    docrondir /some/dir (uses default perms)
    docrondir -m0700 (uses default dir)

docron [ exe ] [ perms ]
Install cron executable

   Both arguments are optional.

ex: docron -m 0700 -o root -g root ('exe' defaults to "cron")
    docron crond -m 0110

docrontab [ exe ] [ perms ]
Install crontab executable

  Uses same semantics as docron.

cron_pkg_postinst
Outputs a message about system crontabs daemons that have a true system crontab set CRON_SYSTEM_CRONTAB="yes"
AUTHORS
Original Author: Aaron Walker <ka0ttic@gentoo.org>
MAINTAINERS
maintainer-needed@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
cron.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/cron.eclass
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
Time: 00:27:01 GMT, October 11, 2020