# Maintainer: Noah Craig <noahdcraig@outlook.com>
pkgname=vesktop-tray # '-bzr', '-git', '-hg' or '-svn'
pkgver=1.5.2
pkgrel=1
pkgdesc="Discord client with Vencord with system-tray customisability patch preinstalled, using system electron"
arch=('x86_64')
url="https://github.com/Vencord/Vesktop"
license=('GPL-3.0-or-later')
groups=()
depends=('electron29')
makedepends=('git' 'pnpm') # 'bzr', 'git', 'mercurial' or 'subversion'
provides=('vesktop')
conflicts=('vesktop' 'vesktop-bin' 'vesktop-git' 'vesktop_electron')
replaces=()
backup=()
options=()
install=
source=('vesktop-tray::git+https://github.com/NoahC500/vesktop-tray.git' 'vesktop.desktop' 'vesktop.sh')
noextract=()
sha256sums=('SKIP' '1fc38b41729f89c6075bc8d71e345597b1275ccc46973e48d059d7fa3cf41891' 'cbe7ee58fb9d04fbab2a2112e0ee4bd1c4810d4c22708ef83bcd4179fc528d94')

prepare() {
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
  install -d "$pkgdir/usr/bin/${pkgname}"

  cp "$srcdir/$pkgname/dist/linux-unpacked/resources/app.asar" "$pkgdir/usr/lib/${pkgname}/"
  install -Dm 755 "vesktop.sh" "$pkgdir/usr/bin/vesktop-tray"

  install -Dm 644 "vesktop.desktop" "$pkgdir/usr/share/applications/vesktop.desktop"
  install -Dm 644 "$srcdir/$pkgname/static/icon.png" "$pkgdir/usr/share/pixmaps/vesktop.png"
  install -Dm 644 "$srcdir/$pkgname/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
