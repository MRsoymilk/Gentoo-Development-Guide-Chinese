LATEX-PACKAGE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
latex-package.eclass - An eclass for easy installation of LaTeX packages
DESCRIPTION
This eClass is designed to be easy to use and implement. The vast majority of LaTeX packages will only need to define SRC_URI (and sometimes S) for a successful installation. If fonts need to be installed, then the variable SUPPLIER must also be defined.
However, those packages that contain subdirectories must process each subdirectory individually. For example, a package that contains directories DIR1 and DIR2 must call latex-package_src_compile() and latex-package_src_install() in each directory, as shown here:

src_compile() {
   cd ${S}
   cd DIR1
   latex-package_src_compile
   cd ..
   cd DIR2
   latex-package_src_compile }

src_install() {
   cd ${S}
   cd DIR1
   latex-package_src_install
   cd ..
   cd DIR2
   latex-package_src_install }

The eClass automatically takes care of rehashing TeX's cache (ls-lR) after installation and after removal, as well as creating final documentation from TeX files that come with the source. Note that we break TeX layout standards by placing documentation in /usr/share/doc/${PN}

For examples of basic installations, check out dev-tex/aastex and dev-tex/leaflet .

NOTE: The CTAN "directory grab" function creates files with different MD5 signatures EVERY TIME. For this reason, if you are grabbing from the CTAN, you must either grab each file individually, or find a place to mirror an archive of them. (iBiblio)

SUPPORTED EAPIS
7
FUNCTIONS
latex-package_src_doinstall [ module ]
[module] can be one or more of: sh, sty, cls, fd, clo, def, cfg, dvi, ps, pdf, tex, dtx, tfm, vf, afm, pfb, ttf, bst, styles, doc, fonts, bin, or all. If [module] is not given, all is assumed. It installs the files found in the current directory to the standard locations for a TeX installation
latex-package_src_compile
Calls latex for each *.ins in the current directory in order to generate the relevant files that will be installed
latex-package_src_install
Installs the package
latex-package_pkg_postinst
Calls latex-package_rehash to ensure the TeX installation is consistent with the kpathsea database
latex-package_pkg_postrm
Calls latex-package_rehash to ensure the TeX installation is consistent with the kpathsea database
latex-package_rehash
Rehashes the kpathsea database, according to the current TeX installation
ECLASS VARIABLES
SUPPLIER = "misc"
This refers to the font supplier; it should be overridden (see eclass DESCRIPTION above)
LATEX_DOC_ARGUMENTS = ""
When compiling documentation (.tex/.dtx), this variable will be passed to pdflatex as additional argument (e.g. -shell-escape). This variable must be set after inherit, as it gets automatically cleared otherwise.
AUTHORS
Matthew Turk <satai@gentoo.org>
Martin Ehmsen <ehmsen@gentoo.org>
MAINTAINERS
TeX team <tex@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
latex-package.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/latex-package.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:05 GMT, October 11, 2020
