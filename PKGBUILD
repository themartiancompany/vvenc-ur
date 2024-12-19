# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer : Daniel Bermond <dbermond@archlinux.org>
# Contributor: dummyx <dummyxa at gmail dot com>

_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="gcc-libs"
elif [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
fi
_pkg=vvenc
pkgname="${_pkg}"
pkgver=1.13.0
pkgrel=1
pkgdesc='A H.266/VVC (Versatile Video Coding) encoder'
arch=(
  'x86_64'
  'i686'
  'arm'
  'aarch64'
  'armv7l'
  'armv6l'
  'mips'
  'powerpc'
  'pentium4'
)
_http="https://github.com"
_ns="fraunhoferhhi"
url="${_http}/${_ns}/${_pkg}"
license=(
  'BSD-3-Clause-Clear'
)
depends=(
  "${_libc}"
)
makedepends=(
  'cmake'
)
source=(
  "${url}/archive/v${pkgver}/${pkgname}-${pkgver}.tar.gz"
  "010-${_pkg}-fix-unit-test-with-shared-lib.patch::${url}/pull/492/commits/42763a903fe122bc203f3aa9bbe5a5f4754758a0.patch"
)
sha256sums=(
  '28994435e4f7792cc3a907b1c5f20afd0f7ef1fcd82eee2af7713df7a72422eb'
  '324299c2f01d16c1c77175318ddd3d102e14508da1d4d380f9591b52a946bd2b'
)

prepare() {
  patch \
    -d \
      "${pkgname}-${pkgver}" \
    -Np1 \
    -i \
      "${srcdir}/010-vvenc-fix-unit-test-with-shared-lib.patch"
}

build() {
  local \
    _cmake_opts=()
  _cmake_opts=(
    -G
      'Unix Makefiles'
    -DBUILD_SHARED_LIBS:BOOL='ON'
    -DCMAKE_BUILD_TYPE:STRING='None'
    -DCMAKE_INSTALL_PREFIX:PATH='/usr'
    -DCMAKE_SKIP_RPATH:BOOL='YES'
    -DVVENC_INSTALL_FULLFEATURE_APP:BOOL='ON'
    -Wno-dev
  )
  cmake \
    -B \
      build \
    -S \
      "${pkgname}-${pkgver}" \
    "${_cmake_opts[@]}"
  cmake \
    --build \
      build
}

check() {
  export \
    LD_LIBRARY_PATH="${srcdir}/build/source/Lib/${_pkg}"
  ctest \
    --test-dir \
      build \
    --output-on-failure
}

package() {
  DESTDIR="${pkgdir}" \
  cmake \
    --install \
      build
  cp \
    -dr \
    --no-preserve='ownership' \
    "build/source/Lib/${_pkg}/"*".so"* \
    "${pkgdir}/usr/lib"
  install \
    -Dm755 \
    "build/source/App/"{"vvencapp/vvencapp","${_pkg}FFapp/${_pkg}FFapp"} \
    -t \
    "${pkgdir}/usr/bin"
  install \
    -Dm644 \
    "${pkgname}-${pkgver}/LICENSE.txt" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
