EPATCH.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
epatch.eclass - easy patch application functions
DESCRIPTION
An eclass providing epatch and epatch_user functions to easily apply patches to ebuilds. Mostly superseded by eapply* in EAPI 6.
SUPPORTED EAPIS
0 1 2 3 4 5 6
FUNCTIONS
EPATCH_SOURCE = "${WORKDIR}/patch"
Default directory to search for patches.
EPATCH_SUFFIX = "patch.bz2"
Default extension for patches (do not prefix the period yourself).
EPATCH_OPTS = ""
Options to pass to patch. Meant for ebuild/package-specific tweaking such as forcing the patch level (-p#) or fuzz (-F#) factor. Note that for single patch tweaking, you can also pass flags directly to epatch.
EPATCH_COMMON_OPTS = "-g0 -E --no-backup-if-mismatch"
Common options to pass to `patch`. You probably should never need to change these. If you do, please discuss it with base-system first to be sure.
-g0 - keep RCS, ClearCase, Perforce and SCCS happy #24571
--no-backup-if-mismatch - do not leave .orig files behind
-E - automatically remove empty files
EPATCH_EXCLUDE = ""
List of patches not to apply.   Note this is only file names,
and not the full path. Globs accepted.
EPATCH_SINGLE_MSG = ""
Change the printed message for a single patch.
EPATCH_MULTI_MSG = "Applying various patches (bugfixes/updates) ..."
Change the printed message for multiple patches.
EPATCH_FORCE = "no"
Only require patches to match EPATCH_SUFFIX rather than the extended arch naming style.
EPATCH_USER_EXCLUDE
List of patches not to apply.   Note this is only file names,
and not the full path. Globs accepted.
epatch [options] [patches] [dirs of patches]
epatch is designed to greatly simplify the application of patches. It can process patch files directly, or directories of patches. The patches may be compressed (bzip/gzip/etc...) or plain text. You generally need not specify the -p option as epatch will automatically attempt -p0 to -p4 until things apply successfully.
If you do not specify any patches/dirs, then epatch will default to the directory specified by EPATCH_SOURCE.

Any options specified that start with a dash will be passed down to patch for this specific invocation. As soon as an arg w/out a dash is found, then arg processing stops.

When processing directories, epatch will apply all patches that match:

if ${EPATCH_FORCE} != "yes"
        ??_${ARCH}_foo.${EPATCH_SUFFIX}
else
        *.${EPATCH_SUFFIX}
The leading ?? are typically numbers used to force consistent patch ordering. The arch field is used to apply patches only for the host architecture with the special value of "all" means apply for everyone. Note that using values other than "all" is highly discouraged -- you should apply patches all the time and let architecture details be detected at configure/compile time.
If EPATCH_SUFFIX is empty, then no period before it is implied when searching for patches to apply.

Refer to the other EPATCH_xxx variables for more customization of behavior.

EPATCH_USER_SOURCE ?= ${PORTAGE_CONFIGROOT%/}/etc/portage/patches
Location for user patches, see the epatch_user function. Should be set by the user. Don't set this in ebuilds.
epatch_user
Applies user-provided patches to the source tree. The patches are taken from /etc/portage/patches/<CATEGORY>/<P-PR|P|PN>[:SLOT]/, where the first of these three directories to exist will be the one to use, ignoring any more general directories which might exist as well. They must end in ".patch" to be applied.
User patches are intended for quick testing of patches without ebuild modifications, as well as for permanent customizations a user might desire. Obviously, there can be no official support for arbitrarily patched ebuilds. So whenever a build log in a bug report mentions that user patches were applied, the user should be asked to reproduce the problem without these.

Not all ebuilds do call this function, so placing patches in the stated directory might or might not work, depending on the package and the eclasses it inherits and uses. It is safe to call the function repeatedly, so it is always possible to add a call at the ebuild level. The first call is the time when the patches will be applied.

Ideally, this function should be called after gentoo-specific patches have been applied, so that their code can be modified as well, but before calls to e.g. eautoreconf, as the user patches might affect autotool input files as well.

MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
epatch.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/epatch.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020