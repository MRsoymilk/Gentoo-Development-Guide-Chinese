RUBY-SINGLE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
ruby-single.eclass - An eclass for Ruby packages not installed for multiple implementations.
DESCRIPTION
An eclass for packages which don't support being installed for multiple Ruby implementations. This mostly includes ruby-based scripts. Set USE_RUBY to include all the ruby targets that have been verified to work and include the eclass. RUBY_DEPS is now available to pull in the dependency on the requested ruby targets.
USE_RUBY="ruby20 ruby21"
inherit ruby-single
RDEPEND="${RUBY_DEPS}"
SUPPORTED EAPIS
4 5 6 7
ECLASS VARIABLES
USE_RUBY (REQUIRED)
This variable contains a space separated list of targets (see above) a package is compatible to. It must be set before the `inherit' call. There is no default. All ebuilds are expected to set this variable.
RUBY_DEPS
This is an eclass-generated Ruby dependency string for all implementations listed in USE_RUBY. Any one of the supported ruby targets will satisfy this dependency. A dependency on virtual/rubygems is also added to ensure that this is installed in time for the package to use it.

Example use:

RDEPEND="${RUBY_DEPS}
  dev-foo/mydep"
BDEPEND="${RDEPEND}"
Example value:

|| ( dev-lang/ruby:2.0 dev-lang/ruby:1.9 ) virtual/rubygems
The order of dependencies will change over time to best match the current state of ruby targets, e.g. stable version first.

AUTHORS
Author: Hans de Graaff <graaff@gentoo.org>
Based on python-single-r1 by: Michał Górny <mgorny@gentoo.org>
MAINTAINERS
Ruby team <ruby@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
ruby-single.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/ruby-single.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020
