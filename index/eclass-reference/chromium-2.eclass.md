CHROMIUM-2.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
chromium-2.eclass - Shared functions for chromium and google-chrome
FUNCTIONS
chromium_suid_sandbox_check_kernel_config
Ensures the system kernel supports features needed for SUID sandbox to work.
chromium_remove_language_paks
Removes pak files from the current directory for languages that the user has not selected via the L10N variable. Also performs QA checks to ensure CHROMIUM_LANGS has been set correctly.
EGYP_CHROMIUM_COMMAND ?= build/gyp_chromium
Path to the gyp_chromium script.
EGYP_CHROMIUM_DEPTH ?= .
Depth for egyp_chromium.
egyp_chromium [gyp arguments]
Calls EGYP_CHROMIUM_COMMAND with depth EGYP_CHROMIUM_DEPTH and given arguments. The full command line is echoed for logging.
gyp_use <USE flag> [GYP flag] [true suffix] [false suffix]
If USE flag is set, echo -D[GYP flag]=[true suffix].
If USE flag is not set, echo -D[GYP flag]=[false suffix].

[GYP flag] defaults to use_[USE flag] with hyphens converted to underscores.

[true suffix] defaults to 1. [false suffix] defaults to 0.

ECLASS VARIABLES
CHROMIUM_LANGS
List of language packs available for this package.
AUTHORS
Mike Gilbert <floppym@gentoo.org>
MAINTAINERS
Chromium Herd <chromium@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
chromium-2.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/chromium-2.eclass
Index
NAME
FUNCTIONS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020