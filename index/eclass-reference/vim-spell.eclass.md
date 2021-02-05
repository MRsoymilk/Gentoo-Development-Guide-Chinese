VIM-SPELL.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
vim-spell.eclass - Eclass for managing Vim spell files.
DESCRIPTION
How to make a vim spell file package using prebuilt spell lists from upstream (${CODE} is the language's two letter code):
* Get the ${CODE}.*.spl, ${CODE}.*.sug (if your language has them) and
  README_${CODE}.txt files. Currently they're at
  ftp://ftp.vim.org/pub/vim/unstable/runtime/spell/ (except for English,
  which should be taken from CVS instead).

* Stick them in vim-spell-${CODE}-$(date --iso | tr -d - ).tar.bz2 . Make sure
  that they're in the appropriately named subdirectory to avoid having to mess
  with S=.

* Upload the tarball to the Gentoo mirrors.

* Add your spell file to package.mask next to the other vim things. Vim
  Project members will handle unmasking your spell packages when vim comes out
  of package.mask.

* Create the app-vim/vim-spell-${CODE} package. You should base your ebuild
  upon app-vim/vim-spell-en. You will need to change VIM_SPELL_LANGUAGE,
  KEYWORDS and LICENSE. Check the license carefully! The README will tell
  you what it is.

* Don't forget metadata.xml. You should list the Vim project and yourself
  as maintainers. There is no need to join the Vim project just for spell
  files. Here's an example of a metadata.xml file:


    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE pkgmetadata SYSTEM "https://www.gentoo.org/dtd/metadata.dtd">
    <pkgmetadata>
       <maintainer type="person">
               <email>your@email.tld</email>
               <name>Your Name</name>
       </maintainer>
       <maintainer type="project">
               <email>vim@gentoo.org</email>
               <name>Vim Maintainers</name>
       </maintainer>


        <longdescription lang="en">

                Vim spell files for French (fr). Supported character sets are

                UTF-8 and latin1.

        </longdescription>

    </pkgmetadata>

* Send an email to vim@gentoo.org to let us know.

Don't forget to update your package as necessary.

If there isn't an upstream-provided pregenerated spell file for your language yet, read :help spell.txt from inside vim for instructions on how to create spell files. It's best to let upstream know if you've generated spell files for another language rather than keeping them Gentoo-specific.

FUNCTIONS
vim-spell_src_install
This function installs Vim spell files.
vim-spell_pkg_postinst
This function displays installed Vim spell files.
ECLASS VARIABLES
VIM_SPELL_LANGUAGE ?= "English"
This variable defines the language for the spell package being installed. The default value is "English".
DESCRIPTION ?= "vim spell files: ${VIM_SPELL_LANGUAGE} (${VIM_SPELL_LOCALE})"
This variable defines the DESCRIPTION for Vim spell ebuilds.
HOMEPAGE ?= "https://www.vim.org"
This variable defines the HOMEPAGE for Vim spell ebuilds.
AUTHORS
Ciaran McCreesh <ciaranm@gentoo.org>
MAINTAINERS
Vim Maintainers <vim@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
vim-spell.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/vim-spell.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:01 GMT, October 11, 2020