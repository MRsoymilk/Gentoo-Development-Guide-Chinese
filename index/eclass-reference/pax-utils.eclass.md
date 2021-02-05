PAX-UTILS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
pax-utils.eclass - functions to provide PaX markings for hardened kernels
DESCRIPTION
This eclass provides support for manipulating PaX markings on ELF binaries, whether the system is using legacy PT_PAX markings or the newer XATTR_PAX. The eclass wraps the use of paxctl-ng, paxctl, set/getattr and scanelf utilities, deciding which to use depending on what's installed on the build host, and whether we're working with PT_PAX, XATTR_PAX or both. Legacy PT_PAX markings no longer supported.

To control what markings are made, set PAX_MARKINGS in /etc/portage/make.conf to contain either "PT", "XT" or "none". The default is none

FUNCTIONS
pax-mark <flags> <ELF files>
Marks <ELF files> with provided PaX <flags>
Flags are passed directly to the utilities unchanged.

p: disable PAGEEXEC             P: enable PAGEEXEC
e: disable EMUTRAMP             E: enable EMUTRAMP
m: disable MPROTECT             M: enable MPROTECT
r: disable RANDMMAP             R: enable RANDMMAP
s: disable SEGMEXEC             S: enable SEGMEXEC
Default flags are 'PeMRS', which are the most restrictive settings. Refer to https://pax.grsecurity.net/ for details on what these flags are all about.

Please confirm any relaxation of restrictions with the Gentoo Hardened team. Either ask on the gentoo-hardened mailing list, or CC/assign hardened@gentoo.org on the bug report.

Return value: Shell true if we succeed, shell false otherwise

list-paxables <files>
Print to stdout all of the <files> that are suitable to have PaX flag markings, i.e., filter out the ELF executables or shared objects from a list of files. This is useful for passing wild-card lists to pax-mark, although in general it is preferable for ebuilds to list precisely which ELFS are to be marked. Often not all the ELF installed by a package need remarking.
Return value: Subset of <files> which are ELF executables or shared objects

host-is-pax
This is intended for use where the build process must be modified conditionally depending on whether the host is PaX enabled or not. It is not indented to determine whether the final binaries need PaX markings. Note: if procfs is not mounted on /proc, this returns shell false (e.g. Gentoo/FreeBSD).
Return value: Shell true if the build process is PaX enabled, shell false otherwise

ECLASS VARIABLES
PAX_MARKINGS = ${PAX_MARKINGS:="none"}
Control which markings are made: PT = PT_PAX markings, XT = XATTR_PAX markings Default to none markings.
AUTHORS
Author: Kevin F. Quinn <kevquinn@gentoo.org>
Author: Anthony G. Basile <blueness@gentoo.org>
MAINTAINERS
The Gentoo Linux Hardened Team <hardened@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
pax-utils.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/pax-utils.eclass
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
Time: 00:27:03 GMT, October 11, 2020