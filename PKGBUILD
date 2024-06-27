# Maintainer : Daniel Bermond <dbermond@archlinux.org>
# Contributor: dummyx <dummyxa at gmail dot com>

pkgname=vvenc
pkgver=1.11.1
pkgrel=1
pkgdesc='A H.266/VVC (Versatile Video Coding) encoder'
arch=('x86_64')
url='https://github.com/fraunhoferhhi/vvenc/'
license=('BSD-3-Clause-Clear')
depends=('gcc-libs')
makedepends=('cmake')
source=("https://github.com/fraunhoferhhi/vvenc/archive/v${pkgver}/${pkgname}-${pkgver}.tar.gz")
sha256sums=('4f0c8ac3f03eb970bee7a0cacc57a886ac511d58f081bb08ba4bce6f547d92fa')

build() {
    cmake -B build -S "${pkgname}-${pkgver}" \
        -G 'Unix Makefiles' \
        -DBUILD_SHARED_LIBS:BOOL='ON' \
        -DCMAKE_BUILD_TYPE:STRING='None' \
        -DCMAKE_INSTALL_PREFIX:PATH='/usr' \
        -DCMAKE_SKIP_RPATH:BOOL='YES' \
        -DVVENC_INSTALL_FULLFEATURE_APP:BOOL='ON' \
        -Wno-dev
    cmake --build build
}

check() {
    export LD_LIBRARY_PATH="${srcdir}/build/source/Lib/vvenc"
    ctest --test-dir build --output-on-failure
}

package() {
    DESTDIR="$pkgdir" cmake --install build
    cp -dr --no-preserve='ownership' build/source/Lib/vvenc/*.so* "${pkgdir}/usr/lib"
    install -D -m755 build/source/App/{vvencapp/vvencapp,vvencFFapp/vvencFFapp} -t "${pkgdir}/usr/bin"
    install -D -m644 "${pkgname}-${pkgver}/LICENSE.txt" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
