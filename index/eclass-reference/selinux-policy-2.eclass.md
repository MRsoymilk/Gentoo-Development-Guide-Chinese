SELINUX-POLICY-2.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
selinux-policy-2.eclass - This eclass supports the deployment of the various SELinux modules in sec-policy
DESCRIPTION
The selinux-policy-2.eclass supports deployment of the various SELinux modules defined in the sec-policy category. It is responsible for extracting the specific bits necessary for single-module deployment (instead of full-blown policy rebuilds) and applying the necessary patches.
Also, it supports for bundling patches to make the whole thing just a bit more manageable.

SUPPORTED EAPIS
5 6
FUNCTIONS
selinux-policy-2_src_unpack
Unpack the policy sources as offered by upstream (refpolicy).
selinux-policy-2_src_prepare
Patch the reference policy sources with our set of enhancements. Start with the base patchbundle referred to by the ebuilds through the BASEPOL variable, then apply the additional patches as offered by the ebuild.
Next, extract only those files needed for this particular module (i.e. the .te and .fc files for the given module in the MODS variable).

Finally, prepare the build environments for each of the supported SELinux types (such as targeted or strict), depending on the POLICY_TYPES variable content.

selinux-policy-2_src_compile
Build the SELinux policy module (.pp file) for just the selected module, and this for each SELinux policy mentioned in POLICY_TYPES
selinux-policy-2_src_install
Install the built .pp (or copied .cil) files in the correct subdirectory within /usr/share/selinux.
selinux-policy-2_pkg_postinst
Install the built .pp (or copied .cil) files in the SELinux policy stores, effectively activating the policy on the system.
selinux-policy-2_pkg_postrm
Uninstall the module(s) from the SELinux policy stores, effectively deactivating the policy on the system.
ECLASS VARIABLES
MODS ?= "_illegal"
This variable contains the (upstream) module name for the SELinux module. This name is only the module name, not the category!
BASEPOL ?= ${PVR}
This variable contains the version string of the selinux-base-policy package that this module build depends on. It is used to patch with the appropriate patch bundle(s) that are part of selinux-base-policy.
POLICY_PATCH ?= ""
This variable contains the additional patch(es) that need to be applied on top of the patchset already contained within the BASEPOL variable. The variable can be both a simple string (space-separated) or a bash array.
POLICY_FILES ?= ""
When defined, this contains the files (located in the ebuilds' files/ directory) which should be copied as policy module files into the store. Generally, users would want to include at least a .te and .fc file, but .if files are supported as well. The variable can be both a simple string (space-separated) or a bash array.
POLICY_TYPES ?= "targeted strict mcs mls"
This variable informs the eclass for which SELinux policies the module should be built. Currently, Gentoo supports targeted, strict, mcs and mls. This variable is the same POLICY_TYPES variable that we tell SELinux users to set in make.conf. Therefore, it is not the module that should override it, but the user.
SELINUX_GIT_REPO ?= "https://anongit.gentoo.org/git/proj/hardened-refpolicy.git";
When defined, this variable overrides the default repository URL as used by this eclass. It allows end users to point to a different policy repository using a single variable, rather than having to set the packagename_LIVE_REPO variable for each and every SELinux policy module package they want to install. The default value is Gentoo's hardened-refpolicy repository.
SELINUX_GIT_BRANCH ?= "master";
When defined, this variable sets the Git branch to use of the repository. This allows for users and developers to use a different branch for the entire set of SELinux policy packages, rather than having to override them one by one with the packagename_LIVE_BRANCH variable. The default value is the 'master' branch.
MAINTAINERS
selinux@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
selinux-policy-2.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/selinux-policy-2.eclass
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
Time: 00:27:02 GMT, October 11, 2020