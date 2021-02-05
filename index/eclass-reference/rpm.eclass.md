RPM.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
rpm.eclass - convenience class for extracting RPMs
FUNCTIONS
rpm_unpack <rpms>
Unpack the contents of the specified rpms like the unpack() function.
srcrpm_unpack <rpms>
Unpack the contents of the specified rpms like the unpack() function as well as any archives that it might contain. Note that the secondary archive unpack isn't perfect in that it simply unpacks all archives in the working directory (with the assumption that there weren't any to start with).
rpm_src_unpack
Automatically unpack all archives in ${A} including rpms. If one of the archives in a source rpm, then the sub archives will be unpacked as well.
rpm_spec_epatch [spec]
Read the specified spec (defaults to ${PN}.spec) and attempt to apply all the patches listed in it. If the spec does funky things like moving files around, well this won't handle that.
MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
rpm.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/rpm.eclass
Index
NAME
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:01 GMT, October 11, 2020