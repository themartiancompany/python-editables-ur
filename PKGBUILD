# Maintainer:
# Contributor: Mark Wagie <mark dot wagie at tutanota dot com>

pkgname=python-editables
pkgver=0.5
pkgrel=1
pkgdesc='Python library for creating editable wheels'
arch=('any')
url='https://github.com/pfmoore/editables'
license=('MIT')
depends=('python')
makedepends=(
  'git'
  'python-build'
  'python-installer'
  'python-flit-core'
)
checkdepends=('python-pytest')
_commit='2331418af764ef203959327354335c431978e70d'
source=("$pkgname::git+$url#commit=$_commit")
b2sums=('SKIP')

pkgver() {
  cd "$pkgname"

  git describe --tags | sed 's/^v//'
}

build() {
  cd "$pkgname"

  python -m build --wheel --no-isolation
}

check() {
  cd "$pkgname"

  PYTHONPATH="$(pwd)/src" pytest -v
}

package() {
  cd "$pkgname"

  python -m installer --destdir="$pkgdir" dist/*.whl

  # symlink license file
  local site_packages=$(python -c "import site; print(site.getsitepackages()[0])")
  install -d "$pkgdir/usr/share/licenses/$pkgname"
  ln -s "$site_packages/${pkgname#python-}-$pkgver.dist-info/LICENSE.txt" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE.txt"
}
