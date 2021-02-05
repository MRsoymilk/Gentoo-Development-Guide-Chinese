ESTACK.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
estack.eclass - stack-like value storage support
DESCRIPTION
Support for storing values on stack-like variables.
FUNCTIONS
estack_push <stack> [items to push]
Push any number of items onto the specified stack. Pick a name that is a valid variable (i.e. stick to alphanumerics), and push as many items as you like onto the stack at once.
The following code snippet will echo 5, then 4, then 3, then ...

        estack_push mystack 1 2 3 4 5
        while estack_pop mystack i ; do
                echo "${i}"
        done
estack_pop <stack> [variable]
Pop a single item off the specified stack. If a variable is specified, the popped item is stored there. If no more items are available, return 1, else return 0. See estack_push for more info.
evar_push <variable to save> [more vars to save]
This let's you temporarily modify a variable and then restore it (including set vs unset semantics). Arrays are not supported at this time.
This is meant for variables where using `local` does not work (such as exported variables, or only temporarily changing things in a func).

For example:

        evar_push LC_ALL
        export LC_ALL=C
        ... do some stuff that needs LC_ALL=C set ...
        evar_pop

        # You can also save/restore more than one var at a time
        evar_push BUTTERFLY IN THE SKY
        ... do stuff with the vars ...
        evar_pop     # This restores just one var, SKY
        ... do more stuff ...
        evar_pop 3   # This pops the remaining 3 vars
evar_push_set <variable to save> [new value to store]
This is a handy shortcut to save and temporarily set a variable. If a value is not specified, the var will be unset.
evar_pop [number of vars to restore]
Restore the variables to the state saved with the corresponding evar_push call. See that function for more details.
eshopts_push [options to `set` or `shopt`]
Often times code will want to enable a shell option to change code behavior. Since changing shell options can easily break other pieces of code (which assume the default state), eshopts_push is used to (1) push the current shell options onto a stack and (2) pass the specified arguments to set.
If the first argument is '-s' or '-u', we assume you want to call `shopt` rather than `set` as there are some options only available via that.

A common example is to disable shell globbing so that special meaning/care may be used with variables/arguments to custom functions. That would be:

        eshopts_push -o noglob
        for x in ${foo} ; do
                if ...some check... ; then
                        eshopts_pop
                        return 0
                fi
        done
        eshopts_pop
eshopts_pop
Restore the shell options to the state saved with the corresponding eshopts_push call. See that function for more details.
eumask_push <new umask>
Set the umask to the new value specified while saving the previous value onto a stack. Useful for temporarily changing the umask.
eumask_pop
Restore the previous umask state.
MAINTAINERS
base-system@gentoo.org
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
estack.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/estack.eclass
Index
NAME
DESCRIPTION
FUNCTIONS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020