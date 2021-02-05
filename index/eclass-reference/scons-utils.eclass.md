SCONS-UTILS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
scons-utils.eclass - helper functions to deal with SCons buildsystem
DESCRIPTION
This eclass provides a set of function to help developers sanely call dev-util/scons and pass parameters to it.
As of dev-util/scons-3.0.1-r100, SCons supports Python 3. Since SCons* files in build systems are written as Python, all packages need to explicitly verify which versions of Python are supported and use appropriate Python suite eclass to select the implementation. The eclass needs to be inherited before scons-utils, and scons-utils will automatically take advantage of it. For more details, please see: https://wiki.gentoo.org/wiki/Project:Python/scons-utils_integration

Please note that SCons is more like a 'build system creation kit', and requires a lot of upstream customization to be used sanely. You will often need to request fixes upstream and/or patch the build system. In particular:

1. There are no 'standard' variables. To respect CC, CXX, CFLAGS, CXXFLAGS, CPPFLAGS, LDFLAGS, upstream needs to define appropriate variables explicitly. In some cases, upstreams respect envvars, in others you need to pass them as options.

2. SCons scrubs out environment by default and replaces it with some pre-defined values. To respect environment variables such as PATH, Upstreams need to explicitly get them from os.environ and copy them to the build environment.

SUPPORTED EAPIS
0 1 2 3 4 5 6 7
EXAMPLE
PYTHON_COMPAT=( python2_7 )
inherit python-any-r1 scons-utils toolchain-funcs

EAPI=5

src_configure() {
        MYSCONS=(
                CC="$(tc-getCC)"
        ENABLE_NLS=$(usex nls)
        )
}

src_compile() {
        escons "${MYSCONS[@]}"
}

src_install() {
        # note: this can be DESTDIR, INSTALL_ROOT, ... depending on package
        escons "${MYSCONS[@]}" DESTDIR="${D}" install
}
FUNCTIONS
myesconsargs
DEPRECATED, EAPI 0..5 ONLY: pass options to escons instead
List of package-specific options to pass to all SCons calls. Supposed to be set in src_configure().

escons [<args>...]
Call scons, passing the supplied arguments. Like emake, this function does die on failure in EAPI 4. Respects nonfatal in EAPI 6 and newer.
use_scons <use-flag> [var-name] [var-opt-true] [var-opt-false]
DEPRECATED, EAPI 0..5 ONLY: use usex instead
Output a SCons parameter with value depending on the USE flag state. If the USE flag is set, output <var-name>=<var-opt-true>; otherwise <var-name>=<var-opt-false>.

If <var-name> is omitted, <use-flag> will be used instead. However, if <use-flag> starts with an exclamation mark (!flag), 'no' will be prepended to the name (e.g. noflag).

If <var-opt-true> and/or <var-opt-false> are omitted, ${USE_SCONS_TRUE} and/or ${USE_SCONS_FALSE} will be used instead.

ECLASS VARIABLES
SCONS_MIN_VERSION
The minimal version of SCons required for the build to work.
SCONSOPTS
The default set of options to pass to scons. Similar to MAKEOPTS, supposed to be set in make.conf. If unset, escons() will use cleaned up MAKEOPTS instead.
EXTRA_ESCONS
The additional parameters to pass to SCons whenever escons() is used. Much like EXTRA_EMAKE, this is not supposed to be used in make.conf and not in ebuilds!
USE_SCONS_TRUE ?= 1
DEPRECATED: use usex instead
The default value for truth in scons-use() (1 by default).

USE_SCONS_FALSE ?= 0
DEPRECATED: use usex instead
The default value for false in scons-use() (0 by default).

MAINTAINERS
mgorny@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
scons-utils.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/scons-utils.eclass
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