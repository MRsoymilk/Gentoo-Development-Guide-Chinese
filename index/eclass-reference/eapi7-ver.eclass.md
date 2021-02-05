EAPI7-VER.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
eapi7-ver.eclass - Testing implementation of EAPI 7 version manipulators
DESCRIPTION
A stand-alone implementation of the version manipulation functions aimed for EAPI 7. Intended to be used for wider testing of the proposed functions and to allow ebuilds to switch to the new model early, with minimal change needed for actual EAPI 7.
https://bugs.gentoo.org/482170

Version strings
The functions support arbitrary version strings consisting of version components interspersed with (possibly empty) version separators.

A version component can either consist purely of digits ([0-9]+) or purely of uppercase and lowercase letters ([A-Za-z]+). A version separator is either a string of any other characters ([^A-Za-z0-9]+), or it occurs at the transition between a sequence of letters and a sequence of digits, or vice versa. In the latter case, the version separator is an empty string.

The version is processed left-to-right, and each successive component is assigned numbers starting with 1. The components are either split on version separators or on boundaries between digits and letters (in which case the separator between the components is empty). Version separators are assigned numbers starting with 1 for the separator between 1st and 2nd components. As a special case, if the version string starts with a separator, it is assigned index 0.

Examples:

  1.2b-alpha4 -> 1 . 2 '' b - alpha '' 4
                 c s c s  c s c     s  c
                 1 1 2 2  3 3 4     4  5

  .11.        -> . 11 .
                 s c  s
                 0 1  1
Ranges
A range can be specified as 'm' for m-th version component, 'm-' for all components starting with m-th or 'm-n' for components starting at m-th and ending at n-th (inclusive). If the range spans outside the version string, it is truncated silently.

SUPPORTED EAPIS
0 1 2 3 4 5 6
FUNCTIONS
ver_cut <range> [<version>]
Print the substring of the version string containing components defined by the <range> and the version separators between them. Processes <version> if specified, ${PV} otherwise.
For the syntax of versions and ranges, please see the eclass description.

ver_rs <range> <repl> [<range> <repl>...] [<version>]
Print the version string after substituting the specified version separators at <range> with <repl> (string). Multiple '<range> <repl>' pairs can be specified. Processes <version> if specified, ${PV} otherwise.
For the syntax of versions and ranges, please see the eclass description.

ver_test [<v1>] <op> <v2>
Check if the relation <v1> <op> <v2> is true. If <v1> is not specified, default to ${PVR}. <op> can be -gt, -ge, -eq, -ne, -le, -lt. Both versions must conform to the PMS version syntax (with optional revision parts), and the comparison is performed according to the algorithm specified in the PMS.
AUTHORS
Ulrich Müller <ulm@gentoo.org>
Michał Górny <mgorny@gentoo.org>
MAINTAINERS
PMS team <pms@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
eapi7-ver.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/eapi7-ver.eclass
Index
NAME
DESCRIPTION
Version strings
Ranges
SUPPORTED EAPIS
FUNCTIONS
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020