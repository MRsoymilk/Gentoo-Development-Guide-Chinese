TOOLCHAIN-FUNCS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
toolchain-funcs.eclass - functions to query common info about the toolchain
DESCRIPTION
The toolchain-funcs aims to provide a complete suite of functions for gleaning useful information about the toolchain and to simplify ugly things like cross-compiling and multilib. All of this is done in such a way that you can rely on the function always returning something sane.
FUNCTIONS
tc-getAR [toolchain prefix]
Return value: name of the archiver
tc-getAS [toolchain prefix]
Return value: name of the assembler
tc-getCC [toolchain prefix]
Return value: name of the C compiler
tc-getCPP [toolchain prefix]
Return value: name of the C preprocessor
tc-getCXX [toolchain prefix]
Return value: name of the C++ compiler
tc-getLD [toolchain prefix]
Return value: name of the linker
tc-getSTRINGS [toolchain prefix]
Return value: name of the strings program
tc-getSTRIP [toolchain prefix]
Return value: name of the strip program
tc-getNM [toolchain prefix]
Return value: name of the symbol/object thingy
tc-getRANLIB [toolchain prefix]
Return value: name of the archive indexer
tc-getREADELF [toolchain prefix]
Return value: name of the ELF reader
tc-getOBJCOPY [toolchain prefix]
Return value: name of the object copier
tc-getOBJDUMP [toolchain prefix]
Return value: name of the object dumper
tc-getF77 [toolchain prefix]
Return value: name of the Fortran 77 compiler
tc-getFC [toolchain prefix]
Return value: name of the Fortran 90 compiler
tc-getGCJ [toolchain prefix]
Return value: name of the java compiler
tc-getGO [toolchain prefix]
Return value: name of the Go compiler
tc-getPKG_CONFIG [toolchain prefix]
Return value: name of the pkg-config tool
tc-getRC [toolchain prefix]
Return value: name of the Windows resource compiler
tc-getDLLWRAP [toolchain prefix]
Return value: name of the Windows dllwrap utility
tc-getBUILD_AR [toolchain prefix]
Return value: name of the archiver for building binaries to run on the build machine
tc-getBUILD_AS [toolchain prefix]
Return value: name of the assembler for building binaries to run on the build machine
tc-getBUILD_CC [toolchain prefix]
Return value: name of the C compiler for building binaries to run on the build machine
tc-getBUILD_CPP [toolchain prefix]
Return value: name of the C preprocessor for building binaries to run on the build machine
tc-getBUILD_CXX [toolchain prefix]
Return value: name of the C++ compiler for building binaries to run on the build machine
tc-getBUILD_LD [toolchain prefix]
Return value: name of the linker for building binaries to run on the build machine
tc-getBUILD_STRINGS [toolchain prefix]
Return value: name of the strings program for building binaries to run on the build machine
tc-getBUILD_STRIP [toolchain prefix]
Return value: name of the strip program for building binaries to run on the build machine
tc-getBUILD_NM [toolchain prefix]
Return value: name of the symbol/object thingy for building binaries to run on the build machine
tc-getBUILD_RANLIB [toolchain prefix]
Return value: name of the archive indexer for building binaries to run on the build machine
tc-getBUILD_READELF [toolchain prefix]
Return value: name of the ELF reader for building binaries to run on the build machine
tc-getBUILD_OBJCOPY [toolchain prefix]
Return value: name of the object copier for building binaries to run on the build machine
tc-getBUILD_PKG_CONFIG [toolchain prefix]
Return value: name of the pkg-config tool for building binaries to run on the build machine
tc-getTARGET_CPP [toolchain prefix]
Return value: name of the C preprocessor for the toolchain being built (or used)
tc-export <list of toolchain variables>
Quick way to export a bunch of compiler vars at once.
tc-is-cross-compiler
Return value: Shell true if we are using a cross-compiler, shell false otherwise
tc-cpp-is-true <condition> [cpp flags]
Evaluate the given condition using the C preprocessor for CTARGET, if defined, or CHOST. Additional arguments are passed through to the cpp command. A typical condition would be in the form defined(__FOO__).
Return value: Shell true if the condition is true, shell false otherwise.

tc-detect-is-softfloat
Detect whether the CTARGET (or CHOST) toolchain is a softfloat based one by examining the toolchain's output, if possible. Outputs a value alike tc-is-softfloat if detection was possible.
Return value: Shell true if detection was possible, shell false otherwise

tc-tuple-is-softfloat
Determine whether the CTARGET (or CHOST) toolchain is a softfloat based one solely from the tuple.
Return value: See tc-is-softfloat for the possible values.

tc-is-softfloat
See if this toolchain is a softfloat based one.
The possible return values:
 - only:   the target is always softfloat (never had fpu)
 - yes:    the target should support softfloat
 - softfp: (arm specific) the target should use hardfloat insns, but softfloat calling convention
 - no:     the target doesn't support softfloat
This allows us to react differently where packages accept softfloat flags in the case where support is optional, but rejects softfloat flags where the target always lacks an fpu.
tc-is-static-only
Return shell true if the target does not support shared libs, shell false otherwise.
tc-stack-grows-down
Return shell true if the stack grows down. This is the default behavior for the vast majority of systems out there and usually projects shouldn't care about such internal details.
tc-export_build_env [compiler variables]
Export common build related compiler settings.
econf_build [econf flags]
Sometimes we need to locally build up some tools to run on CBUILD because the package has helper utils which are compiled+executed when compiling. This won't work when cross-compiling as the CHOST is set to a target which we cannot natively execute.
For example, the python package will build up a local python binary using a portable build system (configure+make), but then use that binary to run local python scripts to build up other components of the overall python. We cannot rely on the python binary in $PATH as that often times will be a different version, or not even installed in the first place. Instead, we compile the code in a different directory to run on CBUILD, and then use that binary when compiling the main package to run on CHOST.

For example, with newer EAPIs, you'd do something like:

src_configure() {
        ECONF_SOURCE=${S}
        if tc-is-cross-compiler ; then
                mkdir "${WORKDIR}"/${CBUILD}
                pushd "${WORKDIR}"/${CBUILD} >/dev/null
                econf_build --disable-some-unused-stuff
                popd >/dev/null
        fi
        ... normal build paths ...
}
src_compile() {
        if tc-is-cross-compiler ; then
                pushd "${WORKDIR}"/${CBUILD} >/dev/null
                emake one-or-two-build-tools
                ln/mv build-tools to normal build paths in ${S}/
                popd >/dev/null
        fi
        ... normal build paths ...
}
tc-ld-is-gold [toolchain prefix]
Return true if the current linker is set to gold.
tc-ld-is-lld [toolchain prefix]
Return true if the current linker is set to lld.
tc-ld-disable-gold [toolchain prefix]
If the gold linker is currently selected, configure the compilation settings so that we use the older bfd linker instead.
tc-has-openmp [toolchain prefix]
See if the toolchain supports OpenMP.
tc-check-openmp
Test for OpenMP support with the current compiler and error out with a clear error message, telling the user how to rectify the missing OpenMP support that has been requested by the ebuild. Using this function to test for OpenMP support should be preferred over tc-has-openmp and printing a custom message, as it presents a uniform interface to the user.
tc-has-tls [-s|-c|-l] [toolchain prefix]
See if the toolchain supports thread local storage (TLS). Use -s to test the compiler, -c to also test the assembler, and -l to also test the C library (the default).
tc-arch-kernel [toolchain prefix]
Return value: name of the kernel arch according to the compiler target
tc-arch [toolchain prefix]
Return value: name of the portage arch according to the compiler target
tc-get-compiler-type
Return value: keyword identifying the compiler: gcc, clang, pathcc, unknown
tc-is-gcc
Return value: Shell true if the current compiler is GCC, false otherwise.
tc-is-clang
Return value: Shell true if the current compiler is clang, false otherwise.
gcc-fullversion
Return value: compiler version (major.minor.micro: [3.4.6])
gcc-version
Return value: compiler version (major.minor: [3.4].6)
gcc-major-version
Return value: major compiler version (major: [3].4.6)
gcc-minor-version
Return value: minor compiler version (minor: 3.[4].6)
gcc-micro-version
Return value: micro compiler version (micro: 3.4.[6])
clang-fullversion
Return value: compiler version (major.minor.micro: [3.4.6])
clang-version
Return value: compiler version (major.minor: [3.4].6)
clang-major-version
Return value: major compiler version (major: [3].4.6)
clang-minor-version
Return value: minor compiler version (minor: 3.[4].6)
clang-micro-version
Return value: micro compiler version (micro: 3.4.[6])
tc-enables-pie
Return truth if the current compiler generates position-independent code (PIC) which can be linked into executables.
Return value: Truth if the current compiler generates position-independent code (PIC) which can be linked into executables

tc-enables-ssp
Return truth if the current compiler enables stack smashing protection (SSP) on level corresponding to any of the following options:
 -fstack-protector
 -fstack-protector-strong
 -fstack-protector-all
Return value: Truth if the current compiler enables stack smashing protection (SSP) on at least minimal level

tc-enables-ssp-strong
Return truth if the current compiler enables stack smashing protection (SSP) on level corresponding to any of the following options:
 -fstack-protector-strong
 -fstack-protector-all
Return value: Truth if the current compiler enables stack smashing protection (SSP) on at least middle level

tc-enables-ssp-all
Return truth if the current compiler enables stack smashing protection (SSP) on level corresponding to any of the following options:
 -fstack-protector-all
Return value: Truth if the current compiler enables stack smashing protection (SSP) on maximal level

gen_usr_ldscript [-a] <list of libs to create linker scripts for>
This function is deprecated. Use the version from usr-ldscript.eclass instead.
MAINTAINERS
Toolchain Ninjas <toolchain@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
toolchain-funcs.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/toolchain-funcs.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:02 GMT, October 11, 2020