PERL-FUNCTIONS.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
perl-functions.eclass - helper functions eclass for perl modules
DESCRIPTION
The perl-functions eclass is designed to allow easier installation of perl modules, and their incorporation into the Gentoo Linux system. It provides helper functions, no phases or variable manipulation in global scope.
SUPPORTED EAPIS
5 6 7
FUNCTIONS
perl_set_version
Extract version information and installation paths from the current Perl interpreter.
This sets the following variables: PERL_VERSION, SITE_ARCH, SITE_LIB, ARCH_LIB, VENDOR_LIB, VENDOR_ARCH

This function used to be called perlinfo as well.

Example:

perl_set_version
echo $PERL_VERSION
perl_delete_localpod
Remove stray perllocal.pod files in the temporary install directory D.
This function used to be called fixlocalpod as well.

perl_fix_osx_extra
Look through ${S} for AppleDouble encoded files and get rid of them.
perl_delete_module_manpages
Bump off manpages installed by the current module such as *.3pm files as well as empty directories.
perl_delete_packlist
Look through ${D} for .packlist files, empty .bs files and empty directories, and get rid of items found.
perl_delete_emptybsdir
Look through ${D} for empty .bs files and empty directories, and get rid of items found.
perl_fix_packlist
Look through ${D} for .packlist text files containing the temporary installation folder (i.e. ${D}). If the pattern is found, silently replace it with `/'. Remove duplicate entries; then validate all entries in the packlist against ${D} and prune entries that do not correspond to installed files.
perl_remove_temppath
Look through ${D} for text files containing the temporary installation folder (i.e. ${D}). If the pattern is found, replace it with `/' and warn.
perl_rm_files <list of files>
Remove certain files from a Perl release and remove them from the MANIFEST while we're there.
Most useful in src_prepare for nuking bad tests, and is highly recommended for any tests like 'pod.t', 'pod-coverage.t' or 'kwalitee.t', as what they test is completely irrelevant to end users, and frequently fail simply because the authors of Test::Pod... changed their recommendations, and thus failures are only useful feedback to Authors, not users.

Removing from MANIFEST also avoids needless log messages warning users about files "missing from their kit".

Example:

src_test() {
  perl_rm_files t/pod{,-coverage}.t
  perl-module_src_test
}
perl_link_duallife_scripts
Moves files and generates symlinks so dual-life packages installing scripts do not lead to file collisions. Mainly for use in pkg_postinst and pkg_postrm, and makes only sense for perl-core packages.
perl_check_env
Checks a blacklist of known-suspect ENV values that can be accidentally set by users doing personal perl work, which may accidentally leak into portage and break the system perl installaton. Dies if any of the suspect fields are found, and tell the user what needs to be unset. There's a workaround, but you'll have to read the code for it.
perl_doexamples <list of files or globs>
Install example files ready-to-run. Is called under certain circumstances in perl-module.eclass src_install (see the documentation there).
Example:

src_install() {
  perl-module_src_install
  use examples && perl_doexamples "eg/*"
}
perl_has_module <module name>
Query the installed system Perl to see if a given module is installed. This does **not** load the module in question, only anticipates if it *might* load.
This is primarily for the purposes of dependency weakening so that conditional behaviour can be triggered without adding dependencies to portage which would confuse a dependency resolver.

returns 'true' if the module is available, returns error if the module is not available

Example:

perl_has_module "Test::Tester" && echo "Test::Tester installed"
Return value: 0 if available, non-zero otherwise

perl_has_module_version <module name> <minimum upstream version>
Query the installed system Perl to see if a given module is installed and is at least a given version.
This requires more caution to use than perl_has_module as it requires loading the module in question to determine version compatibility, which can be SLOW, and can have side effects (ie: compilation fails in require due to some dependency, resulting in a "Fail")

Also take care to note the module version is a *minimum*, *must* be written in upstream versions format and should be a a legal upstream version

returns a true exit code if the module is both available and is at least the specified version

Example:

perl_has_module_version "Test::Tester" "0.017" \
&& echo "Test::Tester 0.017 or greater installed"
Return value: 0 if satisfied, non-zero otherwise

perl_get_module_version <module name>
Query the installed system perl to report the version of the installed module.
Note this should be strictly for diagnostic purposes to the end user, and may be of selective use in pkg_info to enhance emerge --info reports.

Anything that does version comparisons **must not** use the return value from this function

Also note that this **must** at least attempt load the module in question as part of its operation, and is subsequently prone to SLOWness.

Return codes return error in both compilation-failure and not-installed cases.

Example:

MODVER=$(perl_get_module_version "Test::Simple") \
|| die "Test::Simple not installed: $MODVER"
Return value: 0 if module available, non-zero if error

perl_get_raw_vendorlib
Convenience function to optimise for a common case without double-handling variables everywhere.
Note: Will include EPREFIX where relevant

Example:

my_raw_vendorlib="$(perl_get_raw_vendorlib)"
perl_get_vendorlib
Convenience helper for returning Perls' vendor install root without EPREFIXing.
Example:

my_vendorlib="$(perl_get_vendorlib)"
perl_domodule [-C <target>] [-r] <files>
Installs files in paths where they can be found in the default Perl runtime.
Note: Should only be used in src_install or pkg_preinst anywhere else will do the wrong thing or die.

The contents of the <files> list are copied into Perls Vendor library path as follows:

  # install perl/File.pm as Samba::File
  pushd perl/
  perl_domodule -C Samba File.pm

  # install perl/ recursively under VENDORLIB/Samba/
  pushd perl/
  perl_domodule -C Samba -r .
  options:
      -C Target/Name
         The subdirectory relative to the Perl VENDOR_LIB
         to install into.

         defaults to ""
      -r
         Install directories recursively ( see doins )
         files:
         list of .pm files to install to VENDORLIB
AUTHORS
Seemant Kulleen <seemant@gentoo.org>
Andreas K. Huettel <dilfridge@gentoo.org>
Kent Fredric <kentnl@gentoo.org>
MAINTAINERS
perl@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
perl-functions.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/perl-functions.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
FUNCTIONS
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:02 GMT, October 11, 2020