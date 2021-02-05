DESKTOP.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
desktop.eclass - support for desktop files, menus, and icons
FUNCTIONS
make_desktop_entry <command> [name] [icon] [type] [fields]
Make a .desktop file.
binary:   what command does the app run with ?
name:     the name that will show up in the menu
icon:     the icon to use in the menu entry
          this can be relative (to /usr/share/pixmaps) or
          a full path to an icon
type:     what kind of application is this?
          for categories:
          https://specifications.freedesktop.org/menu-spec/latest/apa.html
          if unset, function tries to guess from package's category
fields: extra fields to append to the desktop file; a printf string
make_session_desktop <title> <command> [command args...]
Make a GDM/KDM Session file. The title is the file to execute to start the Window Manager. The command is the name of the Window Manager.
You can set the name of the file via the ${wm} variable.

domenu <menus>
Install the list of .desktop menu files into the appropriate directory (/usr/share/applications).
newmenu <menu> <newname>
Like all other new* functions, install the specified menu as newname.
doicon [options] <icons>
Install icon into the icon directory /usr/share/icons or into /usr/share/pixmaps if "--size" is not set. This is useful in conjunction with creating desktop/menu files.
 options:
 -s, --size
   !!! must specify to install into /usr/share/icons/... !!!
   size of the icon, like 48 or 48x48
   supported icon sizes are:
   16 22 24 32 36 48 64 72 96 128 192 256 512 scalable
 -c, --context
   defaults to "apps"
 -t, --theme
   defaults to "hicolor"

icons: list of icons

example 1: doicon foobar.png fuqbar.svg suckbar.png
results in: insinto /usr/share/pixmaps
            doins foobar.png fuqbar.svg suckbar.png

example 2: doicon -s 48 foobar.png fuqbar.png blobbar.png
results in: insinto /usr/share/icons/hicolor/48x48/apps
            doins foobar.png fuqbar.png blobbar.png
newicon [options] <icon> <newname>
Like doicon, install the specified icon as newname.
example 1: newicon foobar.png NEWNAME.png
results in: insinto /usr/share/pixmaps
            newins foobar.png NEWNAME.png

example 2: newicon -s 48 foobar.png NEWNAME.png
results in: insinto /usr/share/icons/hicolor/48x48/apps
            newins foobar.png NEWNAME.png
MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
desktop.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/desktop.eclass
Index
NAME
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:02 GMT, October 11, 2020