MULTIPROCESSING.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
multiprocessing.eclass - multiprocessing helper functions
DESCRIPTION
The multiprocessing eclass contains a suite of utility functions that could be helpful to controlling parallel multiple job execution. The most common use is processing MAKEOPTS in order to obtain job count.
EXAMPLE
src_compile() {
  # custom build system that does not support most of MAKEOPTS
  ./mybs -j$(makeopts_jobs)
}
FUNCTIONS
get_nproc [${fallback:-1}]
Attempt to figure out the number of processing units available. If the value can not be determined, prints the provided fallback instead. If no fallback is provided, defaults to 1.
makeopts_jobs [${MAKEOPTS}] [${inf:-999}]
Searches the arguments (defaults to ${MAKEOPTS}) and extracts the jobs number specified therein. Useful for running non-make tools in parallel too. i.e. if the user has MAKEOPTS=-j9, this will echo "9" -- we can't return the number as bash normalizes it to [0, 255]. If the flags haven't specified a -j flag, then "1" is shown as that is the default `make` uses. Since there's no way to represent infinity, we return ${inf} (defaults to 999) if the user has -j without a number.
makeopts_loadavg [${MAKEOPTS}] [${inf:-999}]
Searches the arguments (defaults to ${MAKEOPTS}) and extracts the value set for load-average. For make and ninja based builds this will mean new jobs are not only limited by the jobs-value, but also by the current load - which might get excessive due to I/O and not just due to CPU load. Be aware that the returned number might be a floating-point number. Test whether your software supports that. If no limit is specified or --load-average is used without a number, ${inf} (defaults to 999) is returned.
AUTHORS
Brian Harring <ferringb@gentoo.org>
Mike Frysinger <vapier@gentoo.org>
MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
multiprocessing.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/multiprocessing.eclass
Index
NAME
DESCRIPTION
EXAMPLE
FUNCTIONS
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:02 GMT, October 11, 2020