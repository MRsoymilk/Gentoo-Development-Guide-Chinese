SYSTEMD.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
systemd.eclass - helper functions to install systemd units
DESCRIPTION
This eclass provides a set of functions to install unit files for sys-apps/systemd within ebuilds.
SUPPORTED EAPIS
0 1 2 3 4 5 6 7
EXAMPLE
inherit systemd

src_configure() {
local myconf=(
        --enable-foo
        --with-systemdsystemunitdir="$(systemd_get_systemunitdir)"
)

econf "${myconf[@]}"
}
FUNCTIONS
systemd_get_systemunitdir
Output the path for the systemd system unit directory (not including ${D}). This function always succeeds, even if systemd is not installed.
systemd_get_unitdir
Deprecated alias for systemd_get_systemunitdir.
systemd_get_userunitdir
Output the path for the systemd user unit directory (not including ${D}). This function always succeeds, even if systemd is not installed.
systemd_get_utildir
Output the path for the systemd utility directory (not including ${D}). This function always succeeds, even if systemd is not installed.
systemd_get_systemgeneratordir
Output the path for the systemd system generator directory (not including ${D}). This function always succeeds, even if systemd is not installed.
systemd_dounit <unit>...
Install systemd unit(s). Uses doins, thus it is fatal in EAPI 4 and non-fatal in earlier EAPIs.
systemd_newunit <old-name> <new-name>
Install systemd unit with a new name. Uses newins, thus it is fatal in EAPI 4 and non-fatal in earlier EAPIs.
systemd_douserunit <unit>...
Install systemd user unit(s). Uses doins, thus it is fatal in EAPI 4 and non-fatal in earlier EAPIs.
systemd_newuserunit <old-name> <new-name>
Install systemd user unit with a new name. Uses newins, thus it is fatal in EAPI 4 and non-fatal in earlier EAPIs.
systemd_install_serviced <conf-file> [<service>]
Install <conf-file> as the template <service>.d/00gentoo.conf. If <service> is not specified <conf-file> with the .conf suffix stripped is used (e.g. foo.service.conf -> foo.service.d/00gentoo.conf).
systemd_dotmpfilesd <tmpfilesd>...
Deprecated in favor of tmpfiles.eclass.
Install systemd tmpfiles.d files. Uses doins, thus it is fatal in EAPI 4 and non-fatal in earlier EAPIs.

systemd_newtmpfilesd <old-name> <new-name>.conf
Deprecated in favor of tmpfiles.eclass.
Install systemd tmpfiles.d file under a new name. Uses newins, thus it is fatal in EAPI 4 and non-fatal in earlier EAPIs.

systemd_enable_service <target> <service>
Enable service in desired target, e.g. install a symlink for it. Uses dosym, thus it is fatal in EAPI 4 and non-fatal in earlier EAPIs.
systemd_enable_ntpunit <NN-name> <service>...
Add an NTP service provider to the list of implementations in timedated. <NN-name> defines the newly-created ntp-units.d priority and name, while the remaining arguments list service units that will be added to that file.
Uses doins, thus it is fatal in EAPI 4 and non-fatal in earlier EAPIs.

Doc: https://www.freedesktop.org/wiki/Software/systemd/timedated/

systemd_with_unitdir [<configure-option-name>]
Note: deprecated and banned in EAPI 6. Please use full --with-...= parameter for improved ebuild readability.
Output '--with-systemdsystemunitdir' as expected by systemd-aware configure scripts. This function always succeeds. Its output may be quoted in order to preserve whitespace in paths. systemd_to_myeconfargs() is preferred over this function.

If upstream does use invalid configure option to handle installing systemd units (e.g. `--with-systemdunitdir'), you can pass the 'suffix' as an optional argument to this function (`$(systemd_with_unitdir systemdunitdir)'). Please remember to report a bug upstream as well.

systemd_with_utildir
Note: deprecated and banned in EAPI 6. Please use full --with-...= parameter for improved ebuild readability.
Output '--with-systemdsystemutildir' as used by some packages to install systemd helpers. This function always succeeds. Its output may be quoted in order to preserve whitespace in paths.

systemd_update_catalog
Update the journald catalog. This needs to be called after installing or removing catalog files. This must be called in pkg_post* phases.
If systemd is not installed, no operation will be done. The catalog will be (re)built once systemd is installed.

See: https://www.freedesktop.org/wiki/Software/systemd/catalog

systemd_is_booted
Check whether the system was booted using systemd.
This should be used purely for informational purposes, e.g. warning user that he needs to use systemd. Installed files or application behavior *must not* rely on this. Please remember to check MERGE_TYPE to not trigger the check on binary package build hosts!

Returns 0 if systemd is used to boot the system, 1 otherwise.

See: man sd_booted

systemd_tmpfiles_create <tmpfilesd> ...
Deprecated in favor of tmpfiles.eclass.
Invokes systemd-tmpfiles --create with given arguments. Does nothing if ROOT != / or systemd-tmpfiles is not in PATH. This function should be called from pkg_postinst.

Generally, this function should be called with the names of any tmpfiles fragments which have been installed, either by the build system or by a previous call to systemd_dotmpfilesd. This ensures that any tmpfiles are created without the need to reboot the system.

systemd_reenable <unit> ...
Re-enables units if they are currently enabled. This resets symlinks to the defaults specified in the [Install] section.
This function is intended to fix broken symlinks that result from moving the systemd system unit directory. It should be called from pkg_postinst for system units that define the 'Alias' option in their [Install] section. It is not necessary to call this function to fix dependency symlinks generated by the 'WantedBy' and 'RequiredBy' options.

MAINTAINERS
systemd@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
systemd.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/systemd.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
EXAMPLE
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:01 GMT, October 11, 2020