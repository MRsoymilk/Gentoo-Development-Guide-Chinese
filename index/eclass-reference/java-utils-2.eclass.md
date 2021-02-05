JAVA-UTILS-2.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
java-utils-2.eclass - Base eclass for Java packages
DESCRIPTION
This eclass provides functionality which is used by java-pkg-2.eclass, java-pkg-opt-2.eclass and java-ant-2 eclass, as well as from ebuilds.
This eclass should not be inherited this directly from an ebuild. Instead, you should inherit java-pkg-2 for Java packages or java-pkg-opt-2 for packages that have optional Java support. In addition you can inherit java-ant-2 for Ant-based packages.

FUNCTIONS
java-pkg_doexamples [--subdir <subdir>] <file1/dir1> [<file2> ...]
Installs given arguments to /usr/share/doc/${PF}/examples If you give it only one parameter and it is a directory it will install everything in that directory to the examples directory.
Parameters:
--subdir - If the examples need a certain directory structure
$* - list of files to install

Examples:
java-pkg_doexamples demo
java-pkg_doexamples demo/* examples/*
java-pkg_addres <jar> <dir> [<find arguments> ...]
Adds resource files to an existing jar. It is important that the directory given is actually the root of the corresponding resource tree. The target directory as well as sources.lst, MANIFEST.MF, *.class, *.jar, and *.java files are automatically excluded. Symlinks are always followed. Additional arguments are passed through to find.
java-pkg_addres ${PN}.jar resources ! -name "*.html"
java-pkg_rm_files <File1.java> [File2.java] ...
Remove unneeded files in ${S}.
Every now and then, you'll run into situations whereby a file needs removing, be it a unit test or a regular java class.

You can use this function by either: - calling it yourself in src_prepare() and feeding java-pkg_rm_files with the list of files you wish to remove. - defining an array in the ebuild named JAVA_RM_FILES with the list of files you wish to remove.

Both way work and it is left to the developer's preferences. If the JAVA_RM_FILES array is defined, it will be automatically handed over to java-pkg_rm_files during the src_prepare phase.

See java-utils-2_src_prepare.

java-pkg_rm_files File1.java File2.java
java-pkg_dojar <jar1> [<jar2> ...]
Installs any number of jars. Jar's will be installed into /usr/share/${PN}(-${SLOT})/lib/ by default. You can use java-pkg_jarinto to change this path. You should never install a jar with a package version in the filename. Instead, use java-pkg_newjar defined below.
java-pkg_dojar dist/${PN}.jar dist/${PN}-core.jar
java-pkg_regjar </path/to/installed/jar>
Records an already installed (in ${D}) jar in the package.env This would mostly be used if the package has make or a custom script to install things.
WARNING: if you want to use shell expansion, you have to use ${D}/... as the for in this function will not be able to expand the path, here's an example:

  java-pkg_regjar ${D}/opt/my-java/lib/*.jar
java-pkg_newjar <path/to/oldname.jar> [<newname.jar>]
Installs a jar with a new name (defaults to $PN.jar)
For example, installs a versioned jar without the version

java-pkg_addcp <classpath>
Add something to the package's classpath. For jars, you should use dojar, newjar, or regjar. This is typically used to add directories to the classpath. The parameters of this function are appended to JAVA_PKG_CLASSPATH
java-pkg_doso <path/to/file1.so> [...]
Installs any number of JNI libraries They will be installed into /usr/lib by default, but java-pkg_sointo can be used change this path
Example:
java-pkg_doso *.so
java-pkg_regso <file1.so> [...]
Registers an already installed JNI library in package.env.
Parameters:
$@ - JNI libraries to register

Example:
java-pkg_regso *.so /path/*.so
java-pkg_jarinto </path/to/install/jars/into>
Changes the path jars are installed into via subsequent java-pkg_dojar calls.
java-pkg_sointo </path/to/install/sofiles/into>
Changes the path that JNI libraries are installed into via subsequent java-pkg_doso calls.
java-pkg_dohtml <path/to/javadoc/documentation> [...]
Install Javadoc HTML documentation. Usage of java-pkg_dojavadoc is preferred.
java-pkg_dohtml dist/docs/
java-pkg_dojavadoc [--symlink destination] <path/to/javadocs/root>
Installs javadoc documentation. This should be controlled by the doc use flag.
Parameters:
$1: optional --symlink creates to symlink like this for html
           documentation bundles.
$2: - The javadoc root directory.

Examples:
java-pkg_dojavadoc docs/api
java-pkg_dojavadoc --symlink apidocs docs/api
java-pkg_dosrc <path/to/sources> [...]
Installs a zip containing the source for a package, so it can used in from IDEs like eclipse and netbeans. Ebuild needs to DEPEND on app-arch/zip to use this. It also should be controlled by USE=source.
Example:
java-pkg_dosrc src/*
java-pkg_dolauncher <filename> [options]
Make a wrapper script to lauch/start this package If necessary, the wrapper will switch to the appropriate VM.
Can be called without parameters if the package installs only one jar that has the Main-class attribute set. The wrapper will be named ${PN}.

Parameters:
$1 - filename of launcher to create
$2 - options, as follows:
 --main the.main.class.to.start
 --jar /the/jar/too/launch.jar or just <name>.jar
 --java_args 'Extra arguments to pass to java'
 --pkg_args 'Extra arguments to pass to the package'
 --pwd Directory the launcher changes to before executing java
 -into Directory to install the launcher to, instead of /usr/bin
 -pre Prepend contents of this file to the launcher
java-pkg_dowar
Install war files. TODO document
java-pkg_jar-from [--build-only] [--with-dependencies] [--virtual] [--into dir] <package> [<package.jar>] [<destination.jar>]
Makes a symlink to a jar from a certain package A lot of java packages include dependencies in a lib/ directory You can use this function to replace these bundled dependencies. The dependency is recorded into package.env DEPEND line, unless "--build-only" is passed as the very first argument, for jars that have to be present only at build time and are not needed on runtime (junit testing etc).
Example: get all jars from xerces slot 2
java-pkg_jar-from xerces-2

Example: get a specific jar from xerces slot 2
        java-pkg_jar-from xerces-2 xml-apis.jar

Example: get a specific jar from xerces slot 2, and name it diffrently
        java-pkg_jar-from xerces-2 xml-apis.jar xml.jar

Example: get junit.jar which is needed only for building
java-pkg_jar-from --build-only junit junit.jar
Parameters
--build-only - makes the jar(s) not added into package.env DEPEND line.
  (assumed automatically when called inside src_test)
--with-dependencies - get jars also from requested package's dependencies
  transitively.
--virtual - Packages passed to this function are to be handled as virtuals
  and will not have individual jar dependencies recorded.
--into $dir - symlink jar(s) into $dir (must exist) instead of .
$1 - Package to get jars from, or comma-separated list of packages in
case other parameters are not used.
$2 - jar from package. If not specified, all jars will be used.
$3 - When a single jar is specified, destination filename of the
symlink. Defaults to the name of the jar.
java-pkg_jarfrom
See java-pkg_jar-from
java-pkg_getjars [--build-only] [--with-dependencies] <package1>[,<package2>...]
Get the classpath provided by any number of packages Among other things, this can be passed to 'javac -classpath' or 'ant -lib'. The providing packages are recorded as dependencies into package.env DEPEND line, unless "--build-only" is passed as the very first argument, for jars that have to be present only at build time and are not needed on runtime (junit testing etc).
Example: Get the classpath for xerces-2 and xalan,
java-pkg_getjars xerces-2,xalan

Example Return:
/usr/share/xerces-2/lib/xml-apis.jar:/usr/share/xerces-2/lib/xmlParserAPIs.jar:/usr/share/xalan/lib/xalan.jar


Parameters:
--build-only - makes the jar(s) not added into package.env DEPEND line.
  (assumed automatically when called inside src_test)
--with-dependencies - get jars also from requested package's dependencies
  transitively.
$1 - list of packages to get jars from
  (passed to java-config --classpath)
java-pkg_getjar [--build-only] [--virtual] <package> <jarfile>
Get the complete path of a single jar from a package The providing package is recorded as runtime dependency into package.env DEPEND line, unless "--build-only" is passed as the very first argument, for jars that have to be present only at build time and are not needed on runtime (junit testing etc).
Example:
java-pkg_getjar xerces-2 xml-apis.jar
returns
/usr/share/xerces-2/lib/xml-apis.jar

Parameters:
--build-only - makes the jar not added into package.env DEPEND line.
--virtual - Packages passed to this function are to be handled as virtuals
  and will not have individual jar dependencies recorded.
$1 - package to use
$2 - jar to get
java-pkg_register-dependency <package>[,<package2>...] [<jarfile>]
Registers runtime dependency on a package, list of packages, or a single jar from a package, into package.env DEPEND line. Can only be called in src_install phase. Intended for binary packages where you don't need to symlink the jars or get their classpath during build. As such, the dependencies only need to be specified in ebuild's RDEPEND, and should be omitted in DEPEND.
Parameters:
$1 - comma-separated list of packages, or a single package
$2 - if param $1 is a single package, optionally specify the jar
  to depend on

Examples:
Record the dependency on whole xerces-2 and xalan,
java-pkg_register-dependency xerces-2,xalan

Record the dependency on ant.jar from ant-core
java-pkg_register-dependency ant-core ant.jar
Note: Passing both list of packages as the first parameter AND specifying the jar as the second is not allowed and will cause the function to die. We assume that there's more chance one passes such combination as a mistake, than that there are more packages providing identically named jar without class collisions.

java-pkg_register-optional-dependency <package>[,<package2>...] [<jarfile>]
Registers optional runtime dependency on a package, list of packages, or a single jar from a package, into package.env OPTIONAL_DEPEND line. Can only be called in src_install phase. Intended for packages that can use other packages when those are in classpath. Will be put on classpath by launcher if they are installed. Typical case is JDBC implementations for various databases. It's better than having USE flag for each implementation triggering hard dependency.
Parameters:
$1 - comma-separated list of packages, or a single package
$2 - if param $1 is a single package, optionally specify the jar to depend on

Example:
Record the optional dependency on some jdbc providers
java-pkg_register-optional-dependency jdbc-jaybird,jtds-1.2,jdbc-mysql
Note: Passing both list of packages as the first parameter AND specifying the jar as the second is not allowed and will cause the function to die. We assume that there's more chance one passes such combination as a mistake, than that there are more packages providing identically named jar without class collisions.

java-pkg_register-environment-variable <name> <value>
Register an arbitrary environment variable into package.env. The gjl launcher for this package or any package depending on this will export it into environement before executing java command. Must only be called in src_install phase.
java-pkg_get-bootclasspath <version>
Returns classpath of a given bootclasspath-providing package version.
java-pkg_find-normal-jars [<path/to/directory>]
Find the files with suffix .jar file in the given directory (default: $WORKDIR)
java-pkg_ensure-no-bundled-jars
Try to locate bundled jar files in ${WORKDIR} and die if found. This function should be called after WORKDIR has been populated with symlink to system jar files or bundled jars removed.
java-pkg_get-source
Determines what source version should be used, for passing to -source. Unless you want to break things you probably shouldn't set _WANT_SOURCE
java-pkg_get-target
Determines what target version should be used, for passing to -target. If you don't care about lower versions, you can set _WANT_TARGET to the version of your JDK.
java-pkg_get-javac
Returns the compiler executable
java-pkg_javac-args
If an ebuild uses javac directly, instead of using ejavac, it should call this to know what -source/-target to use.
java-pkg_get-jni-cflags
Echos the CFLAGS for JNI compilations
java-pkg_register-ant-task [--version x.y] [<name>]
Register this package as ant task, so that ant will load it when no specific ANT_TASKS are specified. Note that even without this registering, all packages specified in ANT_TASKS will be loaded. Mostly used by the actual ant tasks packages, but can be also used by other ebuilds that used to symlink their .jar into /usr/share/ant-core/lib to get autoloaded, for backwards compatibility.
Parameters
--version x.y Register only for ant version x.y (otherwise for any ant
        version). Used by the ant-* packages to prevent loading of mismatched
        ant-core ant tasks after core was updated, before the tasks are updated,
        without a need for blockers.
$1 Name to register as. Defaults to JAVA_PKG_NAME ($PN[-$SLOT])
ejunit
Junit wrapper function. Makes it easier to run the tests and checks for dev-java/junit in DEPEND. Launches the tests using org.junit.runner.JUnitCore.
Parameters:
$1 - -cp or -classpath
$2 - classpath; junit and recorded dependencies get appended
$@ - the rest of the parameters are passed to java

Examples:
ejunit -cp build/classes org.blinkenlights.jid3.test.AllTests
ejunit org.blinkenlights.jid3.test.AllTests
ejunit org.blinkenlights.jid3.test.FirstTest org.blinkenlights.jid3.test.SecondTest
ejunit4
Junit4 wrapper function. Makes it easier to run the tests and checks for dev-java/junit:4 in DEPEND. Launches the tests using junit.textui.TestRunner.
Parameters:
$1 - -cp or -classpath
$2 - classpath; junit and recorded dependencies get appended
$@ - the rest of the parameters are passed to java

Examples:
ejunit4 -cp build/classes org.blinkenlights.jid3.test.AllTests
ejunit4 org.blinkenlights.jid3.test.AllTests
ejunit4 org.blinkenlights.jid3.test.FirstTest \
        org.blinkenlights.jid3.test.SecondTest
java-utils-2_src_prepare
src_prepare Searches for bundled jars Don't call directly, but via java-pkg-2_src_prepare!
java-utils-2_pkg_preinst
pkg_preinst Searches for missing and unneeded dependencies Don't call directly, but via java-pkg-2_pkg_preinst!
eant <ant_build_target(s)>
Ant wrapper function. Will use the appropriate compiler, based on user-defined compiler. Will also set proper ANT_TASKS from the variable ANT_TASKS, variables:
Variables:
EANT_GENTOO_CLASSPATH - calls java-pkg_getjars for the value and adds to the
                gentoo.classpath property. Be sure to call java-ant_rewrite-classpath in src_unpack.
EANT_NEEDS_TOOLS - add tools.jar to the gentoo.classpath. Should only be used
                for build-time purposes, the dependency is not recorded to
                package.env!
ANT_TASKS - used to determine ANT_TASKS before calling Ant.
ejavac <javac_arguments>
Javac wrapper function. Will use the appropriate compiler, based on /etc/java-config/compilers.conf
ejavadoc <javadoc_arguments>
javadoc wrapper function. Will set some flags based on the VM version due to strict javadoc rules in 1.8.
java-pkg_filter-compiler <compiler(s)_to_filter>
Used to prevent the use of some compilers. Should be used in src_compile. Basically, it just appends onto JAVA_PKG_FILTER_COMPILER
java-pkg_force-compiler <compiler(s)_to_force>
Used to force the use of particular compilers. Should be used in src_compile. A common use of this would be to force ecj-3.1 to be used on amd64, to avoid OutOfMemoryErrors that may come up.
use_doc
Helper function for getting ant to build javadocs. If the user has USE=doc, then 'javadoc' or the argument are returned. Otherwise, there is no return.

The output of this should be passed to ant.

Parameters:
$@ - Option value to return. Defaults to 'javadoc'

Examples:
build javadocs by calling 'javadoc' target
eant $(use_doc)

build javadocs by calling 'apidoc' target
eant $(use_doc apidoc)
java-pkg_clean
Java package cleaner function. This will remove all *.class and *.jar files, removing any bundled dependencies.
ECLASS VARIABLES
JAVA_PKG_WANT_BOOTCLASSPATH
The version of bootclasspath the package needs to work. Translates to a proper dependency. The bootclasspath can then be obtained by java-ant_rewrite-bootclasspath
JAVA_PKG_ALLOW_VM_CHANGE = ${JAVA_PKG_ALLOW_VM_CHANGE:="yes"}
Allow this eclass to change the active VM? If your system VM isn't sufficient for the package, the build will fail instead of trying to switch to another VM.
Overriding the default can be useful for testing specific VMs locally, but should not be used in the final ebuild.

JAVA_PKG_FORCE_VM
Explicitly set a particular VM to use. If its not valid, it'll fall back to whatever /etc/java-config-2/build/jdk.conf would elect to use.
Should only be used for testing and debugging.

Example: use sun-jdk-1.5 to emerge foo:

JAVA_PKG_FORCE_VM=sun-jdk-1.5 emerge foo
JAVA_PKG_WANT_BUILD_VM
A list of VM handles to choose a build VM from. If the list contains the currently active VM use that one, otherwise step through the list till a usable/installed VM is found.
This allows to use an explicit list of JDKs in DEPEND instead of a virtual. Users of this variable must make sure at least one of the listed handles is covered by DEPEND. Requires JAVA_PKG_WANT_SOURCE and JAVA_PKG_WANT_TARGET to be set as well.

JAVA_PKG_WANT_SOURCE
Specify a non-standard Java source version for compilation (via javac -source parameter or Ant equivalent via build.xml rewriting done by java-ant-2 eclass). Normally this is determined from the jdk version specified in DEPEND. See java-pkg_get-source function below.
Should generally only be used for testing and debugging.

Use 1.4 source to emerge baz

JAVA_PKG_WANT_SOURCE=1.4 emerge baz
JAVA_PKG_WANT_TARGET
Same as JAVA_PKG_WANT_SOURCE (see above) but for javac -target parameter, which affects the version of generated bytecode. Normally this is determined from the jre/jdk version specified in RDEPEND. See java-pkg_get-target function below.
Should generallyonly be used for testing and debugging.

emerge bar to be compatible with 1.3

JAVA_PKG_WANT_TARGET=1.3 emerge bar
JAVA_PKG_DEBUG
A variable to be set with "yes" or "y", or ANY string of length non equal to zero. When set, verbosity across java eclasses is increased and extra logging is displayed.
JAVA_PKG_DEBUG="yes"
JAVA_RM_FILES
An array containing a list of files to remove. If defined, this array will be automatically handed over to java-pkg_rm_files for processing during the src_prepare phase.
JAVA_RM_FILES=(
        path/to/File1.java
        DELETEME.txt
)
JAVA_PKG_FORCE_ANT_TASKS
An $IFS separated list of ant tasks. Can be set in environment before calling emerge/ebuild to override variables set in ebuild, mainly for testing before putting the resulting (WANT_)ANT_TASKS into ebuild. Affects only ANT_TASKS in eant() call, not the dependencies specified in WANT_ANT_TASKS.
JAVA_PKG_FORCE_ANT_TASKS="ant-junit ant-trax" \
        ebuild foo.ebuild compile
AUTHORS
Thomas Matthijs <axxo@gentoo.org>, Karl Trygve Kalleberg <karltk@gentoo.org>
MAINTAINERS
java@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
java-utils-2.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/java-utils-2.eclass
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
