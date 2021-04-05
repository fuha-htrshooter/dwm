pkgname=dwm-git
_pkgname=dwm
pkgver=6.2.r9.g67d76bd
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
        dwm-awesomebar-20191003-80e2a76.diff
        dwm-gridmode-20170909-ceac8c9.diff
        dwm-statusallmons-20160731-56a31dc.diff
        dwm-attachbelow-6.2.diff
        dwm-cursorwarp-mononly-20210222-61bb8b2.diff
        awesomebar-multimons.diff
        dwm-extrabar-6.2-rebuild.diff
        dwm-statusmonsbutton.diff)
# so you can customize config.h and local-patches
md5sums=('939f403a71b6e85261d09fc3412269ee'
         'SKIP'
         'SKIP'
         '5754406d68c49ca6db158b9569f45de7'
         '6055775113fd4dc06200bc6aaafb72fb'
         'f3cd9551364f69f247eacf0c4b15d885'
         '4d221bca8b074097748414029f175459'
         '25b1b39ba1f8840ed3f80c4f72ca5cb0'
         '71a99c58c398d08d7dffac9e044f20b6'
         '5ce0f844b22cbfbc36dcc01612cfc611'
         '6af4764e5c41e41edbd2fc0bcad0d88f')

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
  find $srcdir/ -name '*.rej' -exec rm {} \;

  # awesomebar (https://dwm.suckless.org/patches/awesomebar/dwm-awesomebar-20191003-80e2a76.diff)
  patch -p1 -i "$srcdir/dwm-awesomebar-20191003-80e2a76.diff"
  patch -p1 -i "$srcdir/awesomebar-multimons.diff"
  # cursorwarp (https://dwm.suckless.org/patches/cursorwarp/dwm-cursorwarp-mononly-20210222-61bb8b2.diff)
  patch -p1 -i "$srcdir/dwm-cursorwarp-mononly-20210222-61bb8b2.diff"
  # attachbelow (https://dwm.suckless.org/patches/attachbelow/dwm-attachbelow-6.2.diff)
  patch -p1 -i "$srcdir/dwm-attachbelow-6.2.diff"
  # gridmode (https://dwm.suckless.org/patches/gridmode/dwm-gridmode-20170909-ceac8c9.diff)
  rm -fv layouts.c # remove old
  patch -p1 -i "$srcdir/dwm-gridmode-20170909-ceac8c9.diff"
  # statusallmons (https://dwm.suckless.org/patches/statusallmons/dwm-statusallmons-20160731-56a31dc.diff)
  patch -p1 -i "$srcdir/dwm-statusallmons-20160731-56a31dc.diff"
  # extrabar
  patch -p1 -i "$srcdir/dwm-extrabar-6.2-rebuild.diff"
  # statusmonsbutton
  patch -p1 -i "$srcdir/dwm-statusmonsbutton.diff"
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
