SSL-CERT.ECLASS
Section: eclass-manpages (5)
Updated: Sep 2020
Index Return to Main Contents
NAME
ssl-cert.eclass - Eclass for SSL certificates
DESCRIPTION
This eclass implements a standard installation procedure for installing self-signed SSL certificates.
SUPPORTED EAPIS
1 2 3 4 5 6 7
EXAMPLE
"install_cert /foo/bar" installs ${ROOT}/foo/bar.{key,csr,crt,pem}
FUNCTIONS
gen_cnf
Initializes variables and generates the needed OpenSSL configuration file and a CA serial file
Access: private

get_base [if_ca]
Simple function to determine whether we're creating a CA (which should only be done once) or final part
Access: private

Return value: <base path>

gen_key <base path>
Generates an RSA key
Access: private

gen_csr <base path>
Generates a certificate signing request using the key made by gen_key()
Access: private

gen_crt <base path>
Generates either a self-signed CA certificate using the csr and key made by gen_csr() and gen_key() or a signed server certificate using the CA cert previously created by gen_crt()
Access: private

gen_pem <base path>
Generates a PEM file by concatinating the key and cert file created by gen_key() and gen_cert()
Access: private

install_cert <certificates>
Uses all the private functions above to generate and install the requested certificates. <certificates> are full pathnames relative to ROOT, without extension.
Example: "install_cert /foo/bar" installs ${ROOT}/foo/bar.{key,csr,crt,pem}

Access: public

ECLASS VARIABLES
SSL_CERT_MANDATORY ?= 0
Set to non zero if ssl-cert is mandatory for ebuild.
SSL_CERT_USE ?= ssl
Use flag to append dependency to.
SSL_DEPS_SKIP ?= 0
Set to non zero to skip adding to DEPEND and IUSE.
AUTHORS
Max Kalika <max@gentoo.org>
REPORTING BUGS
Please report bugs via https://bugs.gentoo.org/
FILES
ssl-cert.eclass
SEE ALSO
ebuild(5)
https://gitweb.gentoo.org/repo/gentoo.git/log/eclass/ssl-cert.eclass
Index
NAME
DESCRIPTION
SUPPORTED EAPIS
EXAMPLE
FUNCTIONS
ECLASS VARIABLES
AUTHORS
REPORTING BUGS
FILES
SEE ALSO
This document was created by man2html, using the manual pages.
Time: 00:27:04 GMT, October 11, 2020