# Maintainer: dummyx <dummyxa@gmail.com>

pkgname=vvenc
pkgver=1.0.0
pkgrel=1
pkgdesc="Fraunhofer Versatile Video Encoder (VVenC)"
arch=('x86_64')
url='https://github.com/fraunhoferhhi/vvenc'
license=('custom')
depends=('gcc-libs')
makedepends=('cmake>=3.12' 'gcc>=7')
provides=('vvenc')
conflicts=('vvenc-git')
source=("vvenc-${pkgver}.tar.gz::https://github.com/fraunhoferhhi/vvenc/archive/v${pkgver}.tar.gz")
sha256sums=('1f81fffff2e83ca5072ca3a652134a693d03897f4f49f5a3843cc575dccc2c4e')

build() {
	cd "${srcdir}/vvenc-${pkgver}"
	export CFLAGS+=" ${CPPFLAGS}"
	export CXXFLAGS+=" ${CPPFLAGS}"
	make install-release-shared
}

package() {
	cd "${srcdir}/vvenc-${pkgver}"
	cp -r "install" "$pkgdir/usr"
	mkdir -p "$pkgdir/usr/share/licenses/vvenc"
	install -Dm644 "LICENSE.txt" "$pkgdir/usr/share/licenses/vvenc/"
}