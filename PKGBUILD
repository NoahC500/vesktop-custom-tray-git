# Maintainer: Noah Craig <noahdcraig@outlook.com>
pkgname=vesktop-custom-tray-git
pkgver=v1.5.3.r102.gb1c3aba
pkgrel=1
pkgdesc="Discord client with Vencord with system-tray customisability patch preinstalled, using system electron"
arch=('x86_64')
url="https://github.com/Vencord/Vesktop"
license=('GPL-3.0-or-later')
groups=()
depends=('electron32')
makedepends=('git' 'pnpm')
provides=('vesktop')
conflicts=('vesktop' 'vesktop-bin' 'vesktop-git' 'vesktop_electron')
source=("$pkgname::git+https://github.com/Vencord/Vesktop.git" 'https://raw.githubusercontent.com/Vencord/Vesktop/refs/heads/main/pnpm-lock.yaml' 'vesktop.desktop' 'vesktop.sh')
sha256sums=('SKIP' 'SKIP' '831468f389793ad379bac4d100ebe2b604b93bb58510ae04c7493ad6a10b37f1' 'f22a064b874dcc08a225b343759e76b1ca7c232ba1bcc0b53d155aaf6b6e0817')

pkgver() {
  cd "$pkgname"
  git describe --long --tags --abbrev=7 | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
# Pull and merge PR#517
  cd $srcdir/$pkgname/
  git fetch origin pull/517/head:tray-patch
  git switch tray-patch
  cp "$srcdir/pnpm-lock.yaml" "$srcdir/${pkgname}/"  # Overwrites PR's pnpm-lock with origin's (doesn't break anything in my experience)
  git commit pnpm-lock.yaml -m "Using origin's pnpm-lock"
  git switch main
  git merge tray-patch -m "Merging new-tray branch into main"

# Ensures the app's package.json has the correct Electron version (as of writing 32 is the default though)
  sed -i '/linux/s/^/        "electronDist": "\/usr\/lib\/electron32",\n/' "$srcdir/$pkgname/package.json"
}

build() {
  cd "$srcdir/$pkgname"
  pnpm i
  pnpm package:dir
}

package() {
  install -d "$pkgdir/usr/lib/${pkgname}"

  cp "$srcdir/$pkgname/dist/linux-unpacked/resources/app.asar" "$pkgdir/usr/lib/${pkgname}/"
  install -Dm 755 "vesktop.sh" "$pkgdir/usr/bin/vesktop"

  install -Dm 644 "vesktop.desktop" "$pkgdir/usr/share/applications/vesktop.desktop"
  install -Dm 644 "$srcdir/$pkgname/static/icon.png" "$pkgdir/usr/share/pixmaps/vesktop.png"
  install -Dm 644 "$srcdir/$pkgname/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
