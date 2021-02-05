GHC-PACKAGE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
ghc-package.eclass - This eclass helps with the Glasgow Haskell Compiler's package configuration utility.
DESCRIPTION
Helper eclass to handle ghc installation/upgrade/deinstallation process.
FUNCTIONS
ghc-getghc
returns the name of the ghc executable
ghc-getghcpkg
Internal function determines returns the name of the ghc-pkg executable
ghc-getghcpkgbin
returns the name of the ghc-pkg binary (ghc-pkg itself usually is a shell script, and we have to bypass the script under certain circumstances); for Cabal, we add an empty global package config file, because for some reason the global package file must be specified
ghc-version
returns upstream version of ghc as reported by '--numeric-version' Examples: "7.10.2", "7.9.20141222"
ghc-pm-version
returns package manager(PM) version of ghc as reported by '$(best_version)' Examples: "PM:7.10.2", "PM:7.10.2_rc1", "PM:7.8.4-r4"
ghc-cabal-version
return version of the Cabal library bundled with ghc
ghc-is-dynamic
checks if ghc is built against dynamic libraries binaries linked against GHC library (and using plugin loading) have to be linked the same way:
   https://ghc.haskell.org/trac/ghc/ticket/10301
ghc-supports-shared-libraries
checks if ghc is built with support for building shared libraries (aka '-dynamic' option)
ghc-supports-threaded-runtime
checks if ghc is built with support for threaded runtime (aka '-threaded' option)
ghc-supports-smp
checks if ghc is built with support for multiple cores runtime
ghc-supports-interpreter
checks if ghc has interpreter mode (aka GHCi) It usually means that ghc supports for template haskell.
ghc-supports-parallel-make
checks if ghc has support for '--make -j' mode The option was introduced in ghc-7.8-rc1.
ghc-extractportageversion
extract the version of a portage-installed package
ghc-libdir
returns the library directory
ghc-make-args
Returns default arguments passed along 'ghc --make' build mode. Used mainly to enable parallel build mode.
ghc-confdir
returns the (Gentoo) library configuration directory, we store here a hint for 'haskell-updater' about packages installed for old ghc versions and current ones.
ghc-package-db
returns the global package database directory
ghc-localpkgconfd
returns the name of the local (package-specific) package configuration file
ghc-package-exists
tests if a ghc package exists
check-for-collisions
makes sure no packages have the same version as initial package setup
ghc-install-pkg
moves the local (package-specific) package configuration file to its final destination
ghc-recache-db
updates 'package.cache' binary cacne for registered '*.conf' packages
ghc-register-pkg
registers all packages in the local (package-specific) package configuration file
ghc-reregister
re-adds all available .conf files to the global package conf file, to be used on a ghc reinstallation
ghc-unregister-pkg
unregisters a package configuration file
ghc-pkgdeps
exported function: loads a package dependency in a form cabal_package version
ghc-package_pkg_postinst
updates package.cache after package install
ghc-package_pkg_prerm
updates package.cache after package deinstall
ghc-package_pkg_postrm
updates package.cache after package deinstall
AUTHORS
Original Author: Andres Loeh <kosmikus@gentoo.org>
MAINTAINERS
"Gentoo's Haskell Language team" <haskell@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
ghc-package.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/ghc-package.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:05 GMT, October 11, 2020