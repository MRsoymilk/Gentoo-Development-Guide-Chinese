EBUILD
Section: Portage (5)
Updated: Jul 2019
Index Return to Main Contents
NAME
ebuild - the internal format, variables, and functions in an ebuild script
DESCRIPTION
The ebuild(1) program accepts a single ebuild script as an argument. This script contains variables and commands that specify how to download, unpack, patch, compile, install and merge a particular software package from its original sources. In addition to all of this, the ebuild script can also contain pre/post install/remove commands, as required. All ebuild scripts are written in bash.
Dependencies
A depend atom is simply a dependency that is used by portage when calculating relationships between packages. Please note that if the atom has not already been emerged, then the latest version available is matched.
Atom Bases
The base atom is just a full category/packagename. Another name for a full category/packagename is a "catpkg". You will find this shorthand-name used frequently in Gentoo. Here are some base atoms, a.k.a "catpkgs":
sys-apps/sed
sys-libs/zlib
net-misc/dhcp
Atom Versions
It is nice to be more specific and say that only certain versions of atoms are acceptable. Note that versions must be combined with a prefix (see below). Hence you may add a version number as a postfix to the base.
Examples:

        sys-apps/sed-4.0.5
        sys-libs/zlib-1.1.4-r1
        net-misc/dhcp-3.0_p2
Versions are normally made up of two or three numbers separated by periods, such as 1.2 or 4.5.2. This string may be followed by a character such as 1.2a or 4.5.2z. Note that this letter is not meant to indicate alpha, beta, etc... status. For that, use the optional suffix; either _alpha, _beta, _pre (pre-release), _rc (release candidate), or _p (patch). This means for the 3rd pre-release of a package, you would use something like 1.2_pre3. The suffixes here can be arbitrarily chained without limitation.

Atom Prefix Operators [> >= = <= <]
Sometimes you want to be able to depend on general versions rather than specifying exact versions all the time. Hence we provide standard boolean operators:
Examples:

        >media-libs/libgd-1.6
        >=media-libs/libgd-1.6
        =media-libs/libgd-1.6
        <=media-libs/libgd-1.6
        <media-libs/libgd-1.6
Extended Atom Prefixes [!~] and Postfixes [*]
Now to get even fancier, we provide the ability to define blocking packages and version range matching. Also note that these extended prefixes/postfixes may be combined in any way with the atom classes defined above.
~
means match any revision of the base version specified. So in the example below, we would match versions '1.0.2a', '1.0.2a-r1', '1.0.2a-r2', etc...
Example:

        ~net-libs/libnet-1.0.2a
!
means block packages from being installed at the same time.
Example:

        !app-text/dos2unix
!!
means block packages from being installed at the same time and explicitly disallow them from being temporarily installed simultaneously during a series of upgrades. This syntax is supported beginning with EAPI 2.
Example:

        !!<sys-apps/portage-2.1.4_rc1
*
means match any version of the package so long as the specified base is matched. So with a version of '2*', we can match '2.1', '2.2', '2.2.1', etc... and not match version '1.0', '3.0', '4.1', etc... The version part that comes before the '*' must be a valid version in the absence of the '*'. For example, '2' is a valid version and '2.' is not. Therefore, '2*' is allowed and '2.*' is not.
Examples:

        =dev-libs/glib-2*
        !=net-fs/samba-2*
Atom Slots
Beginning with EAPI 1, any atom can be constrained to match a specific SLOT. This is accomplished by appending a colon followed by a SLOT:
Examples:

        x11-libs/qt:3
        ~x11-libs/qt-3.3.8:3
        >=x11-libs/qt-3.3.8:3
        =x11-libs/qt-3.3*:3
Sub Slots
Beginning with EAPI 5, a slot dependency may contain an optional sub-slot part that follows the regular slot and is delimited by a / character.
Examples:

        dev-libs/icu:0/0
        dev-libs/icu:0/49
        dev-lang/perl:0/5.12
        dev-libs/glib:2/2.30
Atom Slot Operators
Beginning with EAPI 5, slot operator dependency consists of a colon followed by one of the following operators:
*
Indicates that any slot value is acceptable. In addition, for runtime dependencies, indicates that the package will not break if the matched package is uninstalled and replaced by a different matching package in a different slot.
Examples:

        dev-libs/icu:*
        dev-lang/perl:*
        dev-libs/glib:*
=
Indicates that any slot value is acceptable. In addition, for runtime dependencies, indicates that the package will break unless a matching package with slot and sub-slot equal to the slot and sub-slot of the best installed version at the time the package was installed is available.
Examples:

        dev-libs/icu:=
        dev-lang/perl:=
        dev-libs/glib:=
slot=
Indicates that only a specific slot value is acceptable, and otherwise behaves identically to the plain equals slot operator.
Examples:

        dev-libs/icu:0=
        dev-lang/perl:0=
        dev-libs/glib:2=
To implement the equals slot operator, the package manager will need to store the slot/sub-slot pair of the best installed version of the matching package. This syntax is only for package manager use and must not be used by ebuilds. The package manager may do this by inserting the appropriate slot/sub-slot pair between the colon and equals sign when saving the package's dependencies. The sub-slot part must not be omitted here (when the SLOT variable omits the sub-slot part, the package is considered to have an implicit sub-slot which is equal to the regular slot).

Examples:

        dev-libs/icu:0/0=
        dev-libs/icu:0/49=
        dev-lang/perl:0/5.12=
        dev-libs/glib:2/2.30=
Atom USE
Beginning with EAPI 2, any atom can be constrained to match specific USE flag settings. When used together with SLOT dependencies, USE dependencies appear on the right hand side of SLOT dependencies.
Unconditional USE Dependencies
Example	Meaning
foo[bar,baz]	foo must have both bar and baz enabled
foo[-bar,baz]	foo must have bar disabled and baz enabled
Conditional USE Dependencies
Compact Form	Equivalent Expanded Form
foo[!bar?]	bar? ( foo ) !bar? ( foo[-bar] )
foo[bar=]	bar? ( foo[bar] ) !bar? ( foo[-bar] )
foo[!bar=]	bar? ( foo[-bar] ) !bar? ( foo[bar] )
Atom USE defaults
Beginning with EAPI 4, USE dependencies may specify default assumptions about values for flags that may or may not be missing from the IUSE of the matched package. Such defaults are specified by immediately following a flag with either (+) or (-). Use (+) to behave as if a missing flag is present and enabled, or (-) to behave as if it is present and disabled:
Examples:

        media-video/ffmpeg[threads(+)]
        media-video/ffmpeg[-threads(-)]
Dynamic Dependencies
Sometimes programs may depend on different things depending on the USE variable. Portage offers a few options to handle this. Note that when using the following syntaxes, each case is considered as 1 Atom in the scope it appears. That means that each Atom both conditionally include multiple Atoms and be nested to an infinite depth.
usevar? ( Atom )
To include the jpeg library when the user has jpeg in USE, simply use the following syntax:
jpeg? ( media-libs/jpeg )

!usevar? ( Atom )
If you want to include a package only if the user does not have a certain option in their USE variable, then use the following syntax:
!nophysfs? ( dev-games/physfs )

This is often useful for those times when you want to want to add optional support for a feature and have it enabled by default.

usevar? ( Atom if true ) !usevar? ( Atom if false )
For functionality like the tertiary operator found in C you must use two statements, one normal and one inverted. If a package uses GTK2 or GTK1, but not both, then you can handle that like this:
gtk2? ( =x11-libs/gtk+-2* ) !gtk2? ( =x11-libs/gtk+-1* )

That way the default is the superior GTK2 library.

|| ( Atom Atom ... )
When a package can work with a few different packages but a virtual is not appropriate, this syntax can easily be used.
Example:

|| (
        app-games/unreal-tournament
        app-games/unreal-tournament-goty
)
Here we see that unreal-tournament has a normal version and it has a goty version. Since they provide the same base set of files, another package can use either. Adding a virtual is inappropriate due to the small scope of it.

Another good example is when a package can be built with multiple video interfaces, but it can only ever have just one.

Example:

|| (
        sdl? ( media-libs/libsdl )
        svga? ( media-libs/svgalib )
        opengl? ( virtual/opengl )
        ggi? ( media-libs/libggi )
        virtual/x11
)
Here only one of the packages will be chosen, and the order of preference is determined by the order in which they appear. So sdl has the best chance of being chosen, followed by svga, then opengl, then ggi, with a default of X if the user does not specify any of the previous choices.

Note that if any of the packages listed are already merged, the package manager will use that to consider the dependency satisfied.

Variable Usage Notes
*
Variables defined in make.conf(5) are available for use in ebuilds (except Portage-specific variables, which might be not supported by other package managers).
*
When assigning values to variables in ebuilds, you cannot have a space between the variable name and the equal sign.
*
Variable values should only contain characters that are members of the ascii(7) character set. This requirement is mandated by GLEP 31.
Variables Used In Ebuilds
ARCH
This variable contains the official Gentoo-specific acronym for the current architecture of the running system. For an authoritative list please review /var/db/repos/gentoo/profiles/arch.list.
P
This variable contains the package name without the ebuild revision. This variable must NEVER be modified.
Example:

        x11-base/xorg-server-1.20.5-r2 --> 'xorg-server-1.20.5'
PN
Contains the name of the script without the version number.
Example:

        x11-base/xorg-server-1.20.5-r2 --> 'xorg-server'
PV
Contains the version number without the revision.
Example:

        x11-base/xorg-server-1.20.5-r2 --> '1.20.5'
PR
Contains the revision number or 'r0' if no revision number exists.
Example:

        x11-base/xorg-server-1.20.5-r2 --> 'r2'
PVR
Contains the version number with the revision (if non-zero).
Example:

        x11-base/xorg-server-1.20.5 --> '1.20.5'
        x11-base/xorg-server-1.20.5-r2 --> '1.20.5-r2'
PF
Contains the full package name PN-PVR
Examples:

        x11-base/xorg-server-1.20.5 --> 'xorg-server-1.20.5'
        x11-base/xorg-server-1.20.5-r2 --> 'xorg-server-1.20.5-r2'
CATEGORY
Contains the package category name.
Example:

        x11-base/xorg-server-1.20.5-r2 --> 'x11-base'
A
Contains all source files required for the package. This variable must not be defined. It is autogenerated from the SRC_URI variable.
WORKDIR = ${PORTAGE_TMPDIR}/portage/${CATEGORY}/${PF}/work
Contains the path to the package build root. Do not modify this variable.
FILESDIR = ${PORTAGE_TMPDIR}/${CATEGORY}/${PF}/files
Contains the path to the directory in which package-specific auxiliary files are located. Do not modify this variable.
EBUILD_PHASE
Contains the abreviated name of the phase function that is currently executing, such as "setup", "unpack", "compile", or "preinst".
EBUILD_PHASE_FUNC
Beginning with EAPI 5, contains the full name of the phase function that is currently executing, such as "pkg_setup", "src_unpack", "src_compile", or "pkg_preinst".
EPREFIX
Beginning with EAPI 3, contains the offset that this Portage was configured for during installation. The offset is sometimes necessary in an ebuild or eclass, and is available in such cases as ${EPREFIX}. EPREFIX does not contain a trailing slash, therefore an absent offset is represented by the empty string. Do not modify this variable.
S = ${WORKDIR}/${P}
Contains the path to the temporary build directory. This variable is used by the functions src_compile and src_install. Both are executed with S as the current directory. This variable may be modified to match the extraction directory of a tarball for the package.
T = ${PORTAGE_TMPDIR}/portage/${CATEGORY}/${PF}/temp
Contains the path to a temporary directory. You may use this for whatever you like.
D = ${PORTAGE_TMPDIR}/portage/${CATEGORY}/${PF}/image/
Contains the path to the temporary install directory. Every write operation that does not involve the helper tools and functions (found below) should be prefixed with ${D}. Beginning with EAPI 3, the offset prefix often needs to be taken into account here, for which the variable ${ED} is provided (see below). Note that with EAPI 7, any trailing slash contained in this path will be automatically removed for ease of concatenating paths within ebuilds. Do not modify this variable.
ED = ${PORTAGE_TMPDIR}/portage/${CATEGORY}/${PF}/image/${EPREFIX}/
Beginning with EAPI 3, contains the path "${D%/}${EPREFIX}/" for convenience purposes. For EAPI values prior to EAPI 3 which do not support ED, helpers use D where they would otherwise use ED. Note that with EAPI 7, any trailing slash contained in this path will be automatically removed for ease of concatenating paths within ebuilds. Do not modify this variable.
MERGE_TYPE
Beginning with EAPI 4, the MERGE_TYPE variable can be used to query the current merge type. This variable will contain one of the following possible values:
Value	Meaning
buildonly	source-build which is not scheduled for merge
source	source-build which is scheduled for merge
PROVIDES_EXCLUDE = [space delimited list of fnmatch patterns]
Sonames and file paths matched by these fnmatch patterns will be excluded during genertion of PROVIDES metadata (see portage(5)). Patterns are delimited by whitespace, and it is possible to create patterns containing quoted whitespace.
PORTAGE_LOG_FILE
Contains the path of the build log. If PORTAGE_LOGDIR variable is unset then PORTAGE_LOG_FILE="${T}/build.log".
PORTAGE_SOCKS5_PROXY
Contains the UNIX socket path to SOCKSv5 proxy providing host network access. Available only when running inside network-sandbox and a proxy is available (see network-sandbox in make.conf(5)).
REPLACED_BY_VERSION
Beginning with EAPI 4, the REPLACED_BY_VERSION variable can be used in pkg_prerm and pkg_postrm to query the package version that is replacing the current package. If there is no replacement package, the variable will be empty, otherwise it will contain a single version number.
REPLACING_VERSIONS
Beginning with EAPI 4, the REPLACING_VERSIONS variable can be used in pkg_pretend, pkg_setup, pkg_preinst and pkg_postinst to query the package version(s) that the current package is replacing. If there are no packages to replace, the variable will be empty, otherwise it will contain a space-separated list of version numbers corresponding to the package version(s) being replaced. Typically, this variable will not contain more than one version, but according to PMS it can contain more.
REQUIRES_EXCLUDE = [space delimited list of fnmatch patterns]
Sonames and file paths matched by these fnmatch patterns will be excluded during generation of REQUIRES metadata (see portage(5)). Patterns are delimited by whitespace, and it is possible to create patterns containing quoted whitespace.
ROOT = /
Please note an important recent change regarding the ROOT variable -- in EAPI 7 ebuilds, this variable will default to "" rather than the traditional "/" within ebuilds when pointing to the root filesystem, and when set to a non-root path this variable will never contain a trailing slash. This has the added convenience of eliminating the need to specify "${ROOT%/}" when concatenating paths, making the use of ROOT more elegant. This applies to the variables EROOT, D and ED as well. Please make note that this will mean that ebuilds must use different logic in conditionals to determine if they are installing to the root filesystem -- use [[ "$ROOT" == "" ]] instead of [[ "$ROOT" == "/" ]].
Contains the path that portage should use as the root of the live filesystem. When packages wish to make changes to the live filesystem, they should do so in the tree prefixed by ${ROOT}. Often the offset prefix needs to be taken into account here, for which the variable ${EROOT} is provided (see below). Do not modify this variable.

EROOT = ${ROOT%/}${EPREFIX}/
Beginning with EAPI 3, contains "${ROOT%/}${EPREFIX}/" for convenience purposes. Do not modify this variable. Also see the important note regarding ROOT with EAPI 7, above. As with ROOT, EROOT will be defined as "" when set to the root filesystem and never have a trailing slash within an ebuild.
BROOT = ${EPREFIX}
Beginning with EAPI 7, the absolute path to the root directory containing build dependencies satisfied by BDEPEND, typically executable build tools. This includes any applicable offset prefix. Do not modify this variable.
DESCRIPTION = A happy little package
Should contain a short description of the package.
EAPI = 0
Defines the ebuild API version to which this package conforms. If not defined then it defaults to "0". If portage does not recognize the EAPI value then it will mask the package and refuse to perform any operations with it since this means that a newer version of portage needs to be installed first. For maximum backward compatiblity, a package should conform to the lowest possible EAPI. Note that anyone who uses the ebuild(1) and repoman(1) commands with this package will be required to have a version of portage that recognizes the EAPI to which this package conforms.
SRC_URI = https://example.com/path/${P}.tar.gz
Contains a list of URIs for the required source files. It can contain multiple URIs for a single source file. The list is processed in order if the file was not found on any of the GENTOO_MIRRORS. Beginning with EAPI 2, the output file name of a given URI may be customized with a "->" operator on the right hand side, followed by the desired output file name. All tokens, including the operator and output file name, should be separated by whitespace.
HOMEPAGE = https://example.com/
Should contain a list of URIs for the sources main sites and other further package dependent information.
KEYWORDS = [-~][x86,ppc,sparc,mips,alpha,arm,hppa,...]
KEYWORDS works in conjunction with ACCEPT_KEYWORDS (see make.conf(5)) to implement a system of creating sets of different types of packages which can then be masked or unmasked en masse. In the Gentoo Linux project, they are used by the Gentoo arch teams to define what ebuilds are included in a particular CPU architecture's set of stable and unstable unmasked packages.

Here's how they work. For purposes of explanation, let's assume you have a stable x86-64bit system, typically referred to as "amd64". ARCH would be defined as "amd64". If you were using the stable build of Gentoo Linux, then ACCEPT_KEYWORDS would be set to "amd64" via profiles. Any ebuild that then has "amd64" in KEYWORDS will be unmasked by default.

On an "unstable" amd64 system, ACCEPT_KEYWORDS will be set to "amd64 ~amd64", with the tilde denoting "unstable." Then, if an ebuild has either "amd64" or "~amd64" in KEYWORDS, it will be keyword unmasked by default on that system. Similarly, if an ebuild is known to not be compatible with a particular architecture, the "-" prefix ( i.e. "-amd64") setting can be specified to mask it only on that arch. If you are developing ebuilds for Gentoo Linux, there are certain policies regarding KEYWORDS that you are expected to follow in order to align with Gentoo's arch team workflow. The most important policies are listed below:

*
If you do not know if an ebuild runs under a particular arch, then do not specify it in KEYWORDS. It will then be masked by default on that architecture.
*
If the ebuild is known not to work on an arch, disable that arch in KEYWORDS. This would be done by specifying "-ppc", for example. This will ensure that it is explicitly keyword-masked for that architecture.
*
When submitting an ebuild to Gentoo Linux, it is the project policy to only have "~arch" set in KEYWORDS for the architecture for which it has been successfully tested, and no more. As the ebuild receives more testing, Gentoo arch teams will gradually expand the KEYWORDS settings to "bump" the package to unstable, and possibly stable.
It is possible to customize the behavior of ACCEPT_KEYWORDS and KEYWORDS on a per-package basis using package.accept_keywords and package.keywords files in profiles. See portage(5) for more information on using these files.

Note that while other Gentoo-based projects have KEYWORDS and ACCEPT_KEYWORDS, they likely will not have exactly the same policies regarding their use. Therefore, it is necessary that you research their specific policies and how they differ from Gentoo.
SLOT
This sets the SLOT for packages that may need to have multiple versions co-exist. By default you should set SLOT="0". If you are unsure, then do not fiddle with this until you seek some guidance from some guru. This value should NEVER be left undefined.
Beginning with EAPI 5, the SLOT variable may contain an optional sub-slot part that follows the regular slot and is delimited by a / character. The sub-slot must be a valid slot name. The sub-slot is used to represent cases in which an upgrade to a new version of a package with a different sub-slot may require dependent packages to be rebuilt. When the sub-slot part is omitted from the SLOT definition, the package is considered to have an implicit sub-slot which is equal to the regular slot. Refer to the Atom Slot Operators section for more information about sub-slot usage.

LICENSE
This should be a space delimited list of licenses that the package falls under. This _must_ be set to a matching license in /var/db/repos/gentoo/licenses/. If the license does not exist in the repository yet, you must add it first.
IUSE
This should be a list of any and all USE flags that are leveraged within your build script. The only USE flags that should not be listed here are arch related flags (see KEYWORDS). Beginning with EAPI 1, it is possible to prefix flags with + or - in order to create default settings that respectively enable or disable the corresponding USE flags. For details about USE flag stacking order, refer to the USE_ORDER variable in make.conf(5). Given the default USE_ORDER setting, negative IUSE default settings are effective only for negation of repo-level USE settings, since profile and user configuration settings override them.
DEPEND
This should contain a list of all packages that are required for the program to compile (aka buildtime dependencies). These are usually libraries and headers.
Starting from EAPI 7, tools should go into the BDEPEND variable instead, as DEPEND will only be installed into the system being built and hence cannot be executed when cross-compiling.

You may use the syntax described above in the Dependencies section.

RDEPEND
This should contain a list of all packages that are required for this program to run (aka runtime dependencies). These are usually libraries.
In EAPI 3 or earlier, if this is not set, then it defaults to the value of DEPEND. In EAPI 4 or later, RDEPEND will never be implicitly set.

You may use the syntax described above in the Dependencies section.

BDEPEND
This should contain a list of all packages that are required to be executable during compilation of this program (aka native build dependencies). These are usually tools, like interpreters or (cross-)compilers. They will be installed into the system performing the build.
This variable was formally introduced in EAPI 7 but was previously known as HDEPEND in the experimental EAPI 5-hdepend.

You may use the syntax described above in the Dependencies section.

PDEPEND
This should contain a list of all packages that should be merged after this one (aka post merge dependencies), but which may be installed by the package manager at any time, if that is not possible.
***WARNING***
Use this only as last resort to break cyclic dependencies!

You may use the syntax described above in the Dependencies section.

REQUIRED_USE
Beginning with EAPI 4, the REQUIRED_USE variable can be used to specify combinations of USE flags that are allowed or not allowed. Elements can be nested when necessary.
Behavior	Expression
If flag1 enabled then flag2 enabled	flag1? ( flag2 )
If flag1 disabled then flag2 enabled	!flag1? ( flag2 )
If flag1 disabled then flag2 disabled	!flag1? ( !flag2 )
Must enable any one or more (inclusive or)	|| ( flag1 flag2 flag3 )
Must enable exactly one but not more (exclusive or)	^^ ( flag1 flag2 flag3 )
May enable at most one (EAPI 5 or later)	?? ( flag1 flag2 flag3 )
RESTRICT = [strip,mirror,fetch,userpriv]
This should be a space delimited list of portage features to restrict. You may use conditional syntax to vary restrictions as seen above in DEPEND.
binchecks
Disable all QA checks for binaries. This should ONLY be used in packages for which binary checks make no sense (linux-headers and kernel-sources, for example, can safely be skipped since they have no binaries). If the binary checks need to be skipped for other reasons (such as proprietary binaries), see the QA CONTROL VARIABLES section for more specific exemptions.
bindist
Distribution of built packages is restricted.
fetch
like mirror but the files will not be fetched via SRC_URI either.
installsources
Disables installsources for specific packages. This is for packages with binaries that are not compatible with debugedit.
mirror
files in SRC_URI will not be downloaded from the GENTOO_MIRRORS.
network-sandbox
Disables the network namespace for specific packages. Should not be used in the main Gentoo tree.
preserve-libs
Disables preserve-libs for specific packages. Note than when a package is merged, RESTRICT=preserve-libs applies if either the new instance or the old instance sets RESTRICT=preserve-libs.
primaryuri
fetch from URIs in SRC_URI before GENTOO_MIRRORS.
splitdebug
Disables splitdebug for specific packages. This is for packages with binaries that trigger problems with splitdebug, such as file-collisions between symlinks in /usr/lib/debug/.build-id (triggered by bundled libraries).
strip
final binaries/libraries will not be stripped of debug symbols.
test
do not run src_test even if user has FEATURES=test.
userpriv
Disables userpriv for specific packages.
PROPERTIES = [interactive,live]
A space delimited list of properties, with conditional syntax support.
interactive
One or more ebuild phases will produce a prompt that requires user interaction.
live
The package uses live source code that may vary each time that the package is installed.
DOCS
Beginning with EAPI 4, an array or space-delimited list of documentation files for the default src_install function to install using dodoc. If undefined, a reasonable default list is used. See the documentation for src_install below.
QA Control Variables:
Usage Notes
Several QA variables are provided which allow an ebuild to manipulate some of the QA checks performed by portage. Use of these variables in ebuilds should be kept to an absolute minimum otherwise they defeat the purpose of the QA checks, and their use is subject to agreement of the QA team. They are primarily intended for use by ebuilds that install closed-source binary objects that cannot be altered.
Note that objects that violate these rules may fail on some architectures.

QA_PREBUILT
This should contain a list of file paths, relative to the image directory, of files that are pre-built binaries. Paths listed here will be appended to each of the QA_* variables listed below. The paths may contain fnmatch-like patterns which will be internally translated to regular expressions for the QA_* variables that support regular expressions instead of fnmatch patterns. The translation mechanism simply replaces "*" with ".*".
QA_TEXTRELS
This variable can be set to a list of file paths, relative to the image directory, of files that contain text relocations that cannot be eliminated. The paths may contain fnmatch patterns.
This variable is intended to be used on closed-source binary objects that cannot be altered.

QA_EXECSTACK
This should contain a list of file paths, relative to the image directory, of objects that require executable stack in order to run. The paths may contain fnmatch patterns.
This variable is intended to be used on objects that truly need executable stack (i.e. not those marked to need it which in fact do not).

QA_WX_LOAD
This should contain a list of file paths, relative to the image directory, of files that contain writable and executable segments. These are rare. The paths may contain fnmatch patterns.
QA_FLAGS_IGNORED
This should contain a list of file paths, relative to the image directory, of files that do not contain .GCC.command.line sections or contain .hash sections. The paths may contain regular expressions with escape-quoted special characters.
This variable is intended to be used on files of binary packages which ignore CFLAGS, CXXFLAGS, FFLAGS, FCFLAGS, and LDFLAGS variables.

QA_MULTILIB_PATHS
This should contain a list of file paths, relative to the image directory, of files that should be ignored for the multilib-strict checks. The paths may contain regular expressions with escape-quoted special characters.
QA_PRESTRIPPED
This should contain a list of file paths, relative to the image directory, of files that contain pre-stripped binaries. The paths may contain regular expressions with escape-quoted special characters.
QA_SONAME
This should contain a list of file paths, relative to the image directory, of shared libraries that lack SONAMEs. The paths may contain regular expressions with escape-quoted special characters.
QA_SONAME_NO_SYMLINK
This should contain a list of file paths, relative to the image directory, of shared libraries that have SONAMEs but should not have a corresponding SONAME symlink in the same directory. The paths may contain regular expressions with escape-quoted special characters.
QA_AM_MAINTAINER_MODE
This should contain a list of lines containing automake missing --run commands. The lines may contain regular expressions with escape-quoted special characters.
QA_CONFIGURE_OPTIONS
This should contain a list of configure options which trigger warnings about unrecognized options. The options may contain regular expressions with escape-quoted special characters.
QA_DT_NEEDED
This should contain a list of file paths, relative to the image directory, of shared libraries that lack NEEDED entries. The paths may contain regular expressions with escape-quoted special characters.
QA_DESKTOP_FILE
This should contain a list of file paths, relative to the image directory, of desktop files which should not be validated. The paths may contain regular expressions with escape-quoted special characters.
PORTAGE DECLARATIONS
inherit
Inherit is portage's maintenance of extra classes of functions that are external to ebuilds and provided as inheritable capabilities and data. They define functions and set data types as drop-in replacements, expanded, and simplified routines for extremely common tasks to streamline the build process. Call to inherit cannot depend on conditions which can vary in given ebuild. Specification of the eclasses contains only their name and not the .eclass extension. Also note that the inherit statement must come before other variable declarations unless these variables are used in global scope of eclasses.
PHASE FUNCTIONS
pkg_pretend
Beginning with EAPI 4, this function can be defined in order to check that miscellaneous requirements are met. It is called as early as possible, before any attempt is made to satisfy dependencies. If the function detects a problem then it should call eerror and die. The environment (variables, functions, temporary directories, etc..) that is used to execute pkg_pretend is not saved and therefore is not available in phases that execute afterwards.
pkg_nofetch
This function will be executed when the files in SRC_URI cannot be fetched for any reason. If you turn on fetch in RESTRICT, this is useful for displaying information to the user on *how* to obtain said files. All you have to do is output a message and let the function return. Do not end the function with a call to die.
pkg_setup
This function can be used if the package needs specific setup actions or checks to be preformed before anything else.
Initial working directory: $PORTAGE_TMPDIR
src_unpack
This function is used to unpack all the sources in A to WORKDIR. If not defined in the ebuild script it calls unpack ${A}. Any patches and other pre configure/compile modifications should be done here.
Initial working directory: $WORKDIR
src_prepare
All preparation of source code, such as application of patches, should be done here. This function is supported beginning with EAPI 2.
Initial working directory: $S
src_configure
All necessary steps for configuration should be done here. This function is supported beginning with EAPI 2.
Initial working directory: $S
src_compile
With less than EAPI 2, all necessary steps for both configuration and compilation should be done here. Beginning with EAPI 2, only compilation steps should be done here.
Initial working directory: $S
src_test
Run all package specific test cases. The default is to run 'emake check' followed 'emake test'. Prior to EAPI 5, the default src_test implementation will automatically pass the -j1 option as the last argument to emake, and beginning with EAPI 5 it will allow the tests to run in parallel.
Initial working directory: $S
src_install
Should contain everything required to install the package in the temporary install directory.
Initial working directory: $S
Beginning with EAPI 4, if src_install is undefined then the following default implementation is used:

src_install() {
    if [[ -f Makefile || -f GNUmakefile || -f makefile ]] ; then
        emake DESTDIR="${D}" install
    fi

    if ! declare -p DOCS &>/dev/null ; then
        local d
        for d in README* ChangeLog AUTHORS NEWS TODO CHANGES \
                THANKS BUGS FAQ CREDITS CHANGELOG ; do
            [[ -s "${d}" ]] && dodoc "${d}"
        done
    elif [[ $(declare -p DOCS) == "declare -a "* ]] ; then
        dodoc "${DOCS[@]}"
    else
        dodoc ${DOCS}
    fi
}
pkg_preinst pkg_postinst
All modifications required on the live-filesystem before and after the package is merged should be placed here. Also commentary for the user should be listed here as it will be displayed last.
Initial working directory: $PWD
pkg_prerm pkg_postrm
Like the pkg_*inst functions but for unmerge.
Initial working directory: $PWD
pkg_config
This function should contain optional basic configuration steps.
Initial working directory: $PWD
HELPER FUNCTIONS
Phases:
default
Calls the default phase function implementation for the currently executing phase. This function is supported beginning with EAPI 2.
default_*
Beginning with EAPI 2, the default pkg_nofetch and src_* phase functions are accessible via a function having a name that begins with default_ and ends with the respective phase function name. For example, a call to a function with the name default_src_compile is equivalent to a call to the default src_compile implementation.
Default Phase Functions
default_src_unpack
default_src_prepare
default_src_configure
default_src_compile
default_src_test
General:
assert [reason]
Checks the value of the shell's PIPESTATUS array variable, and if any component is non-zero (indicating failure), calls die with reason as a failure message.
die [reason]
Causes the current emerge process to be aborted. The final display will include reason.
Beginning with EAPI 4, all helpers automatically call die whenever some sort of error occurs. Helper calls may be prefixed with the nonfatal helper in order to prevent errors from being fatal.

nonfatal <helper>
Execute helper and do not call die if it fails. The nonfatal helper is available beginning with EAPI 4.
use <USE item>
If USE item is in the USE variable, the function will silently return 0 (aka shell true). If USE item is not in the USE variable, the function will silently return 1 (aka shell false). usev is a verbose version of use.
Example:
if use gnome ; then
        guiconf="--enable-gui=gnome --with-x"
elif use gtk ; then
        guiconf="--enable-gui=gtk --with-x"
elif use X ; then
        guiconf="--enable-gui=athena --with-x"
else
        # No gui version will be built
        guiconf=""
fi
usev <USE item>
Like use, but also echoes USE item when use returns true.
usex <USE flag> [true output] [false output] [true suffix] [false suffix]
If USE flag is set, echo [true output][true suffix] (defaults to "yes"), otherwise echo [false output][false suffix] (defaults to "no"). The usex helper is available beginning with EAPI 5.
use_with <USE item> [configure name] [configure opt]
Useful for creating custom options to pass to a configure script. If USE item is in the USE variable and a configure opt is specified, then the string --with-[configure name]=[configure opt] will be echoed. If configure opt is not specified, then just --with-[configure name] will be echoed. If USE item is not in the USE variable, then the string --without-[configure name] will be echoed. If configure name is not specified, then USE item will be used in its place. Beginning with EAPI 4, an empty configure opt argument is recognized. In EAPI 3 and earlier, an empty configure opt argument is treated as if it weren't provided.
Examples:
USE="opengl"
myconf=$(use_with opengl)
(myconf now has the value "--with-opengl")

USE="jpeg"
myconf=$(use_with jpeg libjpeg)
(myconf now has the value "--with-libjpeg")

USE=""
myconf=$(use_with jpeg libjpeg)
(myconf now has the value "--without-libjpeg")

USE="sdl"
myconf=$(use_with sdl SDL all-plugins)
(myconf now has the value "--with-SDL=all-plugins")
use_enable <USE item> [configure name] [configure opt]
Same as use_with above, except that the configure options are --enable- instead of --with- and --disable- instead of --without-. Beginning with EAPI 4, an empty configure opt argument is recognized. In EAPI 3 and earlier, an empty configure opt argument is treated as if it weren't provided.
has <item> <item list>
If item is in item list, then has returns 0. Otherwise, 1 is returned. There is another version, hasv, that will conditionally echo item.
The item list is delimited by the IFS variable. This variable has a default value of ' ', or a space. It is a bash(1) setting.
hasv <item> <item list>
Like has, but also echoes item when has returns true.
has_version [-b] [-d] [-r] [--host-root] <category/package-version>
Check to see if category/package-version is installed. The parameter accepts all values that are acceptable in the DEPEND variable. The function returns 0 if category/package-version is installed, 1 otherwise. The package is searched for in ROOT by default.
In EAPI 5 and EAPI 6, the package is searched for in the build host if the --host-root option is given.

In EAPI 7 and later, the confusing --host-root option has been replaced with -b, which corresponds to a dependency satisfied by BDEPEND in the build host. Similarly, the -d option corresponds to DEPEND in SYSROOT and the -r option corresponds to RDEPEND in ROOT.

best_version [-b] [-d] [-r] [--host-root] <package name>
This function will look up package name in the database of currently installed packages and echo the "best version" of the package that is found or nothing if no version is installed. The package is searched for in ROOT by default. It accepts the same options as has_version.
Example:

        VERINS="$(best_version net-ftp/glftpd)"
        (VERINS now has the value "net-ftp/glftpd-1.27" if glftpd-1.27 is       installed)
Output:
einfo disposable message
Same as elog, but should be used when the message isn't important to the user (like progress or status messages during the build process).
elog informative message
If you need to display a message that you wish the user to read and take notice of, then use elog. It works just like echo(1), but adds a little more to the output so as to catch the user's eye. The message will also be logged by portage for later review.
ewarn warning message
Same as einfo, but should be used when showing a warning to the user.
eqawarn QA warning message
Same as einfo, but should be used when showing a QA warning to the user.
eerror error message
Same as einfo, but should be used when showing an error to the user.
ebegin helpful message
Like einfo, we output a helpful message and then hint that the following operation may take some time to complete. Once the task is finished, you need to call eend.
eend <status> [error message]
Followup the ebegin message with an appropriate "OK" or "!!" (for errors) marker. If status is non-zero, then the additional error message is displayed.
Unpack:
unpack <source> [list of more sources]
This function uncompresses and/or untars a list of sources into the current directory. The function will append source to the DISTDIR variable.
Compile:
econf [configure options]
This is used as a replacement for configure. Performs:
${ECONF_SOURCE:-.}/configure \
        ${CBUILD:+--build=${CBUILD}} \
        --datadir="${EPREFIX}"/usr/share \
        --host=${CHOST} \
        --infodir="${EPREFIX}"/usr/share/info \
        --localstatedir="${EPREFIX}"/var/lib \
        --prefix="${EPREFIX}"/usr \
        --mandir="${EPREFIX}"/usr/share/man \
        --sysconfdir="${EPREFIX}"/etc \
        ${CTARGET:+--target=${CTARGET}} \
        --disable-dependency-tracking \
        ${EXTRA_ECONF} \
        configure options || die "econf failed"
Note that the EXTRA_ECONF is for users only, not for ebuild writers. If you wish to pass more options to configure, just pass the extra arguments to econf. Also note that econf automatically calls die if the configure script fails. Beginning with EAPI 3, econf uses the ${EPREFIX} variable which is disregarded for prior EAPI values. Beginning with EAPI 4, econf adds --disable-dependency-tracking to the arguments if the string disable-dependency-tracking occurs in the output of configure --help. Beginning with EAPI 5, econf adds disable-silent-rules to the arguments if the string disable-silent-rules occurs in the output of configure --help.
emake [make options]
This must be used in place of `make` in ebuilds. Performs `${MAKE:-make} ${MAKEOPTS} make options ${EXTRA_EMAKE}`, and calls `die` automatically starting with EAPI 4.
The MAKEOPTS variable is set by the user so they can enable features such as parallel builds; see make.conf(5) for more details.

The EXTRA_EMAKE knob is portage feature so developers can override things while debugging ebuilds; it is not part of any EAPI specification.

***WARNING***
You must make sure your build is happy with parallel makes (make -j2). It should be tested thoroughly as parallel makes are notorious for failing _sometimes_ but not always. If you determine that your package fails to build in parallel, and you are unable to resolve the issue, then you should run `emake -j1` explicitly. This is a last resort however as it can significantly slow down builds on systems with lots of processors.

Install:
einstall [make options]
This is used as a replacement for make install. Performs:
make \
        prefix=${ED}/usr \
        datadir=${ED}/usr/share \
        infodir=${ED}/usr/share/info \
        localstatedir=${ED}/var/lib \
        mandir=${ED}/usr/share/man \
        sysconfdir=${ED}/etc \
        ${EXTRA_EINSTALL} \
        make options \
        install
Please do not use this in place of 'emake install DESTDIR=${D}'. That is the preferred way of installing make-based packages. Also, do not utilize the EXTRA_EINSTALL variable since it is for users.
docompress [-x] <path> [list of more paths]
Beginning with EAPI 4, the docompress helper is used to manage lists of files to be included or excluded from optional compression. If the first argument is -x, add each of its subsequent arguments to the exclusion list. Otherwise, add each argument to the inclusion list. The inclusion list initially contains /usr/share/doc, /usr/share/info, and /usr/share/man. The exclusion list initially contains /usr/share/doc/${PF}/html.
The optional compression shall be carried out after src_install has completed, and before the execution of any subsequent phase function. For each item in the inclusion list, pretend it has the value of the D variable prepended, then:

If it is a directory, act as if every file or directory immediately under this directory were in the inclusion list.
If the item is a file, it may be compressed unless it has been excluded as described below.

If the item does not exist, it is ignored.

Whether an item is to be excluded is determined as follows: For each item in the exclusion list, pretend it has the value of the D variable prepended, then:

If it is a directory, act as if every file or directory immediately under this directory were in the exclusion list.
If the item is a file, it shall not be compressed.

If the item does not exist, it is ignored.

dosed s:orig:change:g <filename>
Beginning with EAPI 4, the dosed helper no longer exists. Ebuilds should call sed(1) directly (and assume that it is GNU sed).
Performs sed in place on filename inside ${ED}. If no expression is given then "s:${D}::g" is used as the default expression. Note that this expression does NOT use the offset prefix.
'dosed s:/usr/local:/usr:g /usr/bin/some-script' runs sed on ${ED}/usr/bin/some-script

dodir <path> [more paths]
Creates directories inside of ${ED}.
'dodir /usr/lib/apache' creates ${ED}/usr/lib/apache. Note that the do* functions will run dodir for you. If this directory will be empty when it is merged, then please use keepdir instead.
diropts [options for install(1)]
Can be used to define options for the install function used in dodir. The default is -m0755.
into <path>
Sets the root (DESTTREE) for other functions like dobin, dosbin, doman, doinfo, dolib.
The default root is /usr.
keepdir <path> [more paths]
Similar to dodir, but used to create directories that would otherwise be empty. The treatment of completely-empty directories is undefined by PMS, and using keepdir ensures that they are tracked.
dobin <binary> [list of more binaries]
Installs a binary or a list of binaries into DESTTREE/bin. Creates all necessary dirs.
dosbin <binary> [list of more binaries]
Installs a binary or a list of binaries into DESTTREE/sbin. Creates all necessary dirs.
doinitd <init.d script> [list of more init.d scripts]
Install Gentoo init.d scripts. They will be installed into the correct location for Gentoo init.d scripts (/etc/init.d/). Creates all necessary dirs.
doconfd <conf.d file> [list of more conf.d file]
Install Gentoo conf.d files. They will be installed into the correct location for Gentoo conf.d files (/etc/conf.d/). Creates all necessary dirs.
doenvd <env.d entry> [list of more env.d entries]
Install Gentoo env.d entries. They will be installed into the correct location for Gentoo env.d entries (/etc/env.d/). Creates all necessary dirs.
dolib <library> [list of more libraries]
dolib.a <library> [list of more libraries]
dolib.so <library> [list of more libraries]
Installs a library or a list of libraries into DESTTREE/lib. Creates all necessary dirs.
libopts [options for install(1)]
Can be used to define options for the install function used in the dolib functions. The default is -m0644.
doman [-i18n=<locale>] <man-page> [list of more man-pages]
Installs manual-pages into /usr/share/man/man[0-9n] depending on the manual file ending. The files are compressed if they are not already. You can specify locale-specific manpages with the -i18n option. Then the man-page will be installed into /usr/share/man/<locale>/man[0-9n]. Beginning with EAPI 2, a locale-specific manpage which contains a locale in the file name will be installed in /usr/share/man/<locale>/man[0-9n], with the locale portion of the file name removed, and the -i18n option has no effect. For example, with EAPI 2, a manpage named foo.<locale>.1 will be installed as /usr/share/man/<locale>/man1/foo.1. Beginning with EAPI 4, the -i18n option takes precedence over the locale suffix of the file name.
dohard <filename> <linkname>
Beginning with EAPI 4, the dohard helper no longer exists. Ebuilds should call ln(1) directly.
dosym <filename> <linkname>
Performs the ln command to create a symlink.
doheader [-r] <file> [list of more files]
Installs the given header files into /usr/include/, by default with file mode 0644 (this can be overridden with the insopts function). Setting -r sets recursive. The doheader helper is available beginning with EAPI 5.
dohtml [-a filetypes] [-r] [-x list-of-dirs-to-ignore] [list-of-files-and-dirs]
Installs the files in the list of files (space-separated list) into /usr/share/doc/${PF}/html provided the file ends in .htm, .html, .css, .js, Setting -a limits what types of files will be included, -A appends to the default list, setting -x sets which dirs to exclude (CVS excluded by default), -p sets a document prefix, -r sets recursive.
doinfo <info-file> [list of more info-files]
Installs info-pages into DESTDIR/info. Files are automatically gzipped. Creates all necessary dirs.
domo <locale-file> [list of more locale-files]
Installs locale-files into DESTDIR/usr/share/locale/[LANG] depending on local-file's ending. Creates all necessary dirs.
fowners [-h|-H|-L|-P|-R] [user][:group] <file> [files]
fperms [-R] <permissions> <file> [files]
Performs chown (fowners) or chmod (fperms), applying permissions to files.
insinto [path]
Sets the destination path for the doins function.
The default path is /.
insopts [options for install(1)]
Can be used to define options for the install function used in doins. The default is -m0644.
doins [-r] <file> [list of more files]
Installs files into the path controlled by insinto. This function uses install(1). Creates all necessary dirs. Setting -r sets recursive. Beginning with EAPI 4, both doins and newins preserve symlinks. In EAPI 3 and earlier, symlinks are dereferenced rather than preserved.
exeinto [path]
Sets the destination path for the doexe function.
The default path is /.
exeopts [options for install(1)]
Can be used to define options for the install function used in doexe. The default is -m0755.
doexe <executable> [list of more executables]
Installs executables into the path controlled by exeinto. This function uses install(1). Creates all necessary dirs.
docinto [path]
Sets the subdir used by dodoc and dohtml when installing into the document tree (based in /usr/share/doc/${PF}/). Default is no subdir, or just "".
dodoc [-r] <document> [list of more documents]
Installs a document or a list of documents into /usr/share/doc/${PF}/<docinto path>. Documents are marked for compression. Creates all necessary dirs. Beginning with EAPI 4, there is support for recursion, enabled by the new -r option.
newbin <old file> <new filename>
newsbin <old file> <new filename>
newinitd <old file> <new filename>
newconfd <old file> <new filename>
newenvd <old file> <new filename>
newlib.so <old file> <new filename>
newlib.a <old file> <new filename>
newman <old file> <new filename>
newins <old file> <new filename>
newexe <old file> <new filename>
newdoc <old file> <new filename>
All these functions act like the do* functions, but they only work with one file and the file is installed as [new filename]. Beginning with EAPI 5, standard input is read when the first parameter is - (a hyphen).
EXAMPLES
# Copyright 1999-2013 Gentoo Foundation
# Distributed under the terms of the GNU General Public License v2
# $Header: $

EAPI="5"

inherit some_eclass another_eclass

DESCRIPTION="Super-useful stream editor (sed)"
HOMEPAGE="https://www.gnu.org/software/sed/"
SRC_URI="ftp://alpha.gnu.org/pub/gnu/${PN}/${P}.tar.gz"

LICENSE="GPL-2"
SLOT="0"
KEYWORDS="~x86"
IUSE=""

RDEPEND=""
DEPEND="nls? ( sys-devel/gettext )"

src_configure() {
        econf \
                --bindir="${EPREFIX}"/bin
}

src_install() {
        emake DESTDIR="${D}" install
        dodoc NEWS README* THANKS AUTHORS BUGS ChangeLog
}
FILES
The /usr/lib/portage/bin/ebuild.sh script.
The helper apps in /usr/lib/portage/bin.
/etc/portage/make.conf
Contains variables for the build-process and overwrites those in make.defaults.
/usr/share/portage/config/make.globals
Contains the default variables for the build-process, you should edit /etc/portage/make.conf instead.
/etc/portage/color.map
Contains variables customizing colors.
SEE ALSO
ebuild(1), make.conf(5), color.map(5)
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
AUTHORS
Achim Gottinger <achim@gentoo.org>
Mark Guertin <gerk@gentoo.org>
Nicholas Jones <carpaski@gentoo.org>
Mike Frysinger <vapier@gentoo.org>
Arfrever Frehtes Taifersar Arahesis <arfrever@apache.org>
Fabian Groffen <grobian@gentoo.org>
Index
NAME
DESCRIPTION
Dependencies
Variable Usage Notes
Variables Used In Ebuilds
QA Control Variables:
PORTAGE DECLARATIONS
PHASE FUNCTIONS
HELPER FUNCTIONS
Phases:
General:
Output:
Unpack:
Compile:
Install:
EXAMPLES
FILES
SEE ALSO
REPORTING BUGS
AUTHORS
This document was created by man2html, using the manual pages.
Time: 21:27:06 GMT, October 10, 2020
