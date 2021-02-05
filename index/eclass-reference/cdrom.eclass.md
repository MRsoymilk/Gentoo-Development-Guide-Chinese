CDROM.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
cdrom.eclass - Functions for CD-ROM handling
DESCRIPTION
Acquire CD(s) for those lovely CD-based emerges. Yes, this violates the whole "non-interactive" policy, but damnit I want CD support!
Do not call these functions in pkg_* phases like pkg_setup as they should not be used for binary packages. Most packages using this eclass will require RESTRICT="bindist" but the point still stands. The functions are generally called in src_unpack.

FUNCTIONS
cdrom_get_cds <cd1 file>[:alt cd1 file] [cd2 file[:alt cd2 file]] [...]
Attempt to locate a CD based upon a file that is on the CD.
If the data spans multiple discs then additional arguments can be given to check for more files. Call cdrom_load_next_cd() to scan for the next disc in the set.

Sometimes it is necessary to support alternative CD "sets" where the contents differ. Alternative files for each disc can be appended to each argument, separated by the : character. This feature is frequently used to support installing from an existing installation. Note that after the first disc is detected, the set is locked so cdrom_load_next_cd() will only scan for files in that specific set on subsequent discs.

The given files can be within named subdirectories. It is not necessary to specify different casings of the same filename as matching is done case-insensitively. Filenames can include special characters such as spaces. Only : is not allowed.

If you don't want each disc to be referred to as "CD #1", "CD #2", etc. then you can optionally provide your own names. Set CDROM_NAME for a single disc, CDROM_NAMES as an array for multiple discs, or individual CDROM_NAME_# variables for each disc starting from 1.

Despite what you may have seen in older ebuilds, it has never been possible to provide per-set disc names. This would not make sense as all the names are initially displayed before the first disc has been detected. As a workaround, you can redefine the name variable(s) after the first disc has been detected.

This function ends with a cdrom_load_next_cd() call to scan for the first disc. For more details about variables read and written by this eclass, see that function's description.

cdrom_load_next_cd
If multiple arguments were given to cdrom_get_cds() then you can call this function to scan for the next disc. This function is also called implicitly to scan for the first disc.
The file(s) given to cdrom_get_cds() are scanned for on any mounted filesystem that resembles optical media. If no match is found then the user is prompted to insert and mount the disc and press enter to rescan. This will loop continuously until a match is found or the user aborts with Ctrl+C.

The user can override the scan location by setting CD_ROOT for a single disc, CD_ROOT if multiple discs are merged into the same directory tree (useful for existing installations), or individual CD_ROOT_# variables for each disc starting from 1. If no match is found then the function dies with an error as a rescan will not help in this instance.

Users wanting to set CD_ROOT or CD_ROOT_# for specific packages persistently can do so using Portage's /etc/portage/env feature.

Regardless of which scanning method is used, several variables are set by this function for you to use:


 CDROM_ROOT: Root path of the detected disc.
 CDROM_MATCH: Path of the matched file, relative to CDROM_ROOT.
 CDROM_ABSMATCH: Absolute path of the matched file.
 CDROM_SET: The matching set number, starting from 0.

The casing of CDROM_MATCH may not be the same as the argument given to cdrom_get_cds() as matching is done case-insensitively. You should therefore use this variable (or CDROM_ABSMATCH) when performing file operations to ensure the file is found. Use newins rather than doins to keep the final result consistent and take advantage of Bash case-conversion features like ${FOO,,}.

Chances are that you'll need more than just the matched file from each disc though. You should not assume the casing of these files either but dealing with this goes beyond the scope of this ebuild. For a good example, see games-action/descent2-data, which combines advanced globbing with advanced tar features to concisely deal with case-insensitive matching, case conversion, file moves, and conditional exclusion.

Copying directly from a mounted disc using doins/newins will remove any read-only permissions but be aware of these when copying to an intermediate directory first. Attempting to clean a build directory containing read-only files as a non-root user will result in an error. If you're using tar as suggested above then you can easily work around this with --mode=u+w.

Note that you can only go forwards in the disc list, so make sure you only call this function when you're done using the current disc.

If you cd to any location within CDROM_ROOT then remember to leave the directory before calling this function again, otherwise the user won't be able to unmount the current disc.

ECLASS VARIABLES
CDROM_OPTIONAL
By default, the eclass sets PROPERTIES="interactive" on the assumption that people will be using these. If your package optionally supports disc-based installs then set this to "yes" and we'll set things conditionally based on USE="cdinstall".
MAINTAINERS
games@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
cdrom.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/cdrom.eclass
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
Time: 00:27:02 GMT, October 11, 2020