# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Contributor: Mark Wagie <mark dot wagie at tutanota dot com>

_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=editables
pkgname="${_py}-${_pkg}"
pkgver=0.5
pkgrel=1
pkgdesc='Python library for creating editable wheels'
arch=(
  'any'
)
_http='https://github.com'
_ns='pfmoore'
url="${_http}/${_ns}/${_pkg}"
license=(
  'MIT'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
)
makedepends=(
  'git'
  "${_py}-build"
  "${_py}-installer"
  "${_py}-flit-core"
)
checkdepends=(
  "${_py}-pytest"
)
_commit='2331418af764ef203959327354335c431978e70d'
source=(
  "${pkgname}::git+${url}#commit=${_commit}"
)
b2sums=(
  'SKIP'
)

pkgver() {
  cd \
    "${pkgname}"
  git \
    describe \
      --tags | \
    sed \
      's/^v//'
}

build() {
  cd \
    "${pkgname}"
  "${_py}" \
    -m build \
    --wheel \
    --no-isolation
}

check() {
  cd \
    "${pkgname}"
  PYTHONPATH="$(pwd)/src" \
  pytest \
    -v
}

package() {
  cd \
    "${pkgname}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  # symlink license file
  local \
    site_packages=$( \
      python \
        -c \
	"import site; print(site.getsitepackages()[0])")
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${site_packages}/${pkg}-${pkgver}.dist-info/LICENSE.txt" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.txt"
}
