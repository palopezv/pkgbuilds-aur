# Contributor: phoenixlzx < phoenixlzx at phoenixsec.org >
# Contributor: vorbote

pkgname=vuze
pkgver=4.8.1.2
pkgrel=2
_extra=a
pkgdesc="One of the most powerful bitTorrent client with GUI in the world, written in Java."
arch=('i686' 'x86_64')
url="http://azureus.sf.net/"
license=('GPL')
depends=('java-runtime' 'desktop-file-utils')
optdepends=('libgnomeui: for vuze GUI')
install=vuze.install
options=(!strip)

source=(
  "http://downloads.sourceforge.net/azureus/vuze/Vuze_${pkgver//./}/Vuze_${pkgver//./}${_extra}_linux.tar.bz2")

package() {
  cd "${srcdir}/${pkgname}"

  #install systemwide plugins
  mkdir -p "${pkgdir}/usr/share/vuze"
  cp -a "${srcdir}/${pkgname}/plugins" "${pkgdir}/usr/share/vuze/"
  chown -R root:root "${pkgdir}/usr/share/vuze/plugins"

  #install desktop entries
  install -Dm644 vuze.desktop  "${pkgdir}/usr/share/applications/vuze.desktop"
  install -Dm644 vuze.png "${pkgdir}/usr/share/pixmaps/vuze.png"
  install -Dm644 vuze.torrent.png "${pkgdir}/usr/share/pixmaps/vuze.torrent.png"
  install -Dm644 vuze.schemas "${pkgdir}/usr/share/gconf/schemas/vuze.schemas"

  # install SWT
  if [[ $CARCH == i686 ]] ; then
    install -Dm644 swt/swt32.jar "${pkgdir}/usr/share/vuze/swt32.jar"
  elif [[ $CARCH == x86_64 ]] ; then
    install -Dm644 swt/swt64.jar "${pkgdir}/usr/share/vuze/swt64.jar"
  fi

  # install vuze
  install -Dm755 vuze "${pkgdir}/usr/bin/vuze"
  sed -i 's|#PROGRAM_DIR="/home/username/apps/azureus"|PROGRAM_DIR="/usr/share/vuze"|' ${pkgdir}/usr/bin/vuze
  install -Dm644 Azureus2.jar "${pkgdir}/usr/share/vuze/Azureus2.jar"
}

sha256sums=('4a8c7b04af84d15a08ead14589d03fe276e131a0208534ed24a338982bbd5ad7')
