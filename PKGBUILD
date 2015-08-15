# Maintainer: SpepS <dreamspepser at yahoo dot it>

pkgname=fabula
pkgver=0.8.2
pkgrel=1
pkgdesc="A Python Game Engine for adventure, role-playing games and more"
arch=(i686 x86_64)
url="http://fabula-engine.org/"
license=('GPL')
depends=('python-pygame-hg' 'tk')
makedepends=('python-cx_freeze')
source=("http://static.fabula-engine.org/$pkgname-$pkgver.zip")
md5sums=('c759318508632b40a31565556856e3ed')

build() {
  return 0
}

package() {
  cd "$srcdir/$pkgname-$pkgver"

  python setup.py install --prefix=/usr --root="$pkgdir/"

  # save logs in ~/.config/fabula
  sed -e "/{}\.log/s/\"/os.getenv('HOME') + \"\/.config\/$pkgname\//" -e "29iimport os" \
      -i ${pkgdir}/usr/lib/python3.2/site-packages/$pkgname/{run,core/client}.py

  # rm broken cx_freezed executables links
  find "$pkgdir/usr/bin" -type l -delete

  # rm useless folder (same content of /usr/lib/)
  rm -rf "$pkgdir/usr/share/$pkgname"

  # mv executables to /usr/lib (do not work in /usr/bin)
  mv ${pkgdir}/usr/bin/*.py \
    "$pkgdir/usr/lib/$pkgname-$pkgver"

  # create launchers
  cd "$pkgdir/usr/lib/$pkgname-$pkgver"
  for _exe in run*.py ; do
    cat << EOF > ${pkgdir}/usr/bin/${_exe%%.*}
#!/bin/bash
[ -d "\$HOME/.config/$pkgname" ] || mkdir "\$HOME/.config/$pkgname"
cd /usr/lib/$pkgname-$pkgver && python $_exe
EOF
  done

  chmod +x ${pkgdir}/usr/bin/*
}

# vim:set ts=2 sw=2 et:
