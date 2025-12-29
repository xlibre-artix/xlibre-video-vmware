# Maintainer: artist for Artix Linux and XLibre <artist@artixlinux.org>

pkgname=xlibre-video-vmware
_pkgname=xf86-video-vmware
pkgver=25.0.0
pkgrel=6
pkgdesc="XLibre fork of X.org vmware video driver"
arch=(x86_64)
license=('MIT AND X11')
_pkgname="${pkgname//xlibre/xf86}"
url="https://github.com/X11Libre/${_pkgname}"
depends=("xlibre-xserver>=${pkgver%.*}" 'glibc')
makedepends=("xlibre-xserver-devel>=${pkgver%.*}" 'xorgproto')
conflicts=("${_pkgname}")
provides=("${_pkgname}")
source=("${url}/archive/refs/tags/xlibre-${_pkgname}-${pkgver}.tar.gz")
groups=('xlibre-drivers')
depends+=('mesa' 'libxext' 'libx11' 'libdrm')
provides+=('xf86-video-vmware')   # for virtualbox-guest-utils / nous
options=('!emptydirs')

build() {
  cd ${_pkgname}-xlibre-${_pkgname}-${pkgver}
  export CFLAGS=${CFLAGS/-fno-plt}
  export CXXFLAGS=${CXXFLAGS/-fno-plt}
  export LDFLAGS=${LDFLAGS/-Wl,-z,now}

  ./autogen.sh
  ./configure --prefix=/usr --enable-vmwarectrl-client
  make
}

package() {
  cd ${_pkgname}-xlibre-${_pkgname}-${pkgver}
  make DESTDIR="${pkgdir}" install
  
  install -m755 -d "${pkgdir}/usr/share/licenses/xlibre-${_pkgname}"
  install -m644 COPYING "${pkgdir}/usr/share/licenses/xlibre-${_pkgname}/"
}

sha256sums=('5aabf5932fe071c21f621a5bfa5bc93523cbd6ef10cef5f3ecf89bf0ea66bbb4')
