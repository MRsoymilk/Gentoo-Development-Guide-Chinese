GAMES.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
games.eclass - Standardizing the install of games.
DESCRIPTION
This eclass makes sure that games are consistently handled in gentoo. It installs game files by default in FHS-compatible directories like /usr/share/games and sets more restrictive permissions in order to avoid some security bugs.
The installation directories as well as the user and group files are installed as can be controlled by the user. See the variables like GAMES_BINDIR, GAMES_USER etc. below. These are NOT supposed to be set by ebuilds!

For a general guide on writing games ebuilds, see: https://wiki.gentoo.org/wiki/Project:Games/Ebuild_howto

WARNING: This eclass is DEPRECATED and must not be used by new games ebuilds, bug #574082. When writing game ebuilds, no specific eclass is needed. For more details, see the QA team policies page: https://wiki.gentoo.org/wiki/Project:Quality_Assurance/Policies#Games

SUPPORTED EAPIS
0 1 2 3 4 5
FUNCTIONS
games_get_libdir
Gets the directory where to install games libraries. This is in LDPATH.
egamesconf [<args>...]
Games equivalent to 'econf' for autotools based build systems. It passes the necessary games specific directories automatically.
dogamesbin <path>...
Install one or more games binaries.
dogamessbin <path>...
Install one or more games system binaries.
dogameslib <path>...
Install one or more games libraries.
dogameslib.a <path>...
Install one or more static games libraries.
dogameslib.so <path>...
Install one or more shared games libraries.
newgamesbin <path> <newname>
Install one games binary with a new name.
newgamessbin <path> <newname>
Install one system games binary with a new name.
games_make_wrapper <wrapper> <target> [chdir] [libpaths] [installpath]
Create a shell wrapper script named wrapper in installpath (defaults to the games bindir) to execute target (default of wrapper) by first optionally setting LD_LIBRARY_PATH to the colon-delimited libpaths followed by optionally changing directory to chdir.
gamesowners [<args excluding owner/group>...] <path>...
Run 'chown' with the given args on the given files. Owner and group are GAMES_USER and GAMES_GROUP and must not be passed as args.
gamesperms <path>...
Run 'chmod' with games specific permissions on the given files.
prepgamesdirs
Fix all permissions/owners of files in games related directories, usually called at the end of src_install().
games_pkg_setup
Export some toolchain specific variables and create games related groups and users. This function is exported as pkg_setup().
games_src_configure
Runs egamesconf if there is a configure file. This function is exported as src_configure().
games_src_compile
Runs base_src_make(). This function is exported as src_compile().
games_pkg_preinst
Synchronizes GAMES_STATEDIR of the ebuild image with the live filesystem.
games_pkg_postinst
Prints some warnings and infos, also related to games groups.
games_ut_unpack <directory or file to unpack>
Unpack .uz2 files for UT2003/UT2004.
games_umod_unpack <file to unpack>
Unpacks .umod/.ut2mod/.ut4mod files for UT/UT2003/UT2004. Don't forget to set 'dir' and 'Ddir'.
ECLASS VARIABLES
GAMES_PREFIX = ${GAMES_PREFIX:-/usr/games}
Prefix where to install games, mostly used by GAMES_BINDIR. Games data should still go into GAMES_DATADIR. May be set by the user.
GAMES_PREFIX_OPT = ${GAMES_PREFIX_OPT:-/opt}
Prefix where to install precompiled/blob games, usually followed by package name. May be set by the user.
GAMES_DATADIR = ${GAMES_DATADIR:-/usr/share/games}
Base directory where to install game data files, usually followed by package name. May be set by the user.
GAMES_DATADIR_BASE = ${GAMES_DATADIR_BASE:-/usr/share}
Similar to GAMES_DATADIR, but only used when a package auto appends 'games' to the path. May be set by the user.
GAMES_SYSCONFDIR = ${GAMES_SYSCONFDIR:-/etc/games}
Where to install global games configuration files, usually followed by package name. May be set by the user.
GAMES_STATEDIR = ${GAMES_STATEDIR:-/var/games}
Where to install/store global variable game data, usually followed by package name. May be set by the user.
GAMES_LOGDIR = ${GAMES_LOGDIR:-/var/log/games}
Where to store global game log files, usually followed by package name. May be set by the user.
GAMES_BINDIR = ${GAMES_BINDIR:-${GAMES_PREFIX}/bin}
Where to install the game binaries. May be set by the user. This is in PATH.
GAMES_USER = ${GAMES_USER:-root}
The USER who owns all game files and usually has write permissions. May be set by the user.
GAMES_USER_DED = ${GAMES_USER_DED:-games}
The USER who owns all game files related to the dedicated server part of a package. May be set by the user.
GAMES_GROUP = ${GAMES_GROUP:-games}
The GROUP that owns all game files and usually does not have write permissions. May be set by the user. If you want games world-executable, then you can at least set this variable to 'users' which is almost the same.
MAINTAINERS
Games team <games@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
games.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/games.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
ECLASS VARIABLES
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020