ELISP-COMMON.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
elisp-common.eclass - Emacs-related installation utilities
DESCRIPTION
Usually you want to use this eclass for (optional) GNU Emacs support of your package. This is NOT for XEmacs!

Many of the steps here are sometimes done by the build system of your package (especially compilation), so this is mainly for standalone elisp files you gathered from somewhere else.

When relying on the emacs USE flag, you need to add

        emacs? ( >=app-editors/emacs-23.1:* )
to your DEPEND/RDEPEND line and use the functions provided here to bring the files to the correct locations.

If your package requires a minimum Emacs version, e.g. Emacs 26.1, then the dependency should be on >=app-editors/emacs-26.1:* instead. Because the user can select the Emacs executable with eselect, you should also make sure that the active Emacs version is sufficient. The eclass will automatically ensure this if you assign variable NEED_EMACS with the Emacs version, as in the following example:

        NEED_EMACS=26.1
Please note that this should be done only for packages that are known to fail with lower Emacs versions.

src_compile() usage:
An elisp file is compiled by the elisp-compile() function defined here and simply takes the source files as arguments. The case of interdependent elisp files is also supported, since the current directory is added to the load-path which makes sure that all files are loadable.

        elisp-compile *.el
Function elisp-make-autoload-file() can be used to generate a file with autoload definitions for the lisp functions. It takes the output file name (default: "${PN}-autoloads.el") and a list of directories (default: working directory) as its arguments. Use of this function requires that the elisp source files contain magic ";;;###autoload" comments. See the Emacs Lisp Reference Manual (node "Autoload") for a detailed explanation.

src_install() usage:
The resulting compiled files (.elc) should be put in a subdirectory of /usr/share/emacs/site-lisp/ which is named after the first argument of elisp-install(). The following parameters are the files to be put in that directory. Usually the subdirectory should be ${PN}, you can choose something else, but remember to tell elisp-site-file-install() (see below) the change, as it defaults to ${PN}.

        elisp-install ${PN} *.el *.elc
To let the Emacs support be activated by Emacs on startup, you need to provide a site file (shipped in ${FILESDIR}) which contains the startup code (have a look in the documentation of your software). Normally this would look like this:

        (add-to-list 'load-path "@SITELISP@")
        (add-to-list 'auto-mode-alist '("\.csv\'" . csv-mode))
        (autoload 'csv-mode "csv-mode" "Major mode for csv files." t)
If your Emacs support files are installed in a subdirectory of /usr/share/emacs/site-lisp/ (which is strongly recommended), you need to extend Emacs' load-path as shown in the first non-comment line. The elisp-site-file-install() function of this eclass will replace "@SITELISP@" and "@SITEETC@" by the actual paths.

The next line tells Emacs to load the mode opening a file ending with ".csv" and load functions depending on the context and needed features. Be careful though. Commands as "load-library" or "require" bloat the editor as they are loaded on every startup. When having many Emacs support files, users may be annoyed by the start-up time. Also avoid keybindings as they might interfere with the user's settings. Give a hint in pkg_postinst(), which should be enough. The guiding principle is that emerging your package should not by itself cause a change of standard Emacs behaviour.

The naming scheme for this site-init file matches the shell pattern "[1-8][0-9]*-gentoo*.el", where the two digits at the beginning define the loading order (numbers below 10 or above 89 are reserved for internal use). So if your initialisation depends on another Emacs package, your site file's number must be higher! If there are no such interdependencies then the number should be 50. Otherwise, numbers divisible by 10 are preferred.

Best practice is to define a SITEFILE variable in the global scope of your ebuild (e.g., right after S or RDEPEND):

        SITEFILE="50${PN}-gentoo.el"
Which is then installed by

        elisp-site-file-install "${FILESDIR}/${SITEFILE}"
in src_install(). Any characters after the "-gentoo" part and before the extension will be stripped from the destination file's name. For example, a file "50${PN}-gentoo-${PV}.el" will be installed as "50${PN}-gentoo.el". If your subdirectory is not named ${PN}, give the differing name as second argument.

pkg_setup() usage:
If your ebuild uses the elisp-compile eclass function to compile its elisp files (see above), then you don't need a pkg_setup phase, because elisp-compile and elisp-make-autoload-file do their own sanity checks. On the other hand, if the elisp files are compiled by the package's build system, then there is often no check for the Emacs version. In this case, you can add an explicit check in pkg_setup:

        elisp-check-emacs-version
When having optional Emacs support, you should prepend "use emacs &&" to above call of elisp-check-emacs-version().

pkg_postinst() / pkg_postrm() usage:
After that you need to recreate the start-up file of Emacs after emerging and unmerging by using

        pkg_postinst() {
                elisp-site-regen
        }

        pkg_postrm() {
                elisp-site-regen
        }
Again, with optional Emacs support, you should prepend "use emacs &&" to above calls of elisp-site-regen().

FUNCTIONS
elisp-emacs-version
Output version of currently active Emacs.
Return value: exit status of Emacs

elisp-check-emacs-version [version]
Test if the eselected Emacs version is at least the version of GNU Emacs specified in the NEED_EMACS variable, or die otherwise.
elisp-compile <list of elisp files>
Byte-compile Emacs Lisp files.
This function uses GNU Emacs to byte-compile all ".el" specified by its arguments. The resulting byte-code (".elc") files are placed in the same directory as their corresponding source file.

The current directory is added to the load-path. This will ensure that interdependent Emacs Lisp files are visible between themselves, in case they require or load one another.

elisp-make-autoload-file [output file] [list of directories]
Generate a file with autoload definitions for the lisp functions.
elisp-install <subdirectory> <list of files>
Install files in SITELISP directory.
elisp-modules-install <subdirectory> <list of files>
Install dynamic modules in EMACSMODULES directory.
elisp-site-file-install <site-init file> [subdirectory]
Install Emacs site-init file in SITELISP directory. Automatically inserts a standard comment header with the name of the package (unless it is already present). Tokens @SITELISP@, @SITEETC@, and
elisp-site-regen
Regenerate the site-gentoo.el file, based on packages' site initialisation files in the /usr/share/emacs/site-lisp/site-gentoo.d/ directory.
ECLASS VARIABLES
SITELISP = /usr/share/emacs/site-lisp
Directory where packages install Emacs Lisp files.
SITEETC = /usr/share/emacs/etc
Directory where packages install miscellaneous (not Lisp) files.
EMACSMODULES = /usr/@libdir@/emacs/modules
Directory where packages install dynamically loaded modules. May contain a @libdir@ token which will be replaced by $(get_libdir).
EMACS = ${EPREFIX}/usr/bin/emacs
Path of Emacs executable.
EMACSFLAGS = "-batch -q --no-site-file"
Flags for executing Emacs in batch mode. These work for Emacs versions 18-24, so don't change them.
BYTECOMPFLAGS = "-L ."
Emacs flags used for byte-compilation in elisp-compile().
NEED_EMACS ?= 23.1
The minimum Emacs version required for the package.
AUTHORS
Matthew Kennedy <mkennedy@gentoo.org>
Jeremy Maitin-Shepard <jbms@attbi.com>
Mamoru Komachi <usata@gentoo.org>
Christian Faulhammer <fauli@gentoo.org>
Ulrich Müller <ulm@gentoo.org>
MAINTAINERS
Gentoo GNU Emacs project <gnu-emacs@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
elisp-common.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/elisp-common.eclass
Index
NAME
DESCRIPTION
src_compile() usage:
src_install() usage:
pkg_setup() usage:
pkg_postinst() / pkg_postrm() usage:
FUNCTIONS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:01 GMT, October 11, 2020