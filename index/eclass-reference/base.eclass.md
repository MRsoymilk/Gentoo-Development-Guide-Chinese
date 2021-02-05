BASE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
base.eclass - The base eclass defines some default functions and variables.
DESCRIPTION
The base eclass defines some default functions and variables.
SUPPORTED EAPIS
0 1 2 3 4 5
FUNCTIONS
base_src_unpack
The base src_unpack function, which is exported. Calls also src_prepare with eapi older than 2.
base_src_prepare
The base src_prepare function, which is exported EAPI is greater or equal to 2. Here the PATCHES array is evaluated.
base_src_configure
The base src_configure function, which is exported when EAPI is greater or equal to 2. Runs basic econf.
base_src_compile
The base src_compile function, calls src_configure with EAPI older than 2.
base_src_make
Actual function that runs emake command.
base_src_install
The base src_install function. Runs make install and installs documents and html documents from DOCS and HTML_DOCS arrays.
base_src_install_docs
Actual function that install documentation from DOCS and HTML_DOCS arrays.
ECLASS VARIABLES
DOCS
Array containing documents passed to dodoc command.
DOCS=( "${S}/doc/document.txt" "${S}/doc/doc_folder/" )

HTML_DOCS
Array containing documents passed to dohtml command.
HTML_DOCS=( "${S}/doc/document.html" "${S}/doc/html_folder/" )

PATCHES
PATCHES array variable containing all various patches to be applied. This variable is expected to be defined in global scope of ebuild. Make sure to specify the full path. This variable is utilised in src_unpack/src_prepare phase based on EAPI.
NOTE: if using patches folders with special file suffixes you have to define one additional variable EPATCH_SUFFIX="something"

PATCHES=( "${FILESDIR}/mypatch.patch" "${FILESDIR}/patches_folder/" )

AUTHORS
Original author: Dan Armak <danarmak@gentoo.org>
MAINTAINERS
QA Team <qa@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
base.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/base.eclass
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
Time: 00:27:01 GMT, October 11, 2020