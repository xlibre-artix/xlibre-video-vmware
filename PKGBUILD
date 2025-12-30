# Maintainer: artist for Artix Linux and XLibre <artist@artixlinux.org>

pkgname=xlibre-video-vmware
_pkgname=xf86-video-vmware
pkgver=25.0.0
pkgrel=8
pkgdesc="XLibre fork of X.org vmware video driver"
arch=(x86_64 aarch64)
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
  case "$CARCH" in
    "x86_64")
      CFLAGS=" -march=x86-64"
      ;;
    "aarch64")
      CFLAGS=" -march=armv8-a"
      ;;
    *)
      CFLAGS=" -march=native"
      ;;
  esac
  CFLAGS+=" -mtune=generic -O2 -pipe -fexceptions -Wp,-D_FORTIFY_SOURCE=3 -Wformat -Werror=format-security"
  CFLAGS+=" -fstack-clash-protection -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer"
  LDFLAGS=" -Wl,-O1 -Wl,--sort-common -Wl,--as-needed -Wl,-z,lazy -Wl,-z,relro -Wl,-z,pack-relative-relocs"
  if [[ $CARCH != 'aarch64' ]]; then
    CFLAGS+=" -fcf-protection"
  fi
  if [[ "$_pkgname" == *"xf86-input"* ]]; then
    CFLAGS+=" -fno-plt"
    LDFLAGS+=" -Wl,-z,now"
  fi
  if [[ "$_pkgname" == *"xf86-video-intel"* ]]; then
    CFLAGS+=" -fno-lto"
    LDFLAGS+=" -fno-lto"
  fi
  CXXFLAGS="${CFLAGS} -Wp,-D_GLIBCXX_ASSERTIONS"
  export CFLAGS="${CFLAGS}"
  export CXXFLAGS="${CXXFLAGS}"
  export LDFLAGS="${LDFLAGS}"

  cd ${_pkgname}-xlibre-${_pkgname}-${pkgver}
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
