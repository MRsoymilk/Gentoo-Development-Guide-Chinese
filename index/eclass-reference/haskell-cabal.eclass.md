HASKELL-CABAL.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
haskell-cabal.eclass - for packages that make use of the Haskell Common Architecture for Building Applications and Libraries (cabal)
DESCRIPTION
Basic instructions:
Before inheriting the eclass, set CABAL_FEATURES to reflect the tools and features that the package makes use of.

Currently supported features:
  haddock    --  for documentation generation
  hscolour   --  generation of colourised sources
  hoogle     --  generation of documentation search index
  profile    --  if package supports to build profiling-enabled libraries
  bootstrap  --  only used for the cabal package itself
  lib        --  the package installs libraries
  nocabaldep --  don't add dependency on cabal.
                 only used for packages that _must_ not pull the dependency
                 on cabal, but still use this eclass (e.g. haskell-updater).
  ghcdeps    --  constraint dependency on package to ghc onces
                 only used for packages that use libghc internally and _must_
                 not pull upper versions
  test-suite --  add support for cabal test-suites (introduced in Cabal-1.8)

FUNCTIONS
cabal_flag
ebuild.sh:use_enable() taken as base
Usage examples:


    CABAL_CONFIGURE_FLAGS=$(cabal_flag gui)
 leads to "--flags=gui" or "--flags=-gui" (useflag 'gui')


    CABAL_CONFIGURE_FLAGS=$(cabal_flag gtk gui)
 also leads to "--flags=gui" or " --flags=-gui" (useflag 'gtk')

cabal_chdeps
Allows easier patching of $CABAL_FILE (${S}/${PN}.cabal by default) depends
Accepts argument list as pairs of substitutions: <from-string> <to-string>...

Dies on error.

Usage examples:

src_prepare() {
   cabal_chdeps        'base >= 4.2 && < 4.6' 'base >= 4.2 && < 4.7'        'containers ==0.4.*' 'containers >= 0.4 && < 0.6' } or src_prepare() {
   CABAL_FILE=${S}/${MY_PN}.cabal cabal_chdeps        'base >= 4.2 && < 4.6' 'base >= 4.2 && < 4.7'
   CABAL_FILE=${S}/${MY_PN}-tools.cabal cabal_chdeps        'base == 3.*' 'base >= 4.2 && < 4.7' }

cabal-constraint
Allowes to set contraint to the libraries that are used by specified package
replace-hcflags <old> <new>
Replace the <old> flag with <new> in HCFLAGS. Accepts shell globs for <old>. The implementation is picked from flag-o-matic.eclass:replace-flags()
ECLASS VARIABLES
CABAL_EXTRA_CONFIGURE_FLAGS
User-specified additional parameters passed to 'setup configure'. example: /etc/portage/make.conf:
   CABAL_EXTRA_CONFIGURE_FLAGS="--enable-shared --enable-executable-dynamic"
CABAL_EXTRA_BUILD_FLAGS
User-specified additional parameters passed to 'setup build'. example: /etc/portage/make.conf: CABAL_EXTRA_BUILD_FLAGS=-v
GHC_BOOTSTRAP_FLAGS
User-specified additional parameters for ghc when building _only_ 'setup' binary bootstrap. example: /etc/portage/make.conf: GHC_BOOTSTRAP_FLAGS=-dynamic to make linking 'setup' faster.
CABAL_EXTRA_TEST_FLAGS
User-specified additional parameters passed to 'setup test'. example: /etc/portage/make.conf:
   CABAL_EXTRA_TEST_FLAGS="-v3 --show-details=streaming"
CABAL_DEBUG_LOOSENING
Show debug output for 'cabal_chdeps' function if set. Needs working 'diff'.
CABAL_REPORT_OTHER_BROKEN_PACKAGES ?= yes
Show other broken packages if 'cabal configure' fails. It should be normally enabled unless you know you are about to try to compile a lot of broken packages. Default value: 'yes' Set to anything else to disable.
AUTHORS
Original author: Andres Loeh <kosmikus@gentoo.org>
Original author: Duncan Coutts <dcoutts@gentoo.org>
MAINTAINERS
Haskell herd <haskell@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
haskell-cabal.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/haskell-cabal.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020