# Maintainer: modprobe

pkgname=bitsharesx
pkgver=0.4.23.1
pkgrel=1
pkgdesc="A decentralized, P2P bank and exchange"
arch=('i686' 'x86_64')
url="https://github.com/DacSunLimited/bitsharesx"
depends=('qt5-base' 'qt5-imageformats' 'qt5-webkit' 'boost' 'zlib')
makedepends=('git' 'cmake' 'clang' 'nodejs')
conflicts=('bitsharesx-git')
source=("git://github.com/DacSunLimited/bitsharesx.git" "BitSharesX.desktop")
sha256sums=('SKIP' '9da61f0976a6cd7a3034f66b54387524f31141ca70aecae37648787d555b27e2')
_gitname="bitsharesx"
license=("Public Domain")

build() {
  cd "$srcdir/$_gitname"
  
  #Fetch submodules
  git checkout "v$pkgver"
  git submodule init
  git submodule update
  
  #Prepare NodeJS web GUI dependencies
  cd programs/web_wallet
  npm install lineman
  npm install

  #Do the actual build
  cd -
  #Due to a bug in GCC 4.9.1 which prevents it from building BitSharesX, we must use clang instead.
  cmake -DCMAKE_C_COMPILER=/usr/bin/clang -DCMAKE_CXX_COMPILER=/usr/bin/clang++ -DCMAKE_BUILD_TYPE=Release -DINCLUDE_QT_WALLET=ON .
  make forcebuildweb
  make bitshares_client BitSharesX
}

package() {
  cd "$srcdir/$_gitname"

  #Do the actual install; it's really simple, just 4 files
  mkdir -p $pkgdir/usr/bin $pkgdir/usr/share/icons $pkgdir/usr/share/applications
  install -D -m644 $startdir/BitSharesX.desktop $pkgdir/usr/share/applications/BitSharesX.desktop
  install -D -m644 ./programs/qt_wallet/images/qtapp80.png $pkgdir/usr/share/icons/BitSharesX.png
  install -D -m755 ./programs/client/bitshares_client $pkgdir/usr/bin/bitsharesx-cli
  install -D -m755 ./programs/qt_wallet/bin/BitSharesX $pkgdir/usr/bin/bitsharesx-gui
}
