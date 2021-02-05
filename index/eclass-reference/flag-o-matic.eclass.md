FLAG-O-MATIC.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
flag-o-matic.eclass - common functions to manipulate and query toolchain flags
DESCRIPTION
This eclass contains a suite of functions to help developers sanely and safely manage toolchain flags in their builds.
FUNCTIONS
filter-flags <flags>
Remove particular <flags> from {C,CPP,CXX,CCAS,F,FC,LD}FLAGS. Accepts shell globs.
filter-lfs-flags
Remove flags that enable Large File Support.
filter-ldflags <flags>
Remove particular <flags> from LDFLAGS. Accepts shell globs.
append-cppflags <flags>
Add extra <flags> to the current CPPFLAGS.
append-cflags <flags>
Add extra <flags> to the current CFLAGS. If a flag might not be supported with different compilers (or versions), then use test-flags-CC like so:
append-cflags $(test-flags-CC -funky-flag)
append-cxxflags <flags>
Add extra <flags> to the current CXXFLAGS. If a flag might not be supported with different compilers (or versions), then use test-flags-CXX like so:
append-cxxflags $(test-flags-CXX -funky-flag)
append-fflags <flags>
Add extra <flags> to the current {F,FC}FLAGS. If a flag might not be supported with different compilers (or versions), then use test-flags-F77 like so:
append-fflags $(test-flags-F77 -funky-flag)
append-lfs-flags
Add flags that enable Large File Support.
append-ldflags <flags>
Add extra <flags> to the current LDFLAGS.
append-flags <flags>
Add extra <flags> to your current {C,CXX,F,FC}FLAGS.
replace-flags <old> <new>
Replace the <old> flag with <new>. Accepts shell globs for <old>.
replace-cpu-flags <old> <new>
Replace cpu flags (like -march/-mcpu/-mtune) that select the <old> cpu with flags that select the <new> cpu. Accepts shell globs for <old>.
is-flagq <flag>
Returns shell true if <flag> is in {C,CXX,F,FC}FLAGS, else returns shell false. Accepts shell globs.
is-flag <flag>
Echo's "true" if flag is set in {C,CXX,F,FC}FLAGS. Accepts shell globs.
is-ldflagq <flag>
Returns shell true if <flag> is in LDFLAGS, else returns shell false. Accepts shell globs.
is-ldflag <flag>
Echo's "true" if flag is set in LDFLAGS. Accepts shell globs.
filter-mfpmath <math types>
Remove specified math types from the fpmath flag. For example, if the user has -mfpmath=sse,386, running `filter-mfpmath sse` will leave the user with -mfpmath=386.
strip-flags
Strip *FLAGS of everything except known good/safe flags. This runs over all flags returned by all_flag_vars().
test-flag-CC <flag>
Returns shell true if <flag> is supported by the C compiler, else returns shell false.
test-flag-CXX <flag>
Returns shell true if <flag> is supported by the C++ compiler, else returns shell false.
test-flag-F77 <flag>
Returns shell true if <flag> is supported by the Fortran 77 compiler, else returns shell false.
test-flag-FC <flag>
Returns shell true if <flag> is supported by the Fortran 90 compiler, else returns shell false.
test-flag-CCLD <flag>
Returns shell true if <flag> is supported by the C compiler and linker, else returns shell false.
test-flags-CC <flags>
Returns shell true if <flags> are supported by the C compiler, else returns shell false.
test-flags-CXX <flags>
Returns shell true if <flags> are supported by the C++ compiler, else returns shell false.
test-flags-F77 <flags>
Returns shell true if <flags> are supported by the Fortran 77 compiler, else returns shell false.
test-flags-FC <flags>
Returns shell true if <flags> are supported by the Fortran 90 compiler, else returns shell false.
test-flags-CCLD <flags>
Returns shell true if <flags> are supported by the C compiler and default linker, else returns shell false.
test-flags <flags>
Short-hand that should hopefully work for both C and C++ compiler, but its really only present due to the append-flags() abomination.
test_version_info <version>
Returns shell true if the current C compiler version matches <version>, else returns shell false. Accepts shell globs.
strip-unsupported-flags
Strip {C,CXX,F,FC}FLAGS of any flags not supported by the active toolchain.
get-flag <flag>
Find and echo the value for a particular flag. Accepts shell globs.
replace-sparc64-flags
Sets mcpu to v8 and uses the original value as mtune if none specified.
append-libs <libs>
Add extra <libs> to the current LIBS. All arguments should be prefixed with either -l or -L. For compatibility, if arguments are not prefixed as options, they are given a -l prefix automatically.
raw-ldflags [flags]
Turn C style ldflags (-Wl,-foo) into straight ldflags - the results are suitable for passing directly to 'ld'; note LDFLAGS is usually passed to gcc where it needs the '-Wl,'.
If no flags are specified, then default to ${LDFLAGS}.

no-as-needed
Return value: Flag to disable asneeded behavior for use with append-ldflags.
MAINTAINERS
toolchain@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
flag-o-matic.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/flag-o-matic.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:05 GMT, October 11, 2020