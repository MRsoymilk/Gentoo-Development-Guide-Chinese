MERCURIAL.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
mercurial.eclass - This eclass provides generic mercurial fetching functions
DESCRIPTION
This eclass provides generic mercurial fetching functions. To fetch sources from mercurial repository just set EHG_REPO_URI to correct repository URI. If you need to share single repository between several ebuilds set EHG_PROJECT to project name in all of them.
FUNCTIONS
mercurial_fetch [repository_uri] [module] [sourcedir]
Clone or update repository.
If repository URI is not passed it defaults to EHG_REPO_URI, if module is empty it defaults to basename of EHG_REPO_URI, sourcedir defaults to EHG_CHECKOUT_DIR, which defaults to S.

mercurial_src_unpack
The mercurial src_unpack function, which will be exported.
ECLASS VARIABLES
EHG_REPO_URI
Mercurial repository URI.
EHG_REVISION ?= "default"
Create working directory for specified revision, defaults to default.
EHG_REVISION is passed as a value for --updaterev parameter, so it can be more than just a revision, please consult `hg help revisions' for more details.

EHG_STORE_DIR = "${PORTAGE_ACTUAL_DISTDIR:-${DISTDIR}}/hg-src"
Mercurial sources store directory. Users may override this in /etc/portage/make.conf
EHG_PROJECT = "${PN}"
Project name.
This variable default to $PN, but can be changed to allow repository sharing between several ebuilds.

EGIT_CHECKOUT_DIR
The directory to check the hg sources out to.
EHG_CHECKOUT_DIR=${S}

EHG_QUIET ?= "OFF"
Suppress some extra noise from mercurial, set it to 'ON' to be quiet.
EHG_CONFIG
Extra config option to hand to hg clone/pull
EHG_CLONE_CMD = "hg clone ${EHG_CONFIG:+--config ${EHG_CONFIG}} ${EHG_QUIET_CMD_OPT} --pull --noupdate"
Command used to perform initial repository clone.
EHG_PULL_CMD = "hg pull ${EHG_CONFIG:+--config ${EHG_CONFIG}} ${EHG_QUIET_CMD_OPT}"
Command used to update repository.
EHG_OFFLINE = "${EHG_OFFLINE:-${EVCS_OFFLINE}}"
Set this variable to a non-empty value to disable the automatic updating of a mercurial source tree. This is intended to be set outside the ebuild by users.
AUTHORS
Next gen author: Krzysztof Pawlik <nelchael@gentoo.org>
Original author: Aron Griffis <agriffis@gentoo.org>
MAINTAINERS
Christoph Junghans <junghans@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
mercurial.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/mercurial.eclass
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
Time: 00:27:02 GMT, October 11, 2020