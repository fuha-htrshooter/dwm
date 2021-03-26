pkgname=dwm-git
_pkgname=dwm
pkgver=6.2.r8.g61bb8b2
pkgrel=1
pkgdesc="A dynamic window manager for X"
url="http://dwm.suckless.org"
arch=('i686' 'x86_64')
license=('MIT')
options=(zipman)
depends=('libx11' 'libxinerama' 'libxft')
makedepends=('git')
install=dwm.install
provides=('dwm')
conflicts=('dwm')
source=(dwm.desktop
        "$_pkgname::git+http://git.suckless.org/dwm"
        config.h
        https://dwm.suckless.org/patches/awesomebar/dwm-awesomebar-20191003-80e2a76.diff
        https://dwm.suckless.org/patches/gridmode/dwm-gridmode-20170909-ceac8c9.diff
        https://dwm.suckless.org/patches/statusallmons/dwm-statusallmons-20160731-56a31dc.diff
        https://dwm.suckless.org/patches/attachbelow/dwm-attachbelow-6.2.diff
        https://dwm.suckless.org/patches/cursorwarp/dwm-cursorwarp-mononly-20210222-61bb8b2.diff
        awesomebar-multimons.diff
        dwm-extrabar-6.2-rebuild.diff
        dwm-statusmonsbutton.patch)
# so you can customize config.h and local-patches
md5sums=('939f403a71b6e85261d09fc3412269ee'
         'SKIP'
         'SKIP'
         '5754406d68c49ca6db158b9569f45de7'
         '6055775113fd4dc06200bc6aaafb72fb'
         'f3cd9551364f69f247eacf0c4b15d885'
         '4d221bca8b074097748414029f175459'
         '25b1b39ba1f8840ed3f80c4f72ca5cb0'
         'SKIP'
         'SKIP'
         'SKIP')

pkgver(){
  cd $_pkgname
  git describe --long --tags | sed -E 's/([^-]*-g)/r\1/;s/-/./g'
}

prepare() {
  cd $_pkgname
  if [[ -f "$srcdir/config.h" ]]; then
    cp -fv "$srcdir/config.h" config.h
  fi

  # remove old .rej
  find $srcdir/ -name '*.rej' | xargs rm

  # awesomebar
  patch -p1 -i "$srcdir/dwm-awesomebar-20191003-80e2a76.diff"
  patch -p1 -i "$srcdir/awesomebar-multimons.diff"
  # cursorwarp
  patch -p1 -i "$srcdir/dwm-cursorwarp-mononly-20210222-61bb8b2.diff"
  # attachbelow
  patch -p1 -i "$srcdir/dwm-attachbelow-6.2.diff"
  # gridmode
  rm -fv layouts.c # remove old
  patch -p1 -i "$srcdir/dwm-gridmode-20170909-ceac8c9.diff"
  # statusallmons
  patch -p1 -i "$srcdir/dwm-statusallmons-20160731-56a31dc.diff"
  # extrabar
  patch -p1 -i "$srcdir/dwm-extrabar-6.2-rebuild.diff"
  # statusmonsbutton
  patch -p1 -i "$srcdir/dwm-statusmonsbutton.patch"

}

build() {
  cd $_pkgname
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  cd $_pkgname
  make PREFIX=/usr DESTDIR="$pkgdir" install
  install -m644 -D LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -m644 -D README "$pkgdir/usr/share/doc/$pkgname/README"
  install -m644 -D ../dwm.desktop "$pkgdir/usr/share/xsessions/dwm.desktop"
}

# vim:set ts=2 sw=2 et:
