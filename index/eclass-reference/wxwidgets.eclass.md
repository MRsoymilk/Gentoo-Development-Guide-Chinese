WXWIDGETS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
wxwidgets.eclass - Manages build configuration for wxGTK-using packages.
DESCRIPTION
This eclass sets up the proper environment for ebuilds using the wxGTK libraries. Ebuilds using wxPython do not need to inherit this eclass.
More specifically, this eclass controls the configuration chosen by the /usr/bin/wx-config wrapper.

Using the eclass is simple:


  - set WX_GTK_VER equal to a SLOT of wxGTK
  - call setup-wxwidgets()

The configuration chosen is based on the version required and the flags wxGTK was built with.

SUPPORTED EAPIS
0 1 2 3 4 5 6 7
FUNCTIONS
setup-wxwidgets
Call this in your ebuild to set up the environment for wxGTK. Besides controlling the wx-config wrapper this exports WX_CONFIG containing the path to the config in case it needs to be passed to a build system.
In wxGTK-2.9 and later it also controls the level of debugging output from the libraries. In these versions debugging features are enabled by default and need to be disabled at the package level. Because this causes many warning dialogs to pop up during runtime we add -DNDEBUG to CPPFLAGS to disable debugging features (unless your ebuild has a debug USE flag and it's enabled). If you don't like this behavior you can set WX_DISABLE_NDEBUG to override it.

See: http://docs.wxwidgets.org/trunk/overview_debugging.html

MAINTAINERS
wxwidgets@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
wxwidgets.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/wxwidgets.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:02 GMT, October 11, 2020