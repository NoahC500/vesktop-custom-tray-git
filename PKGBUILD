# Maintainer: Noah Craig <noahdcraig@outlook.com>
pkgname=vesktop-custom-tray-git
pkgver=v1.5.3.r92.g6cbd169
pkgrel=1
pkgdesc="Discord client with Vencord with system-tray customisability patch preinstalled, using system electron"
arch=('x86_64')
url="https://github.com/Vencord/Vesktop"
license=('GPL-3.0-or-later')
groups=()
depends=('electron29')
makedepends=('git' 'pnpm')
provides=('vesktop')
conflicts=('vesktop' 'vesktop-bin' 'vesktop-git' 'vesktop_electron')
replaces=()
backup=()
options=()
install=
source=("$pkgname::git+https://github.com/Vencord/Vesktop.git" 'vesktop.desktop' 'vesktop.sh')
noextract=()
sha256sums=('SKIP' 'SKIP' '48f937289c2763396014835192cbd779a09187cc8682ed331636b99053f85b0e')

pkgver() {
  cd "$pkgname"
  git describe --long --tags --abbrev=7 | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cd $srcdir/$pkgname/
  # Pull and merge patch
  git fetch origin pull/517/head:tray-patch
  git merge tray-patch -m "Merging new-tray branch into main"
  # Use system's electron
  sed -i '/linux/s/^/        "electronDist": "\/usr\/lib\/electron29",\n/' "$srcdir/$pkgname/package.json"
}

build() {
  cd "$srcdir/$pkgname"
  COREPACK_ENABLE_STRICT=0 pnpm i
  COREPACK_ENABLE_STRICT=0 pnpm package:dir
}

package() {
  install -d "$pkgdir/usr/lib/${pkgname}"

  cp "$srcdir/$pkgname/dist/linux-unpacked/resources/app.asar" "$pkgdir/usr/lib/${pkgname}/"
  install -Dm 755 "vesktop.sh" "$pkgdir/usr/bin/vesktop"

  install -Dm 644 "vesktop.desktop" "$pkgdir/usr/share/applications/vesktop.desktop"
  install -Dm 644 "$srcdir/$pkgname/static/icon.png" "$pkgdir/usr/share/pixmaps/vesktop.png"
  install -Dm 644 "$srcdir/$pkgname/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
