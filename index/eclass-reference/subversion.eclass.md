SUBVERSION.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
subversion.eclass - Fetch software sources from subversion repositories
DESCRIPTION
The subversion eclass provides functions to fetch, patch and bootstrap software sources from subversion repositories.
SUPPORTED EAPIS
4 5 6 7
FUNCTIONS
subversion_fetch [repo_uri] [destination]
Wrapper function to fetch sources from subversion via svn checkout or svn update, depending on whether there is an existing working copy in ${ESVN_STORE_DIR}.
Can take two optional parameters:
  repo_uri    - a repository URI. default is ESVN_REPO_URI.
  destination - a check out path in S.

subversion_bootstrap
Apply patches in ${ESVN_PATCHES} and run ${ESVN_BOOTSTRAP} if specified. Removed in EAPI 6 and later.
subversion_wc_info [repo_uri]
Get svn info for the specified repo_uri. The default repo_uri is ESVN_REPO_URI.
The working copy information on the specified repository URI are set to ESVN_WC_* variables.

Return value: ESVN_WC_URL, ESVN_WC_ROOT, ESVN_WC_UUID, ESVN_WC_REVISION and ESVN_WC_PATH

subversion_src_unpack
Default src_unpack. Fetch.
subversion_src_prepare
Default src_prepare. Bootstrap. Removed in EAPI 6 and later.
subversion_pkg_preinst [repo_uri]
Log the svn revision of source code. Doing this in pkg_preinst because we want the logs to stick around if packages are uninstalled without messing with config protection.
ECLASS VARIABLES
ESVN_STORE_DIR = "${PORTAGE_ACTUAL_DISTDIR:-${DISTDIR}}/svn-src"
subversion sources store directory. Users may override this in /etc/portage/make.conf
ESVN_FETCH_CMD = "svn checkout"
subversion checkout command
ESVN_UPDATE_CMD = "svn update"
subversion update command
ESVN_SWITCH_CMD = "svn switch"
subversion switch command
ESVN_OPTIONS = "${ESVN_OPTIONS:-}"
the options passed to checkout or update. If you want a specific revision see ESVN_REPO_URI instead of using -rREV.
ESVN_REPO_URI = "${ESVN_REPO_URI:-}"
repository uri
e.g. http://example.org/trunk, svn://example.org/branch/foo@1234

supported URI schemes:
  http://
  https://
  svn://
  svn+ssh://
  file://

to peg to a specific revision, append @REV to the repo's uri

ESVN_REVISION = "${ESVN_REVISION:-}"
User configurable revision checkout or update to from the repository
Useful for live svn or trunk svn ebuilds allowing the user to peg to a specific revision

Note: This should never be set in an ebuild!

ESVN_USER = "${ESVN_USER:-}"
User name
ESVN_PASSWORD = "${ESVN_PASSWORD:-}"
Password
ESVN_PROJECT = "${ESVN_PROJECT:-${PN/-svn}}"
project name of your ebuild (= name space)
subversion eclass will check out the subversion repository like:


  ${ESVN_STORE_DIR}/${ESVN_PROJECT}/${ESVN_REPO_URI##*/}

so if you define ESVN_REPO_URI as http://svn.collab.net/repo/svn/trunk or http://svn.collab.net/repo/svn/trunk/. and PN is subversion-svn. it will check out like:


  ${ESVN_STORE_DIR}/subversion/trunk

this is not used in order to declare the name of the upstream project. so that you can declare this like:


  # jakarta commons-loggin
  ESVN_PROJECT=commons/logging

default: ${PN/-svn}.

ESVN_BOOTSTRAP = "${ESVN_BOOTSTRAP:-}"
Bootstrap script or command like autogen.sh or etc.. Removed in EAPI 6 and later.
ESVN_PATCHES = "${ESVN_PATCHES:-}"
subversion eclass can apply patches in subversion_bootstrap(). you can use regexp in this variable like *.diff or *.patch or etc. NOTE: patches will be applied before ESVN_BOOTSTRAP is processed.
Patches are searched both in ${PWD} and ${FILESDIR}, if not found in either location, the installation dies.

Removed in EAPI 6 and later, use PATCHES instead.

ESVN_RESTRICT = "${ESVN_RESTRICT:-}"
this should be a space delimited list of subversion eclass features to restrict.
  export)
    don't export the working copy to S.
ESVN_OFFLINE = "${ESVN_OFFLINE:-${EVCS_OFFLINE}}"
Set this variable to a non-empty value to disable the automatic updating of an svn source tree. This is intended to be set outside the subversion source tree by users.
ESVN_UMASK = "${ESVN_UMASK:-${EVCS_UMASK}}"
Set this variable to a custom umask. This is intended to be set by users. By setting this to something like 002, it can make life easier for people who do development as non-root (but are in the portage group), and then switch over to building with FEATURES=userpriv. Or vice-versa. Shouldn't be a security issue here as anyone who has portage group write access already can screw the system over in more creative ways.
ESVN_UP_FREQ = "${ESVN_UP_FREQ:=}"
Set the minimum number of hours between svn up'ing in any given svn module. This is particularly useful for split KDE ebuilds where we want to ensure that all submodules are compiled for the same revision. It should also be kept user overrideable.
ESCM_LOGDIR = "${ESCM_LOGDIR:=}"
User configuration variable. If set to a path such as e.g. /var/log/scm any package inheriting from subversion.eclass will record svn revision to ${CATEGORY}/${PN}.log in that path in pkg_preinst. This is not supposed to be set by ebuilds/eclasses. It defaults to empty so users need to opt in.
AUTHORS
Original Author: Akinori Hattori <hattya@gentoo.org>
MAINTAINERS
Akinori Hattori <hattya@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
subversion.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/subversion.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:02 GMT, October 11, 2020