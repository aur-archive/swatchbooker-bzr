# Creator: Cristóvão Duarte Sousa <crisjss@gmail.com>
pkgname=swatchbooker-bzr
pkgver=145
pkgrel=3
pkgdesc="Swatch book viewer/convertor/editor (with patch for pantone bridge colors)"
arch=('i686' 'x86_64')
url="http://www.selapa.net/swatchbooker/"
license=('GPL')
groups=()
depends=('python2' 'pyqt' 'lcms' 'python-imaging')
makedepends=('bzr')
provides=()
conflicts=('swatchbooker')
replaces=()
backup=()
options=()
install=
source=('pantone.py.diff')
noextract=()
md5sums=('660a0a7153c557f1809e9151c674c2a3')



_bzrtrunk='http://bazaar.launchpad.net/~olivier-berten/swatchbooker/trunk'
_bzrmod='swatchbooker'

build() {
  cd "$srcdir"
  msg "Connecting to Bazaar server...."

  if [[ -d "$_bzrmod" ]]; then
    cd "$_bzrmod" && bzr --no-plugins pull "$_bzrtrunk" -r "$pkgver"
    msg "The local files are updated."
  else
    bzr --no-plugins branch "$_bzrtrunk" "$_bzrmod" -q -r "$pkgver"
  fi

  msg "Bazaar checkout done or server timeout"
  
  msg "Applying patch..."
  cd "$srcdir"
  patch "$_bzrmod/src/swatchbook/websvc/pantone.py" pantone.py.diff || return 1
  
  msg "Starting build..."

  rm -rf "$srcdir/$_bzrmod-build"
  cp -r "$srcdir/$_bzrmod" "$srcdir/$_bzrmod-build"
  cd "$srcdir/$_bzrmod-build"

  python2 ./setup.py build
}

package() {
  cd "$srcdir/$_bzrmod-build"
  
  python2 ./setup.py install --prefix ${pkgdir}/usr/
  
  for i in ${pkgdir}/usr/bin/*; do sed -i 's/which python/which python2/g' $i; done
  for i in ${pkgdir}/usr/bin/*; do chmod +x $i; done
  for i in ${pkgdir}/usr/share/applications/*.desktop; do sed -i 's/Icon=swatchbooker/Icon=\/usr\/share\/icons\/swatchbooker.svg/g' $i; done
}

# vim:set ts=2 sw=2 et: