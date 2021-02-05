UNPACKER.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
unpacker.eclass - helpers for extraneous file formats and consistent behavior across EAPIs
DESCRIPTION
Some extraneous file formats are not part of PMS, or are only in certain EAPIs. Rather than worrying about that, support the crazy cruft here and for all EAPI versions.
FUNCTIONS
unpack_pdv <file to unpack> <size of off_t>
Unpack those pesky pdv generated files ... They're self-unpacking programs with the binary package stuffed in the middle of the archive. Valve seems to use it a lot ... too bad it seems to like to segfault a lot :(. So lets take it apart ourselves.
You have to specify the off_t size ... I have no idea how to extract that information out of the binary executable myself. Basically you pass in the size of the off_t type (in bytes) on the machine that built the pdv archive.

One way to determine this is by running the following commands:

        strings <pdv archive> | grep lseek
        strace -elseek <pdv archive>
Basically look for the first lseek command (we do the strings/grep because sometimes the function call is _llseek or something) and steal the 2nd parameter. Here is an example:

        $ strings hldsupdatetool.bin | grep lseek
        lseek
        $ strace -elseek ./hldsupdatetool.bin
        lseek(3, -4, SEEK_END)                                  = 2981250
Thus we would pass in the value of '4' as the second parameter.

unpack_makeself [file to unpack] [offset] [tail|dd]
Unpack those pesky makeself generated files ... They're shell scripts with the binary package tagged onto the end of the archive. Loki utilized the format as does many other game companies.
If the file is not specified, then ${A} is used. If the offset is not specified then we will attempt to extract the proper offset from the script itself.

unpack_deb <one deb to unpack>
Unpack a Debian .deb archive in style.
unpack_cpio <one cpio to unpack>
Unpack a cpio archive, file "-" means stdin.
unpack_zip <zip file>
Unpack zip archives. This function ignores all non-fatal errors (i.e. warnings). That is useful for zip archives with extra crap attached (e.g. self-extracting archives).
unpacker [archives to unpack]
This works in the same way that `unpack` does. If you don't specify any files, it will default to ${A}.
unpacker_src_unpack
Run `unpacker` to unpack all our stuff.
unpacker_src_uri_depends [archives that we will unpack]
Walk all the specified files (defaults to $SRC_URI) and figure out the dependencies that are needed to unpack things.
Note: USE flags are not yet handled.

Return value: Dependencies needed to unpack all the archives

ECLASS VARIABLES
UNPACKER_BZ2
Utility to use to decompress bzip2 files. Will dynamically pick between `pbzip2` and `bzip2`. Make sure your choice accepts the "-dc" options. Note: this is meant for users to set, not ebuilds.
UNPACKER_LZIP
Utility to use to decompress lzip files. Will dynamically pick between `plzip`, `pdlzip` and `lzip`. Make sure your choice accepts the "-dc" options. Note: this is meant for users to set, not ebuilds.
MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
unpacker.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/unpacker.eclass
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
Time: 00:27:01 GMT, October 11, 2020