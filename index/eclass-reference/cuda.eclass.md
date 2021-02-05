CUDA.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
cuda.eclass - Common functions for cuda packages
DESCRIPTION
This eclass contains functions to be used with cuda package. Currently it is setting and/or sanitizing NVCCFLAGS, the compiler flags for nvcc. This is automatically done and exported in src_prepare() or manually by calling cuda_sanatize.
SUPPORTED EAPIS
5 6 7
EXAMPLE
inherit cuda
FUNCTIONS
cuda_gccdir [-f]
Helper for determination of the latest gcc bindir supported by then current nvidia cuda toolkit.
Example:

cuda_gccdir -f
-> --compiler-bindir "/usr/x86_64-pc-linux-gnu/gcc-bin/4.6.3"
Return value: gcc bindir compatible with current cuda, optionally (-f) prefixed with "--compiler-bindir "

cuda_sanitize
Correct NVCCFLAGS by adding the necessary reference to gcc bindir and passing CXXFLAGS to underlying compiler without disturbing nvcc.
cuda_add_sandbox [-w]
Add nvidia dev nodes to the sandbox predict list. with -w, add to the sandbox write list.
cuda_toolkit_version
echo the installed version of dev-util/nvidia-cuda-toolkit
cuda_cudnn_version
echo the installed version of dev-libs/cudnn
cuda_src_prepare
Sanitise and export NVCCFLAGS by default
ECLASS VARIABLES
NVCCFLAGS ?= -O2
nvcc compiler flags (see nvcc --help), which should be used like CFLAGS for c compiler
CUDA_VERBOSE ?= true
Being verbose during compilation to see underlying commands
MAINTAINERS
Gentoo Science Project <sci@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
cuda.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/cuda.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
EXAMPLE
FUNCTIONS
ECLASS VARIABLES
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:02 GMT, October 11, 2020