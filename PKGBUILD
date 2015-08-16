
# Maintainer: (epsilom) Xavier Corredor <xavier.corredor.llano (a) gmail.com>
# Contributor: Dieter Rethmeyer <Dieter@rethmeyers.de>
# Contributor: Devaev Maxim <mdevaev@gmail.com>

pkgname=mgetty-voice
pkgver=1.1.37
pkgrel=5
pkgdesc="Mgetty is a versatile program to handle all aspects of a modem under Unix. The program 'mgetty' allows you to use a modem for handling external logins, receiving faxes and using the modem as a answering machine without interfering with outgoing calls."
url="http://mgetty.greenie.net/"
license=('GPL')
arch=('i686' 'x86_64')
install=mgetty.install
depends=('glibc' 'logrotate' 'udev' 'netpbm')
makedepends=('make')
source=(http://fossies.org/unix/misc/mgetty$pkgver-Jun05.tar.gz
    vgetty-avc.patch
    vgetty-avc-noop-duplex.patch
        Makefile.patch
        config.patch
        policy.patch
        90-mgetty.rules
        faxrunqd)
md5sums=('4df2eb47bd6d5318d3d642572ab56e51'
         'b46b82703d2c07c840b4a024bbcab8d6'
         'a28fa78c80a0790b35f5971c2269b3e0'
         'eaa2b17d77ca099ebb7e92cf2006f6c1'
         'd40de3f241a2851f091e0046cb7f28c0'
         '5556e5e88c784e75acb14ab998d7eb1a'
         '4b73a5654db86a34a8dccdf5f55c699c'
         '5c1888f399f4c3f9b3e26652cd759418')

build() {
  cd ${srcdir}/mgetty-${pkgver}
  cp policy.h-dist policy.h
  patch -Np0 -i ../../vgetty-avc.patch || return 1
  patch -Np0 -i ../../vgetty-avc-noop-duplex.patch || return 1
  patch -Np0 -i ../../config.patch || return 1
  patch -Np0 -i ../../policy.patch || return 1

  sed -i -e  's|-DCONF_DIR=\\"\$(CONFDIR)\\"|-DCONF_DIR=\\"/etc/mgetty+sendfax\\"|g' voice/libutil/Makefile

  make clean
  make || return 1
  make testdisk
  make test
}

package() {
  cd ${srcdir}/mgetty-${pkgver}
  mkdir -p ${pkgdir}/usr
  mkdir -p ${pkgdir}/var/spool
  make prefix=${pkgdir}/usr spool=${pkgdir}/var/spool CONFDIR=${pkgdir}/etc/mgetty+sendfax FAX_OUT_USER=0 install vgetty install-vgetty || return 1
  rm -f $startdir/pkg/usr/bin/g3topbm
  install -D -m644 ${srcdir}/90-mgetty.rules ${pkgdir}/etc/udev/rules.d/90-mgetty.rules
}
