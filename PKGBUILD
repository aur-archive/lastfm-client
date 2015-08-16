# $Id: PKGBUILD 34579 2010-12-09 22:20:48Z mherych $
# Maintainer: Mateusz Herych <heniekk@gmail.com>

pkgname=lastfm-client
pkgver=1.5.4.26862
pkgrel=4
pkgdesc="The Last.fm client"
arch=('i686' 'x86_64')
url="http://www.last.fm/tools/downloads/"
license=('GPL')
depends=('libpng' 'libjpeg' 'libxinerama' 'libxi' 'fontconfig' 'alsa-lib' 'libxcursor' 'libsm' 'libxrandr' 'qt' 'fftw' 'libsamplerate' 'libmad')
optdepends=('libgpod: iPod support')
source=(http://ftp.de.debian.org/debian/pool/main/l/lastfm/lastfm_$pkgver+dfsg.orig.tar.gz
        lastfm.desktop
        build-fixes.diff
        set-firstrun-status.diff
        set-locale.diff
        makefile-qt45.patch
        qt46.diff)
md5sums=('c7991fd2636ca25e68ff476578b506a6'
         '9c5e444704d49cff7b1dc916f290bad0'
         'ae0e4a94af0d9e38172f064642a32e20'
         '494d7c336b09c7d579dad3cc7d7bc627'
         '1aeec4db77dc7cbc8e4660f127485599'
         'bea1168abcacef30832bb1e88a25b5f3'
         '8f9af912aa7eb84ef8d941bccdf6ee66')

build() {
   cd "$srcdir/lastfm-$pkgver+dfsg"
   patch -Np1 -i ../build-fixes.diff
   patch -Np1 -i ../set-locale.diff
   patch -Np1 -i ../set-firstrun-status.diff 
   patch -Np1 -i ../qt46.diff
   ./configure
   MAKEFLAGS=-j1 make src/Makefile
   patch -Np1 -i ../makefile-qt45.patch
   MAKEFLAGS=-j1 make
   (   make || (mv build/fplib/libfplib_debug.a build/fplib/libfplib.a && make ) )
}

package() {
        cd "$srcdir/lastfm-$pkgver+dfsg"
	mkdir -p $pkgdir/opt $pkgdir/usr/bin
	cp -rp bin/ $pkgdir/opt/last.fm
	mkdir -p $pkgdir/opt $pkgdir/usr/bin
	cp -rp  bin/ $pkgdir/opt/last.fm
	printf "#!/bin/sh\nexec /opt/last.fm/last.fm.sh\n" > $pkgdir/usr/bin/lastfm
	chmod +x $pkgdir/usr/bin/lastfm
	install -D -m 644 $srcdir/lastfm.desktop $pkgdir/usr/share/applications/lastfm.desktop
}
