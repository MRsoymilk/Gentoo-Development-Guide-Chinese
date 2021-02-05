GSTREAMER.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
gstreamer.eclass - Helps building core & split gstreamer plugins.
DESCRIPTION
Eclass to make external gst-plugins emergable on a per-plugin basis and to solve the problem with gst-plugins generating far too much unneeded dependencies.
GStreamer consuming applications should depend on the specific plugins they need as defined in their source code. Usually you can find that out by grepping the source tree for 'factory_make'. If it uses playbin plugin, consider adding media-plugins/gst-plugins-meta dependency, but also list any packages that provide explicitly requested plugins.

SUPPORTED EAPIS
5 6
FUNCTIONS
gstreamer_system_link <gst-libs/gst/audio:gstreamer-audio> [...]
Walks through makefiles in order to make sure build will link against system libraries. Takes a list of path fragments and corresponding pkgconfig libraries separated by colon (:). Will replace the path fragment by the output of pkgconfig.
gstreamer_multilib_src_configure
Handles logic common to configuring gstreamer plugins
gstreamer_multilib_src_compile
Compiles requested gstreamer plugin.
gstreamer_multilib_src_install
Installs requested gstreamer plugin.
gstreamer_multilib_src_install_all
Installs documentation for requested gstreamer plugin, and removes .la files.
ECLASS VARIABLES
GST_PLUGINS_BUILD ?= ${PN/gst-plugins-/}
Defines the plugins to be built. May be set by an ebuild and contain more than one indentifier, space seperated (only src_configure can handle mutiple plugins at this time).
GST_PLUGINS_BUILD_DIR ?= ${PN/gst-plugins-/}
Actual build directory of the plugin. Most often the same as the configure switch name.
GST_TARBALL_SUFFIX ?= "xz"
Most projects hosted on gstreamer.freedesktop.org mirrors provide tarballs as tar.bz2 or tar.xz. This eclass defaults to xz. This is because the gstreamer mirrors are moving to only have xz tarballs for new releases.
GST_ORG_MODULE ?= $PN
Name of the module as hosted on gstreamer.freedesktop.org mirrors. Leave unset if package name matches module name.
AUTHORS
Michał Górny <mgorny@gentoo.org>
Gilles Dartiguelongue <eva@gentoo.org>
Saleem Abdulrasool <compnerd@gentoo.org>
foser <foser@gentoo.org>
zaheerm <zaheerm@gentoo.org>
MAINTAINERS
gstreamer@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
gstreamer.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/gstreamer.eclass
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