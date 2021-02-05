CVS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
cvs.eclass - This eclass provides generic cvs fetching functions
DESCRIPTION
This eclass provides the generic cvs fetching functions. To use this from an ebuild, set the ECLASS VARIABLES as specified below in your ebuild before inheriting. Then either leave the default src_unpack or extend over cvs_src_unpack. If you find that you need to call the cvs_* functions directly, I'd be interested to hear about it.
SUPPORTED EAPIS
4 5 6 7
FUNCTIONS
cvs_src_unpack
The cvs src_unpack function, which will be exported
ECLASS VARIABLES
ECVS_CVS_COMPRESS ?= -z1
Set the default compression level. Has no effect when ECVS_CVS_COMMAND is defined by ebuild/user.
ECVS_CVS_OPTIONS ?= "-q -f"
Additional options to the cvs commands. Has no effect when ECVS_CVS_COMMAND is defined by ebuild/user.
ECVS_CVS_COMMAND ?= "cvs ${ECVS_CVS_OPTIONS} ${ECVS_CVS_COMPRESS}"
CVS command to run
You can set, for example, "cvs -t" for extensive debug information on the cvs connection. The default of "cvs -q -f -z4" means to be quiet, to disregard the ~/.cvsrc config file and to use maximum compression.

ECVS_UP_OPTS ?= -dP
CVS options given after the cvs update command. Don't remove "-dP" or things won't work.
ECVS_CO_OPTS
CVS options given after the cvs checkout command.
ECVS_OFFLINE ?= ${EVCS_OFFLINE}
Set this variable to a non-empty value to disable the automatic updating of a CVS source tree. This is intended to be set outside the cvs source tree by users.
ECVS_LOCAL
If this is set, the CVS module will be fetched non-recursively. Refer to the information in the CVS man page regarding the -l command option (not the -l global option).
ECVS_LOCALNAME
Local name of checkout directory
This is useful if the module on the server is called something common like 'driver' or is nested deep in a tree, and you don't like useless empty directories.

WARNING: Set this only from within ebuilds! If set in your shell or some such, things will break because the ebuild won't expect it and have e.g. a wrong $S setting.

ECVS_TOP_DIR ?= "${PORTAGE_ACTUAL_DISTDIR-${DISTDIR}}/cvs-src"
The directory under which CVS modules are checked out.
ECVS_SERVER ?= "offline"
CVS path
The format is "server:/dir", e.g. "anoncvs.kde.org:/home/kde". Remove the other parts of the full CVSROOT, which might look like ":pserver:anonymous@anoncvs.kde.org:/home/kde"; this is generated using other settings also.

Set this to "offline" to disable fetching (i.e. to assume the module is already checked out in ECVS_TOP_DIR).

ECVS_MODULE (REQUIRED)
The name of the CVS module to be fetched
This must be set when cvs_src_unpack is called. This can include several directory levels, i.e. "foo/bar/baz" [[ -z ${ECVS_MODULE} ]] && die "$ECLASS: error: ECVS_MODULE not set, cannot continue"

ECVS_DATE
The date of the checkout. See the -D date_spec option in the cvs man page for more details.
ECVS_BRANCH
The name of the branch/tag to use
The default is "HEAD". The following default _will_ reset your branch checkout to head if used. : ${ECVS_BRANCH:="HEAD"}

ECVS_AUTH ?= "pserver"
Authentication method to use
Possible values are "pserver" and "ext". If `ext' authentication is used, the remote shell to use can be specified in CVS_RSH (SSH is used by default). Currently, the only supported remote shell for `ext' authentication is SSH.

Armando Di Cianno <fafhrd@gentoo.org> 2004/09/27 - Added "no" as a server type, which uses no AUTH method, nor
   does it login
 e.g.
  "cvs -danoncvs@savannah.gnu.org:/cvsroot/backbone co System"
  ( from gnustep-apps/textedit )

ECVS_USER ?= "anonymous"
Username to use for authentication on the remote server.
ECVS_PASS
Password to use for authentication on the remote server
ECVS_SSH_HOST_KEY
If SSH is used for `ext' authentication, use this variable to specify the host key of the remote server. The format of the value should be the same format that is used for the SSH known hosts file.
WARNING: If a SSH host key is not specified using this variable, the remote host key will not be verified.

ECVS_CLEAN
Set this to get a clean copy when updating (passes the -C option to cvs update)
MAINTAINERS
vapier@gentoo.org (and anyone who wants to help)
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
cvs.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/cvs.eclass
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
