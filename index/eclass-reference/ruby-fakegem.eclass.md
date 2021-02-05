RUBY-FAKEGEM.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
ruby-fakegem.eclass - An eclass for installing Ruby packages to behave like RubyGems.
DESCRIPTION
This eclass allows to install arbitrary Ruby libraries (including Gems), providing integration into the RubyGems system even for "regular" packages.
SUPPORTED EAPIS
4 5 6 7
FUNCTIONS
ruby_fakegem_gemsdir
This function returns the gems data directory for the ruby implementation in question.
Return value: Returns the gem data directory

ruby_fakegem_doins <file> [file...]
Installs the specified file(s) into the gems directory.
ruby_fakegem_newins <file> <newname>
Installs the specified file into the gems directory using the provided filename.
ruby_fakegem_install_gemspec
Install a .gemspec file for this package. Either use the file indicated by the RUBY_FAKEGEM_GEMSPEC variable, or generate one using ruby_fakegem_genspec.
ruby_fakegem_gemspec_gemspec <gemspec-input> <gemspec-output>
Generates an installable version of the specification indicated by RUBY_FAKEGEM_GEMSPEC. This file is eval'ed to produce a final specification in a way similar to packaging the gemspec file.
ruby_fakegem_metadata_gemspec <gemspec-metadata> <gemspec-output>
Generates an installable version of the specification indicated by the metadata distributed by the gem itself. This is similar to how rubygems creates an installation from a .gem file.
ruby_fakegem_genspec <output-gemspec>
Generates a gemspec for the package and places it into the "specifications" directory of RubyGems. If the metadata normally distributed with a gem is present then that is used to generate the gemspec file.
As a fallback we can generate our own version. In the gemspec, the following values are set: name, version, summary, homepage, and require_paths=["lib"]. See RUBY_FAKEGEM_NAME and RUBY_FAKEGEM_VERSION for setting name and version. See RUBY_FAKEGEM_REQUIRE_PATHS for setting extra require paths.

ruby_fakegem_binwrapper <command> [path] [content]
Creates a new binary wrapper for a command installed by the RubyGem. path defaults to /usr/bin/$command content is optional and can be used to inject additional ruby code into the wrapper. This may be useful to e.g. force a specific version using the gem command.
all_fakegem_compile
Build documentation for the package if indicated by the doc USE flag and if there is a documetation task defined.
all_ruby_unpack
Unpack the source archive, including support for unpacking gems.
all_ruby_compile
Compile the package.
each_fakegem_test
Run tests for the package for each ruby target if the test task is defined.
each_fakegem_install
Install the package for each ruby target.
each_ruby_install
Install the package for each target.
all_fakegem_install
Install files common to all ruby targets.
all_ruby_install
Install files common to all ruby targets.
ECLASS VARIABLES
RUBY_FAKEGEM_NAME = "${RUBY_FAKEGEM_NAME:-${PN}}"
Sets the Gem name for the generated fake gemspec. This variable MUST be set before inheriting the eclass.
RUBY_FAKEGEM_VERSION = "${RUBY_FAKEGEM_VERSION:-${PV/_pre/.pre}}"
Sets the Gem version for the generated fake gemspec. This variable MUST be set before inheriting the eclass.
RUBY_FAKEGEM_TASK_DOC = "${RUBY_FAKEGEM_TASK_DOC-rdoc}"
Specify the rake(1) task to run to generate documentation.
RUBY_FAKEGEM_RECIPE_TEST = "${RUBY_FAKEGEM_RECIPE_TEST-rake}"
Specify one of the default testing function for ruby-fakegem:
 - rake (default; see also RUBY_FAKEGEM_TASK_TEST)
 - rspec (calls ruby-ng_rspec, adds dev-ruby/rspec:2 to the dependencies)
 - rspec3 (calls ruby-ng_rspec, adds dev-ruby/rspec:3 to the dependencies)
 - cucumber (calls ruby-ng_cucumber, adds dev-util/cucumber to the
   dependencies)
 - none
RUBY_FAKEGEM_TASK_TEST = "${RUBY_FAKEGEM_TASK_TEST-test}"
Specify the rake(1) task used for executing tests. Only valid if RUBY_FAKEGEM_RECIPE_TEST is set to "rake" (the default).
RUBY_FAKEGEM_RECIPE_DOC
Specify one of the default API doc building function for ruby-fakegem:
 - rake (default; see also RUBY_FAKEGEM_TASK_DOC)
 - rdoc (calls `rdoc-2`, adds dev-ruby/rdoc to the dependencies);
 - yard (calls `yard`, adds dev-ruby/yard to the dependencies);
 - none
RUBY_FAKEGEM_DOCDIR
Specify the directory under which the documentation is built; if empty no documentation will be installed automatically. Note: if RUBY_FAKEGEM_RECIPE_DOC is set to `rdoc`, this variable is hardwired to `doc`.
RUBY_FAKEGEM_EXTRADOC
Extra documentation to install (readme, changelogs, …).
RUBY_FAKEGEM_DOC_SOURCES = "${RUBY_FAKEGEM_DOC_SOURCES-lib}"
Allow settings defined sources to scan for documentation. This only applies if RUBY_FAKEGEM_DOC_TASK is set to `rdoc`.
RUBY_FAKEGEM_BINWRAP = "${RUBY_FAKEGEM_BINWRAP-*}"
Binaries to wrap around (relative to the RUBY_FAKEGEM_BINDIR directory)
RUBY_FAKEGEM_BINDIR = "${RUBY_FAKEGEM_BINDIR-bin}"
Path that contains binaries to be binwrapped. Equivalent to the gemspec bindir option.
RUBY_FAKEGEM_REQUIRE_PATHS
Extra require paths (beside lib) to add to the specification
RUBY_FAKEGEM_GEMSPEC
Filename of .gemspec file to install instead of generating a generic one.
RUBY_FAKEGEM_EXTRAINSTALL
List of files and directories relative to the top directory that also get installed. Some gems provide extra files such as version information, Rails generators, or data that needs to be installed as well.
AUTHORS
Author: Diego E. Pettenò <flameeyes@gentoo.org>
Author: Alex Legler <a3li@gentoo.org>
Author: Hans de Graaff <graaff@gentoo.org>
MAINTAINERS
Ruby herd <ruby@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
ruby-fakegem.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/ruby-fakegem.eclass
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