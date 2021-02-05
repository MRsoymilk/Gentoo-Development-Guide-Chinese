RUBY-NG.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
ruby-ng.eclass - An eclass for installing Ruby packages with proper support for multiple Ruby slots.
DESCRIPTION
The Ruby eclass is designed to allow an easier installation of Ruby packages and their incorporation into the Gentoo Linux system.
Currently available targets are listed in ruby-utils.eclass

This eclass does not define the implementation of the configure, compile, test, or install phases. Instead, the default phases are used. Specific implementations of these phases can be provided in the ebuild either to be run for each Ruby implementation, or for all Ruby implementations, as follows:


 * each_ruby_configure
 * all_ruby_configure

SUPPORTED EAPIS
4 5 6 7
FUNCTIONS
ruby_implementation_depend target [comparator [version]]
This function returns the formal package atom for a Ruby implementation.
`target' has to be one of the valid values for USE_RUBY (see above)

Set `comparator' and `version' to include a comparator (=, >=, etc.) and a version string to the returned string

Return value: Package atom of a Ruby implementation to be used in dependencies.

ruby_samelib
Convenience function to output the use dependency part of a dependency. Used as a building block for ruby_add_rdepend() and ruby_add_bdepend(), but may also be useful in an ebuild to specify more complex dependencies.
Return value: use flag string with current ruby implementations

ruby_implementation_command
Not all implementations have the same command basename as the target; This function translate between the two
Return value: the path to the given ruby implementation

ruby_add_rdepend dependencies
Adds the specified dependencies, with use condition(s) to RDEPEND, taking the current set of ruby targets into account. This makes sure that all ruby dependencies of the package are installed for the same ruby targets. Use this function for all ruby dependencies instead of setting RDEPEND yourself. The list of atoms uses the same syntax as normal dependencies.
Note: runtime dependencies are also added as build-time test dependencies.

ruby_add_bdepend dependencies
Adds the specified dependencies, with use condition(s) to DEPEND (or BDEPEND in EAPI7), taking the current set of ruby targets into account. This makes sure that all ruby dependencies of the package are installed for the same ruby targets. Use this function for all ruby dependencies instead of setting DEPEND or BDEPEND yourself. The list of atoms uses the same syntax as normal dependencies.
ruby_add_depend dependencies
Adds the specified dependencies to DEPEND in EAPI7, similar to ruby_add_bdepend.
ruby_get_use_implementations
Gets an array of ruby use targets enabled by the user
ruby_get_use_targets
Gets an array of ruby use targets that the ebuild sets
ruby_implementations_depend
Produces the dependency string for the various implementations of ruby which the package is being built against. This should not be used when RUBY_OPTIONAL is unset but must be used if RUBY_OPTIONAL=yes. Do not confuse this function with ruby_implementation_depend().
Return value: Dependencies suitable for injection into DEPEND and RDEPEND.

ruby-ng_pkg_setup
Check whether at least one ruby target implementation is present.
ruby-ng_src_unpack
Unpack the source archive.
ruby-ng_src_prepare
Apply patches and prepare versions for each ruby target implementation. Also carry out common clean up tasks.
ruby-ng_src_configure
Configure the package.
ruby-ng_src_compile
Compile the package.
ruby-ng_src_test
Run tests for the package.
ruby-ng_src_install
Install the package for each ruby target implementation.
ruby_rbconfig_value rbconfig item
Return value: Returns the value of the given rbconfig item of the Ruby interpreter in ${RUBY}.
doruby file [file...]
Installs the specified file(s) into the sitelibdir of the Ruby interpreter in ${RUBY}.
ruby_get_libruby
Return value: The location of libruby*.so belonging to the Ruby interpreter in ${RUBY}.
ruby_get_hdrdir
Return value: The location of the header files belonging to the Ruby interpreter in ${RUBY}.
ruby_get_version
Return value: The version of the Ruby interpreter in ${RUBY}, or what 'ruby' points to.
ruby_get_implementation
Return value: The implementation of the Ruby interpreter in ${RUBY}, or what 'ruby' points to.
ruby-ng_rspec
This is simply a wrapper around the rspec command (executed by $RUBY}) which also respects TEST_VERBOSE and NOCOLOR environment variables. Optionally takes arguments to pass on to the rspec invocation. The environment variable RSPEC_VERSION can be used to control the specific rspec version that must be executed. It defaults to 2 for historical compatibility.
ruby-ng_cucumber
This is simply a wrapper around the cucumber command (executed by $RUBY}) which also respects TEST_VERBOSE and NOCOLOR environment variables.
ruby-ng_testrb-2
This is simply a replacement for the testrb command that load the test files and execute them, with test-unit 2.x. This actually requires either an old test-unit-2 version or 2.5.1-r1 or later, as they remove their script and we installed a broken wrapper for a while. This also respects TEST_VERBOSE and NOCOLOR environment variables.
ECLASS VARIABLES
USE_RUBY (REQUIRED)
This variable contains a space separated list of targets (see above) a package is compatible to. It must be set before the `inherit' call. There is no default. All ebuilds are expected to set this variable.
RUBY_PATCHES
A String or Array of filenames of patches to apply to all implementations.
RUBY_OPTIONAL
Set the value to "yes" to make the dependency on a Ruby interpreter optional and then ruby_implementations_depend() to help populate BDEPEND, DEPEND and RDEPEND.
RUBY_S
If defined this variable determines the source directory name after unpacking. This defaults to the name of the package. Note that this variable supports a wildcard mechanism to help with github tarballs that contain the commit hash as part of the directory name.
RUBY_QA_ALLOWED_LIBS
If defined this variable contains a whitelist of shared objects that are allowed to exist even if they don't link to libruby. This avoids the QA check that makes this mandatory. This is most likely not what you are looking for if you get the related "Missing links" QA warning, since the proper fix is almost always to make sure the shared object is linked against libruby. There are cases were this is not the case and the shared object is generic code to be used in some other way (e.g. selenium's firefox driver extension). When set this argument is passed to "grep -E" to remove reporting of these shared objects.
AUTHORS
Author: Diego E. Pettenò <flameeyes@gentoo.org>
Author: Alex Legler <a3li@gentoo.org>
Author: Hans de Graaff <graaff@gentoo.org>
MAINTAINERS
Ruby herd <ruby@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
ruby-ng.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/ruby-ng.eclass
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
Time: 00:27:01 GMT, October 11, 2020