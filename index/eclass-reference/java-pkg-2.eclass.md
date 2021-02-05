JAVA-PKG-2.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
java-pkg-2.eclass - Eclass for Java Packages
DESCRIPTION
This eclass should be inherited for pure Java packages, or by packages which need to use Java.
FUNCTIONS
java-pkg-2_pkg_setup
pkg_setup initializes the Java environment
java-pkg-2_src_prepare
wrapper for java-utils-2_src_prepare
java-pkg-2_src_compile
Default src_compile for java packages
Variables:
  EANT_BUILD_XML - controls the location of the build.xml (default: ./build.xml)
  EANT_FILTER_COMPILER - Calls java-pkg_filter-compiler with the value
  EANT_BUILD_TARGET - the ant target/targets to execute (default: jar)
  EANT_DOC_TARGET - the target to build extra docs under the doc use flag
                    (default: javadoc; declare empty to disable completely)
  EANT_GENTOO_CLASSPATH - @see eant documention in java-utils-2.eclass
  EANT_EXTRA_ARGS - extra arguments to pass to eant
  EANT_ANT_TASKS - modifies the ANT_TASKS variable in the eant environment
java-pkg-2_src_test
src_test, not exported.
java-pkg-2_pkg_preinst
wrapper for java-utils-2_pkg_preinst
ECLASS VARIABLES
JAVA_PKG_IUSE
Use JAVA_PKG_IUSE instead of IUSE for doc, source and examples so that the eclass can automatically add the needed dependencies for the java-pkg_do* functions.
AUTHORS
Thomas Matthijs <axxo@gentoo.org>
MAINTAINERS
java@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
java-pkg-2.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/java-pkg-2.eclass
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
Time: 00:27:04 GMT, October 11, 2020