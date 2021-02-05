ROS-CATKIN.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
ros-catkin.eclass - Template eclass for catkin based ROS packages.
DESCRIPTION
Provides function for building ROS packages on Gentoo. It supports selectively building messages, single-python installation, live ebuilds (git only).
SUPPORTED EAPIS
5 6 7
FUNCTIONS
ros-catkin_src_prepare
Calls cmake_src_prepare (so that PATCHES array is handled there) and initialises the workspace by installing a recursive CMakeLists.txt to handle bundles.
mycatkincmakeargs
Optional cmake defines as a bash array. Should be defined before calling src_configure.
ros-catkin_src_configure
Configures a catkin-based package.
ros-catkin_src_compile
Builds a catkin-based package.
ros-catkin_src_test
Run the tests of a catkin-based package.
ros-catkin_src_install
Installs a catkin-based package.
ECLASS VARIABLES
ROS_REPO_URI
URL of the upstream repository. Usually on github. Serves for fetching tarballs, live ebuilds and inferring the meta-package name.
ROS_SUBDIR
Subdir in which current packages is located. Usually, a repository contains several packages, hence a typical value is: ROS_SUBDIR=${PN}
CATKIN_IN_SOURCE_BUILD
Set to enable in-source build.
CATKIN_HAS_MESSAGES
Set it to a non-empty value before inherit to tell the eclass the package has messages to build. Messages will be built based on ROS_MESSAGES USE_EXPANDed variable.
CATKIN_MESSAGES_TRANSITIVE_DEPS
Some messages have dependencies on other messages. In that case, CATKIN_MESSAGES_TRANSITIVE_DEPS should contain a space-separated list of atoms representing those dependencies. The eclass uses it to ensure proper dependencies on these packages.
CATKIN_MESSAGES_CXX_USEDEP = "ros_messages_cxx(-)"
Use it as cat/pkg[${CATKIN_MESSAGES_CXX_USEDEP}] to indicate a dependency on the C++ messages of cat/pkg.
CATKIN_MESSAGES_PYTHON_USEDEP = "ros_messages_python(-),${PYTHON_SINGLE_USEDEP}"
Use it as cat/pkg[${CATKIN_MESSAGES_PYTHON_USEDEP}] to indicate a dependency on the Python messages of cat/pkg.
CATKIN_MESSAGES_LISP_USEDEP = "ros_messages_lisp(-)"
Use it as cat/pkg[${CATKIN_MESSAGES_LISP_USEDEP}] to indicate a dependency on the Common-Lisp messages of cat/pkg.
CATKIN_MESSAGES_EUS_USEDEP = "ros_messages_eus(-)"
Use it as cat/pkg[${CATKIN_MESSAGES_EUS_USEDEP}] to indicate a dependency on the EusLisp messages of cat/pkg.
CATKIN_MESSAGES_NODEJS_USEDEP = "ros_messages_nodejs(-)"
Use it as cat/pkg[${CATKIN_MESSAGES_NODEJS_USEDEP}] to indicate a dependency on the nodejs messages of cat/pkg.
AUTHORS
Alexis Ballier <aballier@gentoo.org>
MAINTAINERS
ros@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
ros-catkin.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/ros-catkin.eclass
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