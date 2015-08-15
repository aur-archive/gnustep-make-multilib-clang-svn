# Maintainer: X0rg

_svnname=gnustep-make
pkgname=$_svnname-multilib-clang-svn
pkgver=38389
pkgrel=6
pkgdesc="The GNUstep make package for multilib, using Clang"
arch=('x86_64')
url="http://www.gnustep.org/"
license=('GPL3')
groups=('gnustep-multilib-clang-svn')
depends=('bash' 'gcc-multilib>=4.9.0') # Clang needs symbol 'GLIBCXX_3.4.20' in /usr/lib/libstdc++.so.6
makedepends=('svn' 'clang')
conflicts=('gnustep-make' 'gnustep-make-clang-svn')
provides=('gnustep-make-clang-svn')
options=('!emptydirs')
install=merge.install
source=("$_svnname::svn://svn.gna.org/svn/gnustep/tools/make/trunk/"
	'arch32'
	'arch64')
md5sums=('SKIP'
         '2a6ac6a4b3feac708d3272829cfcdc37'
         '0daa9a592d808306193540bda0980673')

pkgver() {
  cd "$srcdir/$_svnname"
  svnversion | tr -d [A-z]
}

prepare() {
  msg2 "Make a clone of $_svnname"
  cp -Rv "$srcdir/$_svnname" "$srcdir/$_svnname-32"

  msg2 "Copy new file system layouts for Arch..."
  cp -v "$srcdir/arch64" "$srcdir/$_svnname/FilesystemLayouts/"
  cp -v "$srcdir/arch32" "$srcdir/$_svnname-32/FilesystemLayouts/"
}

build() {
  # 64-bit build
  cd "$srcdir/$_svnname"
  msg2 "Run 'configure' (64-bit)..."
  CC="clang" CXX="clang++" ./configure --prefix=/usr --sysconfdir=/etc/GNUstep --with-layout=arch64 --with-objc-lib-flag=-l:libobjc.so.4.6

  # 32-bit build
  cd "$srcdir/$_svnname-32"
  export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"
  msg2 "Run 'configure' (32-bit)..."
  CC="clang -m32" CXX="clang++ -m32" ./configure --prefix=/usr --libdir=/usr/lib32 --sysconfdir=/etc/GNUstep --with-layout=arch32 \
    --with-objc-lib-flag=-l:libobjc.so.4.6 --with-config-file=/etc/GNUstep/GNUstep32.conf
}

package() {
  # 64-bit install
  cd "$srcdir/$_svnname"
  msg2 "Install (64-bit)..."
  make DESTDIR="$pkgdir" install
  echo -e "# Added by $pkgname package\nexport PATH=$PATH" >> "$pkgdir/usr/share/GNUstep/Makefiles/GNUstep-reset.sh"

  # 32-bit install
  cd "$srcdir/$_svnname-32"
  msg2 "Install (32-bit)..."
  make DESTDIR="$pkgdir" install
  echo -e "# Added by $pkgname package\nexport PATH=$PATH" >> "$pkgdir/usr/share/GNUstep32/Makefiles/GNUstep-reset.sh"
}
