AUTOTOOLS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
autotools.eclass - Regenerates auto* build scripts
DESCRIPTION
This eclass is for safely handling autotooled software packages that need to regenerate their build scripts. All functions will abort in case of errors.
SUPPORTED EAPIS
0 1 2 3 4 5 6 7
FUNCTIONS
eautoreconf
This function mimes the behavior of autoreconf, but uses the different eauto* functions to run the tools. It doesn't accept parameters, but the directory with include files can be specified with AT_M4DIR variable.
Should do a full autoreconf - normally what most people will be interested in. Also should handle additional directories specified by AC_CONFIG_SUBDIRS.

eaclocal_amflags
Extract the ACLOCAL_AMFLAGS value from the Makefile.am and try to handle (most) of the crazy crap that people throw at us.
eaclocal
These functions runs the autotools using autotools_run_tool with the specified parametes. The name of the tool run is the same of the function without e prefix. They also force installing the support files for safety. Respects AT_M4DIR for additional directories to search for macro's.
_elibtoolize
Runs libtoolize.
Note the '_' prefix: avoid collision with elibtoolize() from libtool.eclass.

eautoheader
Runs autoheader.
eautoconf
Runs autoconf.
eautomake
Runs automake.
eautopoint
Runs autopoint (from the gettext package).
config_rpath_update [destination]
Some packages utilize the config.rpath helper script, but don't use gettext directly. So we have to copy it in manually since we can't let `autopoint` do it for us.
ECLASS VARIABLES
WANT_AUTOCONF ?= latest
The major version of autoconf your package needs
WANT_AUTOMAKE ?= latest
The major version of automake your package needs
WANT_LIBTOOL ?= latest
Do you want libtool? Valid values here are "latest" and "none".
AUTOTOOLS_AUTO_DEPEND ?= yes
Set to 'no' to disable automatically adding to DEPEND. This lets ebuilds form conditional depends by using ${AUTOTOOLS_DEPEND} in their own DEPEND string.
AM_OPTS
Additional options to pass to automake during eautoreconf call.
AT_NOEAUTOMAKE
Don't run eautomake command if set to 'yes'; only used to workaround broken packages. Generally you should, instead, fix the package to not call AM_INIT_AUTOMAKE if it doesn't actually use automake.
AT_NOELIBTOOLIZE
Don't run elibtoolize command if set to 'yes', useful when elibtoolize needs to be ran with particular options
AT_M4DIR
Additional director(y|ies) aclocal should search
MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
autotools.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/autotools.eclass
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
Time: 00:27:02 GMT, October 11, 2020