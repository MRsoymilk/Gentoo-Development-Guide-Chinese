ANT-TASKS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
ant-tasks.eclass - Eclass for building dev-java/ant-* packages
DESCRIPTION
This eclass provides functionality and default ebuild variables for building dev-java/ant-* packages easily.
SUPPORTED EAPIS
6 7
FUNCTIONS
ant-tasks_src_unpack [ base ] [ jar-dep ] [ all ]
The function Is split into two parts, defaults to both of them ('all').
base: performs the unpack, build.xml replacement and symlinks ant.jar from ant-core

jar-dep: symlinks the jar file(s) from dependency package(s)

ant-tasks_src_compile
Compiles the jar with installed ant-core.
ant-tasks_src_install
Installs the jar and registers its presence for the ant launcher script. Version param ensures it won't get loaded (thus break) when ant-core is updated to newer version.
ECLASS VARIABLES
ANT_TASK_JDKVER = ${ANT_TASK_JDKVER-1.8}
Affects the >=virtual/jdk version set in DEPEND string. Defaults to 1.8, can be overridden from ebuild BEFORE inheriting this eclass.
ANT_TASK_JREVER = ${ANT_TASK_JREVER-1.8}
Affects the >=virtual/jre version set in DEPEND string. Defaults to 1.8, can be overridden from ebuild BEFORE inheriting this eclass.
ANT_TASK_NAME = "${PN#ant-}"
The name of this ant task as recognized by ant's build.xml, derived from $PN by removing the ant- prefix. Read-only.
ANT_TASK_DEPNAME = ${ANT_TASK_DEPNAME-${ANT_TASK_NAME}}
Specifies JAVA_PKG_NAME (PN{-SLOT} used with java-pkg_jar-from) of the package that this one depends on. Defaults to the name of ant task, ebuild can override it before inheriting this eclass. In case there is more than one dependency, the variable can be specified as bash array with multiple strings, one for each dependency.
ANT_TASK_DISABLE_VM_DEPS
If set, no JDK/JRE deps are added.
AUTHORS
Vlastimil Babka <caster@gentoo.org>
MAINTAINERS
java@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
ant-tasks.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/ant-tasks.eclass
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
Time: 00:27:04 GMT, October 11, 2020