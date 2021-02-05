GIT-R3.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
git-r3.eclass - Eclass for fetching and unpacking git repositories.
DESCRIPTION
Third generation eclass for easing maintenance of live ebuilds using git as remote repository.
SUPPORTED EAPIS
4 5 6 7
FUNCTIONS
git-r3_fetch [<repo-uri> [<remote-ref> [<local-id> [<commit-date>]]]]
Fetch new commits to the local clone of repository.
<repo-uri> specifies the repository URIs to fetch from, as a space- -separated list. The first URI will be used as repository group identifier and therefore must be used consistently. When not specified, defaults to ${EGIT_REPO_URI}.

<remote-ref> specifies the remote ref or commit id to fetch. It is preferred to use 'refs/heads/<branch-name>' for branches and 'refs/tags/<tag-name>' for tags. Other options are 'HEAD' for upstream default branch and hexadecimal commit SHA1. Defaults to the first of EGIT_COMMIT, EGIT_BRANCH or literal 'HEAD' that is set to a non-null value.

<local-id> specifies the local branch identifier that will be used to locally store the fetch result. It should be unique to multiple fetches within the repository that can be performed at the same time (including parallel merges). It defaults to ${CATEGORY}/${PN}/${SLOT%/*}. This default should be fine unless you are fetching multiple trees from the same repository in the same ebuild.

<commit-date> requests attempting to use repository state as of specific date. For more details, see EGIT_COMMIT_DATE.

The fetch operation will affect the EGIT_STORE only. It will not touch the working copy, nor export any environment variables. If the repository contains submodules, they will be fetched recursively.

git-r3_checkout [<repo-uri> [<checkout-path> [<local-id> [<checkout-paths>...]]]]
Check the previously fetched tree to the working copy.
<repo-uri> specifies the repository URIs, as a space-separated list. The first URI will be used as repository group identifier and therefore must be used consistently with git-r3_fetch. The remaining URIs are not used and therefore may be omitted. When not specified, defaults to ${EGIT_REPO_URI}.

<checkout-path> specifies the path to place the checkout. It defaults to ${EGIT_CHECKOUT_DIR} if set, otherwise to ${WORKDIR}/${P}.

<local-id> needs to specify the local identifier that was used for respective git-r3_fetch.

If <checkout-paths> are specified, then the specified paths are passed to 'git checkout' to effect a partial checkout. Please note that such checkout will not cause the repository to switch branches, and submodules will be skipped at the moment. The submodules matching those paths might be checked out in a future version of the eclass.

The checkout operation will write to the working copy, and export the repository state into the environment. If the repository contains submodules, they will be checked out recursively.

git-r3_peek_remote_ref [<repo-uri> [<remote-ref>]]
Peek the reference in the remote repository and print the matching (newest) commit SHA1.
<repo-uri> specifies the repository URIs to fetch from, as a space- -separated list. When not specified, defaults to ${EGIT_REPO_URI}.

<remote-ref> specifies the remote ref to peek. It is preferred to use for tags. Alternatively, 'HEAD' may be used for upstream default branch. Defaults to the first of EGIT_COMMIT, EGIT_BRANCH or literal

The operation will be done purely on the remote, without using local storage. If commit SHA1 is provided as <remote-ref>, the function will fail due to limitations of git protocol.

On success, the function returns 0 and writes hexadecimal commit SHA1 to stdout. On failure, the function returns 1.

ECLASS VARIABLES
EGIT_CLONE_TYPE ?= single
Type of clone that should be used against the remote repository. This can be either of: 'mirror', 'single', 'shallow'.
This is intended to be set by user in make.conf. Ebuilds are supposed to set EGIT_MIN_CLONE_TYPE if necessary instead.

The 'mirror' type clones all remote branches and tags with complete history and all notes. EGIT_COMMIT can specify any commit hash. Upstream-removed branches and tags are purged from the local clone while fetching. This mode is suitable for cloning the local copy for development or hosting a local git mirror. However, clones of repositories with large diverged branches may quickly grow large.

The 'single+tags' type clones the requested branch and all tags in the repository. All notes are fetched as well. EGIT_COMMIT can safely specify hashes throughout the current branch and all tags. No purging of old references is done (if you often switch branches, you may need to remove stale branches yourself). This mode is intended mostly for use with broken git servers such as Google Code that fail to fetch tags along with the branch in 'single' mode.

The 'single' type clones only the requested branch or tag. Tags referencing commits throughout the branch history are fetched as well, and all notes. EGIT_COMMIT can safely specify only hashes in the current branch. No purging of old references is done (if you often switch branches, you may need to remove stale branches yourself). This mode is suitable for general use.

The 'shallow' type clones only the newest commit on requested branch or tag. EGIT_COMMIT can only specify tags, and since the history is unavailable calls like 'git describe' will not reference prior tags. No purging of old references is done. This mode is intended mostly for embedded systems with limited disk space.

EGIT_MIN_CLONE_TYPE ?= shallow
as EGIT_CLONE_TYPE. When user sets a type that's 'lower' (that is, later on the list) than EGIT_MIN_CLONE_TYPE, the eclass uses EGIT_MIN_CLONE_TYPE instead.
This variable is intended to be used by ebuilds only. Users are supposed to set EGIT_CLONE_TYPE instead.

A common case is to use 'single' whenever the build system requires access to full branch history, or 'single+tags' when Google Code or a similar remote is used that does not support shallow clones and fetching tags along with commits. Please use sparingly, and to fix fatal errors rather than 'non-pretty versions'.

EGIT3_STORE_DIR (USER VARIABLE)
Storage directory for git sources.
This is intended to be set by user in make.conf. Ebuilds must not set it.

EGIT3_STORE_DIR=${DISTDIR}/git3-src

EGIT_MIRROR_URI
to fetch from the local mirror instead of using the remote repository.
The mirror needs to follow EGIT3_STORE_DIR structure. The directory created by eclass can be used for that purpose.

Example:

EGIT_MIRROR_URI="git://mirror.lan/"
EGIT_REPO_URI (REQUIRED)
URIs to the repository, e.g. https://foo. If multiple URIs are provided, the eclass will consider the remaining URIs as fallbacks to try if the first URI does not work. For supported URI syntaxes, read the manpage for git-clone(1).
URIs should be using https:// whenever possible. http:// and git:// URIs are completely unsecured and their use (even if only as a fallback) renders the ebuild completely vulnerable to MITM attacks.

Can be a whitespace-separated list or an array.

Example:

EGIT_REPO_URI="https://a/b.git https://c/d.git"
EVCS_OFFLINE
If non-empty, this variable prevents any online operations.
EVCS_UMASK
Set this variable to a custom umask. This is intended to be set by users. By setting this to something like 002, it can make life easier for people who do development as non-root (but are in the portage group), and then switch over to building with FEATURES=userpriv. Or vice-versa. Shouldn't be a security issue here as anyone who has portage group write access already can screw the system over in more creative ways.
EGIT_BRANCH
The branch name to check out. If unset, the upstream default (HEAD) will be used.
EGIT_COMMIT
The tag name or commit identifier to check out. If unset, newest commit from the branch will be used. Note that if set to a commit not on HEAD branch, EGIT_BRANCH needs to be set to a branch on which the commit is available.
EGIT_COMMIT_DATE
Attempt to check out the repository state for the specified timestamp. The date should be in format understood by 'git rev-list'. The commits on EGIT_BRANCH will be considered.
The eclass will select the last commit with commit date preceding the specified date. When merge commits are found, only first parents will be considered in order to avoid switching into external branches (assuming that merges are done correctly). In other words, each merge will be considered alike a single commit with date corresponding to the merge commit date.

EGIT_CHECKOUT_DIR
The directory to check the git sources out to.
EGIT_CHECKOUT_DIR=${WORKDIR}/${P}

EGIT_SUBMODULES
An array of inclusive and exclusive wildcards on submodule names, stating which submodules are fetched and checked out. Exclusions start with '-', and exclude previously matched submodules.
If unset, all submodules are enabled. Empty list disables all submodules. In order to use an exclude-only list, start the array with '*'.

Remember that wildcards need to be quoted in order to prevent filename expansion.

Examples:

# Disable all submodules
EGIT_SUBMODULES=()

# Include only foo and bar
EGIT_SUBMODULES=( foo bar )

# Use all submodules except for test-* but include test-lib
EGIT_SUBMODULES=( '*' '-test-*' test-lib )
MAINTAINERS
Michał Górny <mgorny@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
git-r3.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/git-r3.eclass
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
Time: 21:27:05 GMT, October 10, 2020
