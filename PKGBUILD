# $Id: PKGBUILD 185574 2013-05-15 08:05:34Z bpiotrowski $
# Maintainer: Philipp Gesang
#
# This is the default Arch PKGBUILD from the ABS tree, extended with
# the NNTP patches by Vsevolod Volkov:
#
#       http://mutt.org.ua/download/mutt-1.5.21/
#
# The build() instructions follow the RPM spec by the patch author.
#
# 2014-03-19:
#       - Update PKGBUILD for 1.5.23.
#       - Remove two patches.
#       - Remove references to files no longer part of the patch.

pkgname=mutt-nntp
pkgver=1.5.23
pkgrel=2
pkgdesc='Small but very powerful text-based mail client with NNTP patch'
conflicts=("mutt")
url='http://www.mutt.org/'
license=('GPL')
backup=('etc/Muttrc')
arch=('i686' 'x86_64')
optdepends=('smtp-forwarder: to send mail')
depends=('gpgme' 'ncurses' 'openssl' 'libsasl' 'gdbm' 'libidn' 'mime-types' 'krb5')
# looks like "ftp://ftp.mutt.org/mutt/mutt-${pkgver}.tar.gz" is no longer
# available, falling back on bitbucket download
source=("https://bitbucket.org/mutt/mutt/downloads/mutt-${pkgver}.tar.gz"
        "http://mutt.org.ua/download/mutt-${pkgver}/patch-${pkgver}.rr.compressed.gz"
        "http://mutt.org.ua/download/mutt-${pkgver}/patch-${pkgver}.vvv.initials.gz"
        "http://mutt.org.ua/download/mutt-${pkgver}/patch-${pkgver}.vvv.nntp.gz"
        "http://mutt.org.ua/download/mutt-${pkgver}/patch-${pkgver}.vvv.nntp_ru.gz"
        "http://mutt.org.ua/download/mutt-${pkgver}/patch-${pkgver}.vvv.quote.gz"
        "http://mutt.org.ua/download/mutt-${pkgver}/patch-${pkgver}.vvv.ru.gz"
        "http://mutt.org.ua/download/mutt-${pkgver}/patch-${pkgver}.vvv.uk.gz")

sha512sums=('f1b4a7230253651857f61bd7215cce870a613012f613d4c907d401556083726c8ed7d429d57a8bf858c3b5b23683380d4c1494540d86ca80813e22cb6b95bc1e'
            '5879f4594f468f848acc20a576a30b4ecf75cdbdcd9956a6daef52c6a7192be65ec874091ab08f65f6587307ebf6110afe22af049d4dd98ccb7f47ec82aa8a16'
            '0e2c2a74627b4763777e90d2c637a332b09cbea48c64eab3cf61f770816c13ec171e1aba05f0358043f4ce96ca31830cb93575fc94cc9379c5a0a0b290481fa8'
            '0f209d37fbd0f8d9fb24b5342d1d2eb31ac82ff5d7ddfe0dbd0143a905c18e71dcb5b55eda27c9622545a211c3744ecccc0479d3dd6583d5ba54c00d32c56a35'
            'c7a34b537044c0a736bbac2a3811445c14161a59e8d9b62a357caa91bad93d02232cb7eed4eb08e9b58a5669b51c24c382573364003f511558f7d3cb1ffdf0bd'
            '22d4c2d40a1946114a6340933da44a99d55e698988c188b0180e63bdaec98cb1337a9143a6355e3c9ffd33044016200b076a242a787fe56ff7b1235b3eee62e6'
            '3fa387d0ea2ad51f2406e38178de638da01d507ad51d573a785e587ea98d06ab54c307a0bb231ad910add91f1eab52e7b9c474c49e98564401eb7b4b5de0f318'
            'f47df469941881df4a976e6fdf0994e54103ec625ec8361115debe19d5ebb99e64a25c1b1bbba0eceeaa675519a812335008689cb55d1a06ad3f9a8ff6274bf7')

install=install

prepare() {
    cd "${srcdir}/mutt-${pkgver}"
    echo " * applying NNTP patches"
    patch -p1    < ../patch-${pkgver}.rr.compressed
    patch -p1    < ../patch-${pkgver}.vvv.nntp
    patch -p1    < ../patch-${pkgver}.vvv.nntp_ru
    patch -p1    < ../patch-${pkgver}.vvv.initials
    patch -p1    < ../patch-${pkgver}.vvv.quote
    patch -p1    < ../patch-${pkgver}.vvv.ru
    echo " * invoking autotools"
    aclocal -I m4
    autoheader
    automake --add-missing --foreign
    autoconf
    echo " * invoking configure"
    ./configure                         \
            --prefix=/usr               \
            --sysconfdir=/etc           \
            --enable-compressed         \
            --enable-gpgme              \
            --enable-pop                \
            --enable-imap               \
            --enable-nntp               \
            --enable-smtp               \
            --enable-hcache             \
            --with-curses=/usr          \
            --with-regex                \
            --with-gss=/usr             \
            --with-ssl=/usr             \
            --with-sasl                 \
            --with-idn
}

build() {
    cd "${srcdir}/mutt-${pkgver}"
    echo " * building package ${pkgname} ${pkgver}"
    make
}

package() {
    cd "${srcdir}/mutt-${pkgver}"
    echo " * installing ${pkgname} ${pkgver}"
    make DESTDIR="${pkgdir}" install

    rm "${pkgdir}"/usr/bin/{flea,muttbug}
    rm "${pkgdir}"/usr/share/man/man1/{flea,muttbug}.1
    rm "${pkgdir}"/etc/mime.types{,.dist}
    install -Dm644 contrib/gpg.rc "${pkgdir}"/etc/Muttrc.gpg.dist
}

