=================================================
Pacman error: key could not be looked up remotely
=================================================

A long time after not upgraded my laptop, I was facing a weird error when running ``pacman -Syu``::

    [root@naughty-box]# pacman -Syu
    :: Proceed with installation? [Y/n] Y
    (299/299) checking keys in keyring                 [################] 100%
    downloading required keys...
    error: key "A6234074498E9CEE" could not be looked up remotely
    error: required key missing from keyring
    error: failed to commit transaction (unexpected error)
    Errors occurred, no packages were upgraded.

So, I tried::

    [root@naughty-box] mv /etc/pacman.d/gnupg /etc/pacman.d/gnupg.bck
    [root@naughty-box] mv /root/.gnupg/ /root/.gnupg.bck

Then::

    [root@naughty-box] pacman-key --init
    gpg: /etc/pacman.d/gnupg/trustdb.gpg: trustdb created
    gpg: no ultimately trusted keys found
    gpg: starting migration from earlier GnuPG versions
    gpg: porting secret keys from '/etc/pacman.d/gnupg/secring.gpg' to gpg-agent
    gpg: migration succeeded
    gpg: Generating pacman keyring master key...
    gpg: key CD0513C3 marked as ultimately trusted
    gpg: directory '/etc/pacman.d/gnupg/openpgp-revocs.d' created
    gpg: Done
    ==> Updating trust database...
    gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
    gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u

Then after::

    [root@naughty-box] pacman-key --populate archlinux
    ==> Appending keys from archlinux.gpg...
    gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
    gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
    ==> Locally signing trusted keys in keyring...
      -> Locally signing key 0E8B644079F599DFC1DDC3973348882F6AC6A4C2...
      -> Locally signing key 684148BB25B49E986A4944C55184252D824B18E8...
      -> Locally signing key 44D4A033AC140143927397D47EFD567D4C7EA887...
      -> Locally signing key 27FFC4769E19F096D41D9265A04F9397CDFD6BB0...
      -> Locally signing key AB19265E5D7D20687D303246BA1DFB64FFF979E7...
    ==> Importing owner trust values...
    gpg: inserting ownertrust of 4
    gpg: setting ownertrust to 4
    gpg: setting ownertrust to 4
    gpg: setting ownertrust to 4
    gpg: setting ownertrust to 4
    ==> Disabling revoked keys in keyring...
      -> Disabling key F5A361A3A13554B85E57DDDAAF7EF7873CFD4BB6...
      -> Disabling key 7FA647CD89891DEDC060287BB9113D1ED21E1A55...
      -> Disabling key D4DE5ABDE2A7287644EAC7E36D1A9E70E19DAA50...
      -> Disabling key BC1FBE4D2826A0B51E47ED62E2539214C6C11350...
      -> Disabling key 4A8B17E20B88ACA61860009B5CED81B7C2E5C0D2...
      -> Disabling key 63F395DE2D6398BBE458F281F2DBB4931985A992...
      -> Disabling key 0B20CA1931F5DA3A70D0F8D2EA6836E1AB441196...
      -> Disabling key 8F76BEEA0289F9E1D3E229C05F946DED983D4366...
      -> Disabling key 66BD74A036D522F51DD70A3C7F2A16726521E06D...
      -> Disabling key 81D7F8241DB38BC759C80FCE3A726C6170E80477...
      -> Disabling key E7210A59715F6940CF9A4E36A001876699AD6E84...
    ==> Updating trust database...
    gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
    gpg: depth: 0  valid:   1  signed:   5  trust: 0-, 0q, 0n, 0m, 0f, 1u
    gpg: depth: 1  valid:   5  signed:  60  trust: 0-, 0q, 0n, 5m, 0f, 0u
    gpg: depth: 2  valid:  59  signed:   4  trust: 59-, 0q, 0n, 0m, 0f, 0u
    gpg: next trustdb check due at 2016-01-22

And this is not finished!::

    [root@naughty-box]# pacman-key --refresh-keys
    gpg: refreshing 77 keys from hkp://pool.sks-keyservers.net
    gpg: connecting dirmngr at '/root/.gnupg/S.dirmngr' failed: IPC connect call failed
    gpg: keyserver refresh failed: No dirmngr
    ==> ERROR: A specified local key could not be updated from a keyserver.

We are near the end but a new error crossed my path. To resolve this latter (please note that this is one of the few useful commands here)::

    [root@naughty-box] dirmngr < /dev/null
    dirmngr[14457.0]: error opening '/root/.gnupg/dirmngr_ldapservers.conf': No such file or directory
    dirmngr[14457.0]: permanently loaded certificates: 0
    dirmngr[14457.0]:     runtime cached certificates: 0
    dirmngr[14457.0]: failed to open cache dir file '/root/.gnupg/dirmngr-cache.d/DIR.txt': No such file or directory
    dirmngr[14457.0]: creating directory '/root/.gnupg'
    dirmngr[14457.0]: creating directory '/root/.gnupg/dirmngr-cache.d'
    dirmngr[14457.0]: new cache dir file '/root/.gnupg/dirmngr-cache.d/DIR.txt' created
    # Home: ~/.gnupg
    # Config: [none]
    OK Dirmngr 2.1.0 at your service

Then::

    [root@naughty-box] pacman-key --refresh-keys
    gpg: refreshing 77 keys from hkp://pool.sks-keyservers.net
    gpg: key F15447D5: "Federico Cinelli <cinelli@aur.archlinux.org>" 11 new signatures
    gpg: key B47A0DAB: "Connor Behan <connor.behan@gmail.com>" 1 new signature
    gpg: key 487328A9: "Bartlomiej Piotrowski <b@bpiotrowski.pl>" 12 new signatures
    gpg: key 4CE1C13E: "Florian Pritz <bluewind@xinu.at>" 41 new signatures
    gpg: key 00F0D0F0: "Gaetan Bisson <gaetan@fenua.org>" 116 new signatures
    gpg: key 0901C163: "Balló György <ballogyor@gmail.com>" 1 new signature
    gpg: key 5CF9C8D4: "Alexander Rødseth <rodseth@gmail.com>" 1 new signature
    gpg: key 31361F01: "Evgeniy Alekseev <arcanis@archlinux.org>" 1 new user ID
    gpg: key 31361F01: "Evgeniy Alekseev <arcanis@archlinux.org>" 6 new signatures
    gpg: key EC133BAD: "Angel Velásquez <angvp@archlinux.org>" 1 new signature
    gpg: key 0F2A092B: "Andreas Radke <andyrtr@archlinux.org>" 5 new user IDs
    gpg: key 0F2A092B: "Andreas Radke <andyrtr@archlinux.org>" 7 new signatures
    gpg: key D30DB0AD: "Andrea Scarpino <me@andreascarpino.it>" 318 new signatures
    gpg: key 753E0F1F: "Anatol Pomozov <anatol.pomozov@gmail.com>" 5 new signatures
    gpg: key 98BC6FF5: "Maxime Gauduin <alucryd@archlinux.org>" 1 new user ID
    gpg: key 98BC6FF5: "Maxime Gauduin <alucryd@archlinux.org>" 2 new signatures
    gpg: key EAE999BD: "Allan McRae <me@allanmcrae.com>" 6 new signatures
    gpg: key 31496106: "Andrzej Giniewicz (giniu) <gginiu@gmail.com>" 1 new signature
    gpg: key 824B18E8: "Thomas Bächler (Arch Linux Master Key) <thomas@master-key.archlinux.org>" 3 new signatures
    gpg: key 6AC6A4C2: "Pierre Schmitz (Arch Linux Master Key) <pierre@master-key.archlinux.org>" 3 new signatures
    gpg: key 4C7EA887: "Ionut Biru (Arch Linux Master Key) <ionut@master-key.archlinux.org>" 2 new signatures
    gpg: key CDFD6BB0: "Dan McGee (Arch Linux Master Key) <dan@master-key.archlinux.org>" 4 new signatures
    gpg: key FFF979E7: "Allan McRae (Arch Linux Master Key) <allan@master-key.archlinux.org>" 7 new signatures
    gpg: key 589874AB: "Jürgen Hötzel <juergen@hoetzel.info>" 2 new signatures
    gpg: key F40D2072: "Jonathan Steel <mail@jsteel.org>" 5 new signatures
    gpg: key 013C2580: "Jaroslav Lichtblau (trusted user) <dragonlord@aur.archlinux.org>" 1 new signature
    gpg: key 3B94FA10: "Jan de Groot <jan@linux-specialist.nl>" 2 new user IDs
    gpg: key 3B94FA10: "Jan de Groot <jan@linux-specialist.nl>" 3 new signatures
    gpg: key 7C50773E: "Jelle van der Waa <jelle@vdwaa.nl>" 1 new user ID
    gpg: key 7C50773E: "Jelle van der Waa <jelle@vdwaa.nl>" 5 new signatures
    gpg: key 796CA067: "Ike Devolder <ike.devolder@gmail.com>" 2 new signatures
    gpg: key 615137BC: "Ionut Biru <ibiru@archlinux.org>" 6 new signatures
    gpg: key 4FA415FA: "Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>" 15 new signatures
    gpg: key 215B37AD: "Guillaume ALAUX <guillaume@archlinux.org>" 196 new signatures
    gpg: key F04569AE: "Giovanni Scafora <giovanni@archlinux.org>" 13 new signatures
    gpg: key 30D7CB92: "Felix Yan <felixonmars@gmail.com>" 76 new signatures
    gpg: key A9999C34: "Evangelos Foutras <evangelos@foutrelis.com>" 11 new signatures
    gpg: key 0864983E: "Martin Wimpress (http://www.flexion.org) <martin@flexion.org>" 10 new signatures
    gpg: key 2FA915EC: "Alexandre Filgueira <alexfilgueira@cinnarch.com>" 1 new signature
    gpg: key 9205AC90: "Eric Belanger <eric@archlinux.org>" 1 new signature
    gpg: key 4F010D48: "Daniel Wallace <danielwallace@gtmanfred.com>" 7 new signatures
    gpg: key F56C0C53: "Dave Reisner <d@falconindy.com>" 4 new signatures
    gpg: key 5BD5C938: "Gerardo Exequiel Pozzi <vmlinuz386@gmail.com>" 7 new signatures
    gpg: key BA06C6A9: "Dicebot <public@dicebot.lv>" 1 new signature
    gpg: key B4440678: "Daniel Isenmann <daniel@archlinux.org>" 1 new signature
    gpg: key F53A76ED: "Dan McGee <dpmcgee@gmail.com>" 14 new signatures
    gpg: key 3C4F88BC: "Timothy Redaelli <timothy.redaelli@gmail.com>" 5 new signatures
    gpg: key 7EDF681F: "Tobias Powalowski <tobias.powalowski@googlemail.com>" 7 new signatures
    gpg: key 06361833: "Tom Gundersen <teg@jklm.no>" 5 new signatures
    gpg: key 8E4B1A25: "Thomas Bächler <thomas@archlinux.org>" 6 new signatures
    gpg: key 9AF5F22A: "Daniel Micay <danielmicay@gmail.com>" 1 new signature
    gpg: key 0C84C0A5: "Thomas Dziedzic <gostrc@gmail.com>" 1 new signature
    gpg: key E62EB915: "Sven-Hendrik Haase <sh@lutzhaase.com>" 1 new signature
    gpg: key F1D357C1: "Lukas Jirkovsky <l.jirkovsky@gmail.com>" 1 new signature
    gpg: key EA433FC7: "Sergej Pupykin <arch@sergej.pp.ru>" 4 new signatures
    gpg: key F27FB7DA: "speps <speps@aur.archlinux.org>" 5 new signatures
    gpg: key 2072D77A: "Sébastien Luttringer <seblu@seblu.net>" 4 new user IDs
    gpg: key 2072D77A: "Sébastien Luttringer <seblu@seblu.net>" 25 new signatures
    gpg: key 2072D77A: "Sébastien Luttringer <seblu@seblu.net>" 4 new subkeys
    gpg: key 91B842AE: "Jakob Gruber <jakob.gruber@gmail.com>" 1 new signature
    gpg: key C0711BF1: "Rashif Rahman (Ray) <schiv@archlinux.org>" 1 new signature
    gpg: key 8406FFF3: "Ronald van Haren <ronald@archlinux.org>" 1 new signature
    gpg: key 2D1493D2: "Rémy Oudompheng <remy@archlinux.org>" 12 new signatures
    gpg: key 9741E8AC: "Pierre Schmitz <pierre@archlinux.de>" 13 new signatures
    gpg: key B250F0D3: "Fabio Castelli <muflone@vbsimple.net>" 86 new signatures
    gpg: key DA2EE423: "Massimiliano Torromeo (Personal non-work identity) <massimiliano.torromeo@gmail.com>" 1 new signature
    gpg: key 9326B440: "Lukas Fleischer <info@cryptocrack.de>" 3 new signatures
    gpg: key D1CEDDAC: "Laurent Carlier <lordheavym@gmail.com>" 1 new signature
    gpg: key BAB142C1: "Kyle Keen <keenerd@gmail.com>" 1 new signature
    gpg: key C2E5C0D2: "Xyne. <xyne@archlinux.ca>" 6 new signatures
    gpg: key AB441196: "Stéphane Gaudreault <stephane@archlinux.org>" 6 new signatures
    gpg: key 70E80477: "Роман Кирилич (Roman Kyrylych) <roman@archlinux.org>" 5 new signatures
    gpg: key E19DAA50: "Peter Richard Lewis <pete@muddygoat.org>" 153 new signatures
    gpg: key D21E1A55: "Kaiting Chen <kaitocracy@gmail.com>" 5 new signatures
    gpg: key 983D4366: "Justin Davis (juster) <jrcd83@gmail.com>" 8 new signatures
    gpg: key 3CFD4BB6: "Jonathan Conder <jonno.conder@gmail.com>" 5 new signatures
    gpg: key 1985A992: "Dieter Plaetinck <dieter@plaetinck.be>" 6 new signatures
    gpg: key 99AD6E84: "Gavin Marciniak-Bisesi <Daenyth@gmail.com>" 13 new signatures
    gpg: key C6C11350: "Federico Cinelli <cinelli.federico@gmail.com>" 5 new signatures
    gpg: key 6521E06D: "Christopher Brannon <teiresias@gentoo.org>" 8 new signatures
    gpg: key 8F173680: "Xyne. (key #3) <xyne@archlinux.ca>" 3 new signatures
    gpg: key 437520BD: "Vesa Kaihlavirta <vegai@iki.fi>" 1 new signature
    gpg: key 295AFBF4: "Thorsten Töpper <atsutane@freethoughts.de>" 123 new signatures
    gpg: Total number processed: 76
    gpg:           new user IDs: 14
    gpg:            new subkeys: 4
    gpg:         new signatures: 1457
    gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
    gpg: depth: 0  valid:   1  signed:   5  trust: 0-, 0q, 0n, 0m, 0f, 1u
    gpg: depth: 1  valid:   5  signed:  59  trust: 0-, 0q, 0n, 5m, 0f, 0u
    gpg: depth: 2  valid:  59  signed:   5  trust: 59-, 0q, 0n, 0m, 0f, 0u
    gpg: next trustdb check due at 2016-01-22

I tried again to upgrade my laptop, but again, the devil had not gone away::

    [root@naughty-box] pacman -Syu
    :: Proceed with installation? [Y/n] Y
    (299/299) checking keys in keyring                                                         [####################################################] 100%
    downloading required keys...
    :: Import PGP key 2048R/, "Christian Hesse (Arch Linux Package Signing) <arch@eworm.de>", created: 2011-08-12? [Y/n] Y
    error: key "Christian Hesse (Arch Linux Package Signing) <arch@eworm.de>" could not be imported
    error: required key missing from keyring
    error: failed to commit transaction (unexpected error)
    Errors occurred, no packages were upgraded.

The error is quite different this time, but this is still about a missing PGP key. So, another of the few useful commands here::

    [root@naughty-box] pacman-key --recv-keys A6234074498E9CEE
    gpg: key 498E9CEE: public key "Christian Hesse (Arch Linux Package Signing) <arch@eworm.de>" imported
    gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
    gpg: depth: 0  valid:   1  signed:   5  trust: 0-, 0q, 0n, 0m, 0f, 1u
    gpg: depth: 1  valid:   5  signed:  60  trust: 0-, 0q, 0n, 5m, 0f, 0u
    gpg: depth: 2  valid:  60  signed:   5  trust: 60-, 0q, 0n, 0m, 0f, 0u
    gpg: next trustdb check due at 2016-01-22
    gpg: Total number processed: 1
    gpg:               imported: 1
    ==> Updating trust database...
    gpg: next trustdb check due at 2016-01-22

After what ``pacman -Syu`` succeed to upgrade my system, especially the ``archlinux-keyring`` package::

    [root@naughty-box] pacman -Syu
    ( 33/299) upgrading archlinux-keyring              [################] 100%
    ==> Appending keys from archlinux.gpg...
    gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
    gpg: depth: 0  valid:   1  signed:   5  trust: 0-, 0q, 0n, 0m, 0f, 1u
    gpg: depth: 1  valid:   5  signed:  62  trust: 0-, 0q, 0n, 5m, 0f, 0u
    gpg: depth: 2  valid:  62  signed:   5  trust: 62-, 0q, 0n, 0m, 0f, 0u
    gpg: next trustdb check due at 2016-01-22
    ==> Locally signing trusted keys in keyring...
      -> Locally signing key 0E8B644079F599DFC1DDC3973348882F6AC6A4C2...
      -> Locally signing key 684148BB25B49E986A4944C55184252D824B18E8...
      -> Locally signing key 44D4A033AC140143927397D47EFD567D4C7EA887...
      -> Locally signing key 27FFC4769E19F096D41D9265A04F9397CDFD6BB0...
      -> Locally signing key AB19265E5D7D20687D303246BA1DFB64FFF979E7...
    ==> Importing owner trust values...
    ==> Disabling revoked keys in keyring...
      -> Disabling key F5A361A3A13554B85E57DDDAAF7EF7873CFD4BB6...
      -> Disabling key 7FA647CD89891DEDC060287BB9113D1ED21E1A55...
      -> Disabling key D4DE5ABDE2A7287644EAC7E36D1A9E70E19DAA50...
      -> Disabling key BC1FBE4D2826A0B51E47ED62E2539214C6C11350...
      -> Disabling key 9515D8A8EAB88E49BB65EDBCE6B456CAF15447D5...
      -> Disabling key 4A8B17E20B88ACA61860009B5CED81B7C2E5C0D2...
      -> Disabling key 63F395DE2D6398BBE458F281F2DBB4931985A992...
      -> Disabling key 0B20CA1931F5DA3A70D0F8D2EA6836E1AB441196...
      -> Disabling key 8F76BEEA0289F9E1D3E229C05F946DED983D4366...
      -> Disabling key 66BD74A036D522F51DD70A3C7F2A16726521E06D...
      -> Disabling key 81D7F8241DB38BC759C80FCE3A726C6170E80477...
      -> Disabling key E7210A59715F6940CF9A4E36A001876699AD6E84...
    ==> Updating trust database...
    gpg: next trustdb check due at 2016-01-22


Ressources
----------

    - https://bbs.archlinux.org/viewtopic.php?pid=1501181#p1501181
    - https://bugs.archlinux.org/task/42798#comment132380
