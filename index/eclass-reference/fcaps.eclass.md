FCAPS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
fcaps.eclass - function to set POSIX file-based capabilities
DESCRIPTION
This eclass provides a function to set file-based capabilities on binaries. This is not the same as USE=caps which controls runtime capability changes, often via packages like libcap.
Due to probable capability-loss on moving or copying, this happens in pkg_postinst phase (at least for now).

EXAMPLE
You can manually set the caps on ping and ping6 by doing:
pkg_postinst() {
        fcaps cap_net_raw bin/ping bin/ping6
}
Or set it via the global ebuild var FILECAPS:

FILECAPS=(
        cap_net_raw bin/ping bin/ping6
)
FUNCTIONS
fcaps [-o <owner>] [-g <group>] [-m <mode>] [-M <caps mode>] <capabilities> <file[s]>
Sets the specified capabilities on the specified files.
The caps option takes the form as expected by the cap_from_text(3) man page. If no action is specified, then "=ep" will be used as a default.

If the file is a relative path (e.g. bin/foo rather than /bin/foo), then the appropriate path var ($D/$ROOT/etc...) will be prefixed based on the current ebuild phase.

The caps mode (default 711) is used to set the permission on the file if capabilities were properly set on the file.

If the system is unable to set capabilities, it will use the specified user, group, and mode (presumably to make the binary set*id). The defaults there are root:0 and 4711. Otherwise, the ownership and permissions will be unchanged.

fcaps_pkg_postinst
Process the FILECAPS array.
ECLASS VARIABLES
FILECAPS
An array of fcap arguments to use to automatically execute fcaps. See that function for more details.
All args are consumed until the '--' marker is found. So if you have:

        FILECAPS=( moo cow -- fat cat -- chubby penguin )
This will end up executing:

        fcaps moo cow
        fcaps fat cat
        fcaps chubby penguin
Note: If you override pkg_postinst, you must call fcaps_pkg_postinst yourself.

MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
fcaps.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/fcaps.eclass
Index
NAME
DESCRIPTION
EXAMPLE
FUNCTIONS
ECLASS VARIABLES
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:05 GMT, October 11, 2020