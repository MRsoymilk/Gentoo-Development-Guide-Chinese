EMBOSS-R2.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
emboss-r2.eclass - Use this to easy install EMBOSS and EMBASSY programs (EMBOSS add-ons).
DESCRIPTION
The inheriting ebuild must set at least EAPI=6 and provide EBO_DESCRIPTION before the inherit line. KEYWORDS should be set. Additionally "(R|P)DEPEND"encies and other standard ebuild variables can be extended (FOO+=" bar").
Example:

EAPI=6

EBO_DESCRIPTION="applications from the CBS group"

inherit emboss-r2

SUPPORTED EAPIS
6
FUNCTIONS
emboss-r2_src_prepare
Does the following things

 1. Renames configure.in to configure.ac, if possible
 2. Calls default_src_prepare (i.e.
    applies Gentoo and user patches in EAPI>=6)
 3. If EBO_EAUTORECONF is set, run eautoreconf

emboss-r2_src_configure
runs econf with following options.

 --enable-shared
 $(use_enable static-libs static)
 $(use_with X x)
 $(use_with png pngdriver)
 $(use_with pdf hpdf)
 $(use_with mysql mysql)
 $(use_with postgres postgresql)
 --enable-large
 --without-java
 --enable-systemlibs


 can be appended to like econf, e.g.
   emboss-r2_src_configure --disable-shared

emboss-r2_src_install
Installs the package into the staging area and removes extraneous .la files, if USE="-static-libs"
ECLASS VARIABLES
EBO_DESCRIPTION
Should be set. Completes the generic description of the embassy module as follows:
EMBOSS integrated version of ${EBO_DESCRIPTION}, e.g.

"EMBOSS integrated version of applications from the CBS group"

Defaults to the upstream name of the module.

EBO_EAUTORECONF
If set, run eautoreconf from autotools.eclass after applying patches in emboss-r2_src_prepare.
AUTHORS
Original author: Author Olivier Fisette <ofisette@gmail.com>
Next gen author: Justin Lecher <jlec@gentoo.org>
Next gen author: Ted Tanberry <ted.tanberry@gmail.com>
MAINTAINERS
sci-biology@gentoo.org
ted.tanberry@gmail.com
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
emboss-r2.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/emboss-r2.eclass
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
Time: 00:27:05 GMT, October 11, 2020
