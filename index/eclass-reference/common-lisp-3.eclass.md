COMMON-LISP-3.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
common-lisp-3.eclass - functions to support the installation of Common Lisp libraries
DESCRIPTION
Since Common Lisp libraries share similar structure, this eclass aims to provide a simple way to write ebuilds with these characteristics.
FUNCTIONS
common-lisp-3_src_compile
Since there's nothing to build in most cases, default doesn't do anything.
absolute-path-p
Returns true if ${1} is an absolute path.
common-lisp-install-one-source
Installs ${2} source file in ${3} inside CLSOURCEROOT/CLPACKAGE.
lisp-file-p <file>
Returns true if ${1} is lisp source file.
common-lisp-get-fpredicate <type>
Outputs the corresponding predicate to check files of type ${1}.
common-lisp-install-sources <path> [...]
Recursively install lisp sources of type ${2} if ${1} is -t or Lisp by default. When given a directory, it will be recursively scanned for Lisp source files with suffixes: .lisp, .lsp or .cl.
common-lisp-install-one-asdf <file>
Installs ${1} asdf file in CLSOURCEROOT/CLPACKAGE and symlinks it in CLSYSTEMROOT.
common-lisp-install-asdf <path> [...]
Installs all ASDF files and creates symlinks in CLSYSTEMROOT. When given a directory, it will be recursively scanned for ASDF files with extension .asd.
common-lisp-3_src_install
Recursively install Lisp sources, asdf files and most common doc files.
common-lisp-find-lisp-impl
Outputs an installed Common Lisp implementation. Transverses CLIMPLEMENTATIONS to find it.
common-lisp-export-impl-args <lisp-implementation>
Export a few variables containing the switches necessary to make the CL implementation perform basic functions:
  * CL_BINARY: Common Lisp implementation
  * CL_NORC: don't load syste-wide or user-specific initfiles
  * CL_LOAD: load a certain file
  * CL_EVAL: eval a certain expression at startup
ECLASS VARIABLES
CLIMPLEMENTATIONS = "sbcl clisp clozurecl cmucl ecls gcl abcl"
Common Lisp implementations
CLSOURCEROOT = "${ROOT%/}"/usr/share/common-lisp/source
Default path of Common Lisp libraries sources. Sources will be installed into ${CLSOURCEROOT}/${CLPACKAGE}.
CLSYSTEMROOT = "${ROOT%/}"/usr/share/common-lisp/systems
Default path to find any asdf file. Any asdf files will be symlinked in ${CLSYSTEMROOT}/${CLSYSTEM} as they may be in an arbitrarily deeply nested directory under ${CLSOURCEROOT}/${CLPACKAGE}.
CLPACKAGE = "${PN}"
Default package name. To override, set these after inheriting this eclass.
MAINTAINERS
Common Lisp project <common-lisp@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
common-lisp-3.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/common-lisp-3.eclass
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
Time: 00:27:05 GMT, October 11, 2020