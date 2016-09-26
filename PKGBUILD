# $Id: PKGBUILD 187474 2016-08-24 20:06:20Z arodseth $
# Maintainer: Alexander F Rødseth <xyproto@archlinux.org>
# Contributor: Christopher Reimer <mail+aur@c-reimer.de>

pkgname=cargo
pkgver=0.12.0
pkgrel=1
pkgdesc='Rust package manager'
url='http://crates.io/'
arch=('x86_64' 'i686')
license=('APACHE' 'MIT' 'custom')
depends=('curl' 'rust')
makedepends=('git' 'python' 'cmake')
options=('!emptydirs')
source=("git+https://github.com/rust-lang/cargo.git#tag=$pkgver"
         "https://raw.githubusercontent.com/gzmorell/cargo-arm-pkgbuild/master/platforms.patch")
md5sums=('SKIP'
         '436aab9a4b8aa9c75af96ee14f0da014')

prepare() {
  for cmd in init update; do (cd "$pkgname"; git submodule "$cmd"); done
  sed 's^share/doc^share/licenses^g' -i "$pkgname/Makefile.in"
  cd "$pkgname"
  patch -p1 -i ../platforms.patch
}

build() {
  cd "$pkgname"

  ./configure --prefix=/usr --enable-optimize
  make
}

package() {
  cd "$pkgname"

  make DESTDIR="$pkgdir" install

  # Remove files that contains references to $srcdir or $pkgdir,
  # or that conflicts with the rust package.
  rm -f "$pkgdir/usr/lib/rustlib/"{install.log,manifest-cargo,uninstall.sh}

  install -d "$pkgdir/usr/share/bash-completion/completions"
  mv "$pkgdir/usr/etc/bash_completion.d/cargo" \
    "$pkgdir/usr/share/bash-completion/completions/cargo"
}

# vim:set ts=2 sw=2 et:
