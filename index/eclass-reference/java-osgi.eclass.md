JAVA-OSGI.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
java-osgi.eclass - Java OSGi eclass
DESCRIPTION
This eclass provides functionality which is used by packages that need to be OSGi compliant. This means that the generated jars will have special headers in their manifests. Currently this is used only by Eclipse-3.3 - later we could extend this so that Gentoo Java system would be fully OSGi compliant.
FUNCTIONS
@java-osgi_dojar <jar name> <symbolic name> <bundle name> <header name>
Rewrites a jar, and produce an OSGi compliant jar from arguments given on the command line. The arguments given correspond to the minimal set of headers that must be present on a Manifest file of an OSGi package. If you need more headers, you should use the *-fromfile functions below, that create the Manifest from a file. It will call java-pkg_dojar at the end.
java-osgi_dojar "dist/${PN}.jar" "com.jcraft.jsch" "JSch" "com.jcraft.jsch, com.jcraft.jsch.jce;x-internal:=true"
java-osgi_newjar <jar name> <symbolic name> <bundle name> <header name>
Rewrites a jar, and produce an OSGi compliant jar. The arguments given correspond to the minimal set of headers that must be present on a Manifest file of an OSGi package. If you need more headers, you should use the *-fromfile functions below, that create the Manifest from a file. It will call java-pkg_newjar at the end.
java-osgi_newjar "dist/${PN}.jar" "com.jcraft.jsch" "JSch" "com.jcraft.jsch, com.jcraft.jsch.jce;x-internal:=true"
java-osgi_newjar-fromfile <jar to repackage with OSGi> <Manifest file> <bundle name> <version rewriting>
This function produces an OSGi compliant jar from a given manifest file. The Manifest Bundle-Version header will be replaced by the current version of the package, unless the --no-auto-version option is given. It will call java-pkg_newjar at the end.
java-osgi_newjar-fromfile "dist/${PN}.jar" "${FILESDIR}/MANIFEST.MF" "Standard Widget Toolkit for GTK 2.0"
java-osgi_dojar-fromfile <jar to repackage with OSGi> <Manifest file> <bundle name>
This function produces an OSGi compliant jar from a given manifestfile. The Manifest Bundle-Version header will be replaced by the current version of the package, unless the --no-auto-version option is given. It will call java-pkg_dojar at the end.
java-osgi_dojar-fromfile "dist/${PN}.jar" "${FILESDIR}/MANIFEST.MF" "Standard Widget Toolkit for GTK 2.0"
AUTHORS
Java maintainers (java@gentoo.org)
MAINTAINERS
java@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
java-osgi.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/java-osgi.eclass
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
Time: 00:27:02 GMT, October 11, 2020