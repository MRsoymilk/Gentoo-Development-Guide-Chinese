RUBY-NG-GNOME2.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
ruby-ng-gnome2.eclass - An eclass to simplify handling of various ruby-gnome2 parts.
DESCRIPTION
This eclass simplifies installation of the various pieces of ruby-gnome2 since they share a very common installation procedure.
SUPPORTED EAPIS
6 7
FUNCTIONS
each_ruby_configure
Run the configure script in the subbinding for each specific ruby target.
each_ruby_compile
Compile the C bindings in the subbinding for each specific ruby target.
each_ruby_install
Install the files in the subbinding for each specific ruby target.
all_ruby_install
Install the files common to all ruby targets.
each_ruby_test
Run the tests for this package.
ECLASS VARIABLES
RUBY_GNOME2_NEED_VIRTX ?= "no" (SET BEFORE INHERIT)
If set to 'yes', the test is run with virtx. Set before inheriting this eclass.
AUTHORS
Author: Hans de Graaff <graaff@gentoo.org>
MAINTAINERS
Ruby herd <ruby@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
ruby-ng-gnome2.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/ruby-ng-gnome2.eclass
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
Time: 00:27:03 GMT, October 11, 2020