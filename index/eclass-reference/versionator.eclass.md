VERSIONATOR.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
versionator.eclass - functions which simplify manipulation of ${PV} and similar version strings
DESCRIPTION
This eclass provides functions which simplify manipulating $PV and similar variables. Most functions default to working with $PV, although other values can be used.
SUPPORTED EAPIS
0 1 2 3 4 5 6
EXAMPLE
Simple Example 1: $PV is 1.2.3b, we want 1_2.3b:
    MY_PV=$(replace_version_separator 1 '_' )
Simple Example 2: $PV is 1.4.5, we want 1:
    MY_MAJORV=$(get_major_version )

Rather than being a number, the index parameter can be a separator character such as '-', '.' or '_'. In this case, the first separator of this kind is selected.

There's also:
    version_is_at_least             want      have
 which may be buggy, so use with caution.

FUNCTIONS
get_all_version_components [version]
Split up a version string into its component parts. If no parameter is supplied, defaults to $PV.
    0.8.3       ->  0 . 8 . 3
    7c          ->  7 c
    3.0_p2      ->  3 . 0 _ p2
    20040905    ->  20040905
    3.0c-r1     ->  3 . 0 c - r1
get_version_components [version]
Get the important version components, excluding '.', '-' and '_'. Defaults to $PV if no parameter is supplied.
    0.8.3       ->  0 8 3
    7c          ->  7 c
    3.0_p2      ->  3 0 p2
    20040905    ->  20040905
    3.0c-r1     ->  3 0 c r1
get_major_version [version]
Get the major version of a value. Defaults to $PV if no parameter is supplied.
    0.8.3       ->  0
    7c          ->  7
    3.0_p2      ->  3
    20040905    ->  20040905
    3.0c-r1     ->  3
get_version_component_range <range> [version]
Get a particular component or range of components from the version. If no version parameter is supplied, defaults to $PV.
   1      1.2.3       -> 1
   1-2    1.2.3       -> 1.2
   2-     1.2.3       -> 2.3
get_after_major_version [version]
Get everything after the major version and its separator (if present) of a value. Defaults to $PV if no parameter is supplied.
    0.8.3       ->  8.3
    7c          ->  c
    3.0_p2      ->  0_p2
    20040905    ->  (empty string)
    3.0c-r1     ->  0c-r1
replace_version_separator <search> <replacement> [subject]
Replace the $1th separator with $2 in $3 (defaults to $PV if $3 is not supplied). If there are fewer than $1 separators, don't change anything.
    1 '_' 1.2.3       -> 1_2.3
    2 '_' 1.2.3       -> 1.2_3
    1 '_' 1b-2.3      -> 1b_2.3 Rather than being a number, $1 can be a separator character such as '-', '.' or '_'. In this case, the first separator of this kind is selected.
replace_all_version_separators <replacement> [subject]
Replace all version separators in $2 (defaults to $PV) with $1.
    '_' 1b.2.3        -> 1b_2_3
delete_version_separator <search> [subject]
Delete the $1th separator in $2 (defaults to $PV if $2 is not supplied). If there are fewer than $1 separators, don't change anything.
    1 1.2.3       -> 12.3
    2 1.2.3       -> 1.23
    1 1b-2.3      -> 1b2.3 Rather than being a number, $1 can be a separator character such as '-', '.' or '_'. In this case, the first separator of this kind is deleted.
delete_all_version_separators [subject]
Delete all version separators in $1 (defaults to $PV).
    1b.2.3        -> 1b23
get_version_component_count [version]
How many version components are there in $1 (defaults to $PV)?
    1.0.1       ->  3
    3.0c-r1     ->  4
get_last_version_component_index [version]
What is the index of the last version component in $1 (defaults to $PV)? Equivalent to get_version_component_count - 1.
    1.0.1       ->  2
    3.0c-r1     ->  3
version_is_at_least <want> [have]
Is $2 (defaults to $PVR) at least version $1? Intended for use in eclasses only. May not be reliable, be sure to do very careful testing before actually using this.
version_compare <A> <B>
Takes two parameters (A, B) which are versions. If A is an earlier version than B, returns 1. If A is identical to B, return 2. If A is later than B, return 3. You probably want version_is_at_least rather than this function. May not be very reliable. Test carefully before using this.
version_sort <version> [more versions...]
Returns its parameters sorted, highest version last. We're using a quadratic algorithm for simplicity, so don't call it with more than a few dozen items. Uses version_compare, so be careful.
version_format_string <format> [version]
Reformat complicated version strings. The first argument is the string to reformat with while the rest of the args are passed on to the get_version_components function. You should make sure to single quote the first argument since it'll have variables that get delayed expansions.
MAINTAINERS
Jonathan Callen <jcallen@gentoo.org>
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
versionator.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/versionator.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
EXAMPLE
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020