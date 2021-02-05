PREFIX.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
prefix.eclass - Eclass to provide Prefix functionality
DESCRIPTION
Gentoo Prefix allows users to install into a self defined offset located somewhere in the filesystem. Prefix ebuilds require additional functions and variables which are defined by this eclass.
FUNCTIONS
eprefixify <list of to be eprefixified files>
replaces @GENTOO_PORTAGE_EPREFIX@ with ${EPREFIX} for the given files, dies if no arguments are given, a file does not exist, or changing a file failed.
hprefixify [ -w <line match> ] [ -e <extended regex> ] [ -q <quotation char> ] <list of files>
Tries a set of heuristics to prefixify the given files. Dies if no arguments are given, a file does not exist, or changing a file failed.
Additional extended regular expression can be passed by -e or environment variable PREFIX_EXTRA_REGEX. The default heuristics can be constrained to lines that match a sed expression passed by -w or environment variable PREFIX_LINE_MATCH. Quotation characters can be specified by -q or environment variable PREFIX_QUOTE_CHAR, unless EPREFIX is empty.

prefixify_ro <file>
prefixify a read-only file. copies the files to ${T}, prefixies it, echos the new file.
ECLASS VARIABLES
EPREFIX
The offset prefix of a Gentoo Prefix installation. When Gentoo Prefix is not used, ${EPREFIX} should be "". Prefix Portage sets EPREFIX, hence this eclass has nothing to do here in that case. Note that setting EPREFIX in the environment with Prefix Portage sets Portage into cross-prefix mode.
MAINTAINERS
Feel free to contact the Prefix team through <prefix@gentoo.org> if
you have problems, suggestions or questions.
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
prefix.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/prefix.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
ECLASS VARIABLES
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020