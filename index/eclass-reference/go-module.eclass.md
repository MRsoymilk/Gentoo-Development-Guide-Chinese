GO-MODULE.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
go-module.eclass - basic eclass for building software written as go modules
DESCRIPTION
This eclass provides basic settings and functions needed by all software written in the go programming language that uses modules.
If the software you are packaging has a file named go.mod in its top level directory, it uses modules and your ebuild should inherit this eclass. If it does not, your ebuild should use the golang-* eclasses.

If, besides go.mod, your software has a directory named vendor in its top level directory, the only thing you need to do is inherit the eclass. If there is no vendor directory, you need to also populate EGO_SUM and call go-module_set_globals as discussed below.

Since Go programs are statically linked, it is important that your ebuild's LICENSE= setting includes the licenses of all statically linked dependencies. So please make sure it is accurate. You can use a utility like dev-util/golicense (network connectivity is required) to extract this information from the compiled binary.

SUPPORTED EAPIS
7
EXAMPLE

inherit go-module

EGO_SUM=(
  "github.com/aybabtme/rgbterm v0.0.0-20170906152045-cc83f3b3ce59/go.mod"
  "github.com/aybabtme/rgbterm v0.0.0-20170906152045-cc83f3b3ce59"
)

go-module_set_globals

SRC_URI="https://github.com/example/${PN}/archive/v${PV}.tar.gz -> ${P}.tar.gz
           ${EGO_SUM_SRC_URI}"

FUNCTIONS
go-module_set_globals
Convert the information in EGO_SUM for other usage in the ebuild. - Populates EGO_SUM_SRC_URI that can be added to SRC_URI - Exports _GOMODULE_GOSUM_REVERSE_MAP which provides reverse mapping from
  distfile back to the relative part of SRC_URI, as needed for
  GOPROXY=file:///...
go-module_src_unpack
If EGO_SUM is set, unpack the base tarball(s) and set up the
  local go proxy. - Otherwise, if EGO_VENDOR is set, bail out. - Otherwise do a normal unpack.
_go-module_src_unpack_gosum
Populate a GOPROXY directory hierarchy with distfiles from EGO_SUM and unpack the base distfiles.
Exports GOPROXY environment variable so that Go calls will source the directory correctly.

_go-module_gosum_synthesize_files
Given a path & version, populate all Goproxy metadata files which aren't needed to be downloaded directly. - .../@v/${version}.info - .../@v/list
_go-module_src_unpack_verify_gosum
Validate the Go modules declared by EGO_SUM are sufficient to cover building the package, without actually building it yet.
go-module_live_vendor
This function is used in live ebuilds to vendor the dependencies when upstream doesn't vendor them.
go-module_pkg_postinst
Display a warning about security updates for Go programs.
_go-module_gomod_encode
Encode the name(path) of a Golang module in the format expected by Goproxy.
Upper letters are replaced by their lowercase version with a '!' prefix.

ECLASS VARIABLES
EGO_SUM
This is an array based on the go.sum content from inside the target package. Each array entry must be quoted and contain information from a single line from go.sum.
The format of go.sum is described upstream here: https://tip.golang.org/cmd/go/#hdr-Module_authentication_using_go_sum

For inclusion in EGO_SUM, the h1: value and other future extensions SHOULD be omitted at this time. The EGO_SUM parser will accept them for ease of ebuild creation.

h1:<hash> is the Hash1 structure used by upstream Go The Hash1 is MORE stable than Gentoo distfile hashing, and upstream warns that it's conceptually possible for the Hash1 value to remain stable while the upstream zipfiles change. Here are examples that do NOT change the h1: hash, but do change a regular checksum over all bytes of the file: - Differing mtimes within zipfile - Differing filename ordering with the zipfile - Differing zipfile compression parameters - Differing zipfile extra fields

For Gentoo usage, the authors of this eclass feel that the h1: hash should NOT be included in the EGO_SUM at this time in order to reduce size of the ebuilds. This position will be reconsidered in future when a Go module distfile collision comes to light, where the Hash1 value of two distfiles is the same, but checksums over the file as a byte stream consider the files to be different.

This decision does NOT weaken Go module security, as Go will verify the go.sum copy of the Hash1 values during building of the package.

_GOMODULE_GOPROXY_BASEURI
Golang module proxy service to fetch module files from. Note that the module proxy generally verifies modules via the Hash1 code.
Users in China may find some mirrors in the default list blocked, and should explicitly set an entry in /etc/portage/mirrors for goproxy to https://goproxy.cn/ or another mirror that is not blocked in China. See https://arslan.io/2019/08/02/why-you-should-use-a-go-module-proxy/ for further details

This variable is NOT intended for user-level configuration of mirrors, but rather to cover go modules that might exist only on specific Goproxy servers for non-technical reasons.

This variable should NOT be present in user-level configuration e.g. /etc/portage/make.conf, as it will violate metadata immutability!

I am considering removing this and just hard coding mirror://goproxy below, so please do not rely on it.

_GOMODULE_GOSUM_REVERSE_MAP
Mapping back from Gentoo distfile name to upstream distfile path. Associative array to avoid O(N*M) performance when populating the GOPROXY directory structure.
AUTHORS
William Hubbs <williamh@gentoo.org>
Robin H. Johnson <robbat2@gentoo.org>
MAINTAINERS
William Hubbs <williamh@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
go-module.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/go-module.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
EXAMPLE
FUNCTIONS
ECLASS VARIABLES
AUTHORS
MAINTAINERS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:03 GMT, October 11, 2020
