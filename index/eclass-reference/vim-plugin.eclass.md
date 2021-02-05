VIM-PLUGIN.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
vim-plugin.eclass - used for installing vim plugins
DESCRIPTION
This eclass simplifies installation of app-vim plugins into /usr/share/vim/vimfiles. This is a version-independent directory which is read automatically by vim. The only exception is documentation, for which we make a special case via vim-doc.eclass.
FUNCTIONS
vim-plugin_src_install
Overrides the default src_install phase. In order, this function: * fixes file permission across all files in ${S}. * installs help and documentation files. * installs all files in "${ED}"/usr/share/vim/vimfiles.
vim-plugin_pkg_postinst
Overrides the pkg_postinst phase for this eclass. The following functions are called: * update_vim_helptags * update_vim_afterscripts * display_vim_plugin_help
vim-plugin_pkg_postrm
Overrides the pkg_postrm phase for this eclass. This function calls the update_vim_helptags and update_vim_afterscripts functions and eventually removes a bunch of empty directories.
update_vim_afterscripts
Creates scripts in /usr/share/vim/vimfiles/after/* comprised of the snippets in /usr/share/vim/vimfiles/after/*/*.d
display_vim_plugin_help
Displays a message with the plugin's help file if one is available. Uses the VIM_PLUGIN_HELPFILES env var. If multiple help files are available, they should be separated by spaces. If no help files are available, but the env var VIM_PLUGIN_HELPTEXT is set, that is displayed instead. Finally, if we have nothing else, this functions displays a link to VIM_PLUGIN_HELPURI. An extra message regarding enabling filetype plugins is displayed if VIM_PLUGIN_MESSAGES includes the word "filetype".
MAINTAINERS
vim@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
vim-plugin.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/vim-plugin.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020