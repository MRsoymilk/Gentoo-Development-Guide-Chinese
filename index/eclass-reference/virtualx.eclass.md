VIRTUALX.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
virtualx.eclass - This eclass can be used for packages that needs a working X environment to build.
SUPPORTED EAPIS
4 5 6 7
FUNCTIONS
virtualmake
Function which start new Xvfb session where the VIRTUALX_COMMAND variable content gets executed.
virtx <command> [command arguments]
Start new Xvfb session and run commands in it.
IMPORTANT: The command is run nonfatal !!!

This means we are checking for the return code and raise an exception if it isn't 0. So you need to make sure that all commands return a proper code and not just die. All eclass function used should support nonfatal calls properly.

The rational behind this is the tear down of the started Xfvb session. A straight die would leave a running session behind.

Example:

src_test() {
        virtx default
}
python_test() {
        virtx py.test --verbose
}
my_test() {
  some_command
  return $?
}

src_test() {
  virtx my_test
}
Xmake
Same as "make", but set up the Xvfb hack if needed. Deprecated call.
Xemake
Same as "emake", but set up the Xvfb hack if needed.
Xeconf
Same as "econf", but set up the Xvfb hack if needed.
ECLASS VARIABLES
VIRTUALX_REQUIRED ?= test
Variable specifying the dependency on xorg-server and xhost. Possible special values are "always" and "manual", which specify the dependency to be set unconditionaly or not at all. Any other value is taken as useflag desired to be in control of the dependency (eg. VIRTUALX_REQUIRED="kde" will add the dependency into "kde? ( )" and add kde into IUSE.
VIRTUALX_DEPEND = "${VIRTUALX_DEPEND}
Dep string available for use outside of eclass, in case a more complicated dep is needed. You can specify the variable BEFORE inherit to add more dependencies.
VIRTUALX_COMMAND ?= "emake"
Command (or eclass function call) to be run in the X11 environment (within virtualmake function).
AUTHORS
Original author: Martin Schlemmer <azarah@gentoo.org>
MAINTAINERS
x11@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
virtualx.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/virtualx.eclass
Index
NAME
SUPPORTED EAPIS
FUNCTIONS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:01 GMT, October 11, 2020