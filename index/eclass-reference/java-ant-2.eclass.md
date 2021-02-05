JAVA-ANT-2.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
java-ant-2.eclass - eclass for ant based Java packages
DESCRIPTION
Eclass for Ant-based Java packages. Provides support for both automatic and manual manipulation of build.xml files. Should be inherited after java-pkg-2 or java-pkg-opt-2 eclass.
FUNCTIONS
java-ant-2_src_configure
src_configure rewrites the build.xml files automatically, unless EAPI is undefined, 0 or 1.
java-ant_bsfix_files <path/to/first/build.xml> [path/to/second.build.xml ...]
Attempts to fix named build files.
Affected by variables:
JAVA_PKG_BSFIX_SOURCE_TAGS
JAVA_PKG_BSFIX_TARGET_TAGS
JAVA_ANT_REWRITE_CLASSPATH
JAVA_ANT_JAVADOC_INPUT_DIRS: Where we can find java sources for javadoc
                               input. Can be a space separated list of
                               directories
JAVA_ANT_BSFIX_EXTRA_ARGS: You can use this to pass extra variables to the
                           rewriter if you know what you are doing.
If JAVA_ANT_JAVADOC_INPUT_DIRS is set, we will turn on the adding of a basic javadoc target to the ant's build.xml with the javadoc xml-rewriter feature. Then we will set EANT DOC TARGET to the added javadoc target NOTE: the variable JAVA_ANT_JAVADOC_OUTPUT_DIR points where we will
      generate the javadocs. This is a read-only variable, dont change it.

java-ant_bsfix_one <path/to/build.xml>
Attempts to fix named build file.
Affected by variables:
JAVA_PKG_BSFIX_SOURCE_TAGS
JAVA_PKG_BSFIX_TARGET_TAGS
java-ant_rewrite-classpath [path/to/build.xml]
Adds 'classpath="${gentoo.classpath}"' to specified build file.
Affected by: JAVA_ANT_CLASSPATH_TAGS

Parameter defaults to build.xml when not specified

java-ant_ignore-system-classes [path/to/build.xml]
Makes the available task ignore classes in the system classpath Parameter defaults to build.xml when not specified
java-ant_xml-rewrite <xml rewriter arguments>
Run the right xml-rewrite binary with the given arguments
java-ant_rewrite-bootclasspath <version> [path/to/build.xml] [prepend] [append]
Adds bootclasspath to javac-like tasks in build.xml filled with jars of a bootclasspath package of given version.
Affected by:
JAVA_PKG_BSFIX_TARGET_TAGS - the tags of javac tasks

Parameters:
$1 - the version of bootclasspath (e.g. 1.5), 'auto' for bootclasspath
     of the current JDK
$2 - path to desired build.xml file, defaults to 'build.xml'
$3 - (optional) what to prepend the bootclasspath with (to override)
$4 - (optional) what to append to the bootclasspath
ECLASS VARIABLES
WANT_ANT_TASKS
An $IFS separated list of ant tasks. Ebuild can specify this variable before inheriting java-ant-2 eclass to determine ANT_TASKS it needs. They will be automatically translated to DEPEND variable and ANT_TASKS variable. JAVA_PKG_FORCE_ANT_TASKS can override ANT_TASKS set by WANT_ANT_TASKS, but not the DEPEND due to caching. Ebuilds that need to depend conditionally on certain tasks and specify them differently for different eant calls can't use this simplified approach. You also cannot specify version or anything else than ant-*.
WANT_ANT_TASKS="ant-junit ant-trax"
JAVA_ANT_DISABLE_ANT_CORE_DEP
Setting this variable non-empty before inheriting java-ant-2 disables adding dev-java/ant-core into DEPEND.
JAVA_PKG_BSFIX = ${JAVA_PKG_BSFIX:-"on"}
Should we attempt to 'fix' ant build files to include the source/target attributes when calling javac?
JAVA_PKG_BSFIX_ALL = ${JAVA_PKG_BSFIX_ALL:-"yes"}
If we're fixing build files, should we try to fix all the ones we can find?
JAVA_PKG_BSFIX_NAME = ${JAVA_PKG_BSFIX_NAME:-"build.xml"}
Filename of build files to fix/search for
JAVA_PKG_BSFIX_TARGET_TAGS = ${JAVA_PKG_BSFIX_TARGET_TAGS:-"javac xjavac javac.preset"}
Targets to fix the 'source' attribute in
JAVA_PKG_BSFIX_SOURCE_TAGS = ${JAVA_PKG_BSFIX_SOURCE_TAGS:-"javadoc javac xjavac javac.preset"}
Targets to fix the 'target' attribute in
JAVA_ANT_CLASSPATH_TAGS = "javac xjavac"
Targets to add the classpath attribute to
JAVA_ANT_IGNORE_SYSTEM_CLASSES
When set, <available> Ant tasks are rewritten to ignore Ant's runtime classpath.
AUTHORS
kiorky (kiorky@cryptelium.net), Petteri Räty (betelgeuse@gentoo.org)
MAINTAINERS
java@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
java-ant-2.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/java-ant-2.eclass
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
Time: 00:27:02 GMT, October 11, 2020