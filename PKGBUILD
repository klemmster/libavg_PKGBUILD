# Maintainer: Richard Klemm < klemmster@gmail.com>
# Previous Maintainer: Thomas Dziedzic < gostrc at gmail >
# Contributor: Alexander Baldeck <alexander@archlinux.org>
# Contributor: padfoot <padfoot@exemail.com.au>

pkgname=libavg
pkgver=1.7.1
pkgrel=3
pkgdesc='High-level multimedia platform with a focus on interactive art installations.'
arch=('i686' 'x86_64')
url='http://www.libavg.de'
depends=('libtool' 'librsvg' 'libxi' 'gdk-pixbuf2' 'libxml2' 'ffmpeg' 'boost' 'boost-libs' 'python2' 'pango')
install=$pkgname.install
optdepends=('libvdpau' 'libdc1394' )
makedepends=(python2)
license=('LGPL')
source=("https://www.libavg.de/site/attachments/download/126/libavg-1.7.1.tar.gz"
        "patch_gcc47.patch"
        $pkgname.sh
        $pkgname.csh
        )
md5sums=('63cb010baf08e6f147e00287a80d968a'
         '4a92b4dc6baec264564868e099e7fa82'
         'dc87612b5def50777621de5513694824'
         '6224961a395c77e5bfe2b008ddda024f'
         )

build() {
  cd ${pkgname}-${pkgver}
  patch -p0 -i ../patch_gcc47.patch


  # workaround for linking errors, caused by "-Wl,--as-needed" in /etc/makepkg.conf
  unset LDFLAGS

  # fix python2
  find -type f -exec sed -e 's_#!/usr/bin/python_&2_' -e 's_#!/usr/bin/env python_&2_' -i {} \;

  ./bootstrap

  PYTHON=/usr/bin/python2 ./configure \
    --prefix=/usr

  make ${MAKEFLAGS}
}

package() {
  cd ${pkgname}-${pkgver}
  make DESTDIR=${pkgdir} install
  install -D -m644 ${srcdir}/${pkgname}-${pkgver}/src/avgrc \
    ${pkgdir}/etc/avgrc
  cd "$pkgdir"
  install -Dm755 "$srcdir/$pkgname.sh" etc/profile.d/$pkgname.sh
  install -Dm755 "$srcdir/$pkgname.csh" etc/profile.d/$pkgname.csh
}

