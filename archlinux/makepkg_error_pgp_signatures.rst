==========================================
Cannot install signed package with Makepkg
==========================================

While installing ``cower`` from **AUR**, I had this error::

    $ makepkg -sri
    ==> Making package: cower 14-2 (Mon Mar  7 11:38:29 CET 2016)
    ==> Checking runtime dependencies...
    ==> Checking buildtime dependencies...
    ==> Retrieving sources...
      -> Found cower-14.tar.gz
      -> Found cower-14.tar.gz.sig
    ==> Validating source files with md5sums...
        cower-14.tar.gz ... Passed
        cower-14.tar.gz.sig ... Skipped
    ==> Verifying source file signatures with gpg...
        cower-14.tar.gz ... FAILED (unknown public key 1EB2638FF56C0C53)
    ==> ERROR: One or more PGP signatures could not be verified!
    Makepkg error: One or more PGP signatures could not be verified!

Firstly, I looked at the **PKGBUILD** file of the package, to identify the line mentioning the PGP key::

    validpgpkeys=('487EACC08557AD082088DABA1EB2638FF56C0C53')

Then, I downloaded and signed this key::

  $ gpg --keyserver pgp.mit.edu --recv-keys 487EACC08557AD082088DABA1EB2638FF56C0C53
  # pacman-key --lsign 487EACC08557AD082088DABA1EB2638FF56C0C53

Please note that signing the key must be done as **root**.


Ressources
----------

- http://allanmcrae.com/2015/01/two-pgp-keyrings-for-package-management-in-arch-linux/
