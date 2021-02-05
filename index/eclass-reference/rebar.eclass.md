REBAR.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
rebar.eclass - Build Erlang/OTP projects using dev-util/rebar.
DESCRIPTION
An eclass providing functions to build Erlang/OTP projects using dev-util/rebar.
rebar is a tool which tries to resolve dependencies itself which is by cloning remote git repositories. Dependant projects are usually expected to be in sub-directory 'deps' rather than looking at system Erlang lib directory. Projects relying on rebar usually don't have 'install' make targets. The eclass workarounds some of these problems. It handles installation in a generic way for Erlang/OTP structured projects.

SUPPORTED EAPIS
6
FUNCTIONS
get_erl_libs
Get the full path without EPREFIX to Erlang lib directory.
Return value: the path to Erlang lib directory

rebar_disable_coverage [<rebar_config>]
Disable coverage in rebar.config. This is a workaround for failing coverage. Coverage is not relevant in this context, so there's no harm to disable it, although the issue should be fixed.
erebar <targets>
Run rebar with verbose flag. Die on failure.
rebar_fix_include_path <project_name> [<rebar_config>]
Fix path in rebar.config to 'include' directory of dependant project/package, so it points to installation in system Erlang lib rather than relative 'deps' directory.
<rebar_config> is optional. Default is 'rebar.config'.

The function dies on failure.

rebar_remove_deps [<rebar_config>]
Remove dependencies list from rebar.config and deceive build rules that any dependencies are already fetched and built. Otherwise rebar tries to fetch dependencies and compile them.
<rebar_config> is optional. Default is 'rebar.config'.

The function dies on failure.

rebar_set_vsn [<version>]
Set version in project description file if it's not set.
<version> is optional. Default is PV stripped from version suffix.

The function dies on failure.

rebar_src_prepare
Prevent rebar from fetching and compiling dependencies. Set version in project description file if it's not set.
Existence of rebar.config is optional, but file description file must exist at 'src/${PN}.app.src'.

rebar_src_configure
Configure with ERL_LIBS set.
rebar_src_compile
Compile project with rebar.
rebar_src_test
Run unit tests.
rebar_src_install
Install BEAM files, include headers, executables and native libraries. Install standard docs like README or defined in DOCS variable.
Function expects that project conforms to Erlang/OTP structure.

ECLASS VARIABLES
REBAR_APP_SRC = "${REBAR_APP_SRC-src/${PN}.app.src}"
Relative path to .app.src description file.
AUTHORS
Amadeusz Żołnowski <aidecoe@gentoo.org>
MAINTAINERS
maintainer-needed@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
rebar.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/rebar.eclass
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