JAVA-PKG-SIMPLE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
java-pkg-simple.eclass - Eclass for packaging Java software with ease.
DESCRIPTION
This class is intended to build pure Java packages from Java sources without the use of any build instructions shipped with the sources. There is no support for generating source files, or for controlling the META-INF of the resulting jar, although these issues may be addressed by an ebuild by putting corresponding files into the target directory before calling the src_compile function of this eclass.
FUNCTIONS
java-pkg-simple_src_compile
src_compile for simple bare source java packages. Finds all *.java sources in ${JAVA_SRC_DIR}, compiles them with the classpath calculated from ${JAVA_GENTOO_CLASSPATH}, and packages the resulting classes to a single ${JAVA_JAR_FILENAME}. If the file target/META-INF/MANIFEST.MF exists, it is used as the manifest of the created jar.
If USE FLAG 'binary' exists and is set, it will just copy ${JAVA_BINJAR_FILENAME} to ${S} and skip the rest of src_compile.

java-pkg-simple_src_install
src_install for simple single jar java packages. Simply installs ${JAVA_JAR_FILENAME}. It will also install a launcher if ${JAVA_MAIN_CLASS} is set.
java-pkg-simple_src_test
src_test for simple single java jar file. It will perform test with frameworks that are defined in ${JAVA_TESTING_FRAMEWORKS}.
ECLASS VARIABLES
JAVA_GENTOO_CLASSPATH
Comma or space separated list of java packages to include in the class path. The packages will also be registered as runtime dependencies of this new package. Dependencies will be calculated transitively. See "java-config -l" for appropriate package names.
JAVA_GENTOO_CLASSPATH="foo,bar-2"
JAVA_GENTOO_CLASSPATH_EXTRA
Extra list of colon separated path elements to be put on the classpath when compiling sources.
JAVA_CLASSPATH_EXTRA
An extra comma or space separated list of java packages that are needed only during compiling sources.
JAVA_NEEDS_TOOLS
Add tools.jar to the gentoo.classpath. Should only be used for build-time purposes, the dependency is not recorded to package.env.
JAVA_SRC_DIR
An array of directories relative to ${S} which contain the sources of the application. If you set ${JAVA_SRC_DIR} to a string it works as well. The default value "" means it will get all source files inside ${S}. For the generated source package (if source is listed in ${JAVA_PKG_IUSE}), it is important that these directories are actually the roots of the corresponding source trees.
JAVA_SRC_DIR=( "impl/src/main/java/"
        "arquillian/weld-ee-container/src/main/java/"
)
JAVA_RESOURCE_DIRS
An array of directories relative to ${S} which contain the resources of the application. If you do not set the variable, there will be no resources added to the compiled jar file.
JAVA_RESOURCE_DIRS=("src/java/resources/")
JAVA_ENCODING ?= UTF-8
The character encoding used in the source files.
JAVAC_ARGS
Additional arguments to be passed to javac.
JAVA_MAIN_CLASS
If the java has a main class, you are going to set the variable so that we can generate a proper MANIFEST.MF and create a launcher.
JAVA_MAIN_CLASS="org.gentoo.java.ebuilder.Main"
JAVADOC_ARGS
Additional arguments to be passed to javadoc.
JAVA_JAR_FILENAME ?= ${PN}.jar
The name of the jar file to create and install.
JAVA_BINJAR_FILENAME
The name of the binary jar file to be installed if USE FLAG 'binary' is set.
JAVA_LAUNCHER_FILENAME ?= ${PN}-${SLOT}
If ${JAVA_MAIN_CLASS} is set, we will create a launcher to execute the jar, and ${JAVA_LAUNCHER_FILENAME} will be the name of the script.
JAVA_TESTING_FRAMEWORKS
A space separated list that defines which tests it should launch during src_test.
JAVA_TESTING_FRAMEWORKS="junit pkgdiff"
JAVA_TEST_EXCLUDES
A array of classes that should not be executed during src_test().
JAVA_TEST_EXCLUDES=( "net.sf.cglib.CodeGenTestCase" "net.sf.cglib.TestAll" )
JAVA_TEST_GENTOO_CLASSPATH
The extra classpath we need while compiling and running the source code for testing.
JAVA_TEST_SRC_DIR
An array of directories relative to ${S} which contain the sources for testing. It is almost equivalent to ${JAVA_SRC_DIR} in src_test.
JAVA_TEST_RESOURCE_DIRS
It is almost equivalent to ${JAVA_RESOURCE_DIRS} in src_test.
AUTHORS
Java maintainers (java@gentoo.org)
MAINTAINERS
java@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
java-pkg-simple.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/java-pkg-simple.eclass
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