# Original maintainer: td123 <gostrc AT gmail DOT com>
# Previous maintainer: phoenixlzx < phoenixlzx AT phoenixsec DOT org >
# Maintainer:  P. A. López-Valencia <palopezv AT gmail DOT com>

# This file is provided to you under the terms of the BSD 2-clause
# license. <http://opensource.org/licenses/BSD-2-Clause>

# Don't whine, send a patch instead. 

# Pull requests are gladly accepted at
# <https://github.com/palopezv/vuze-bin-archlinux>

pkgname=vuze
pkgver=5.3.0.0
pkgrel=1

# _ver=${pkgver//./} 
# Just for reference purposes. This is how it should be done, but 
# the AUR CMS can't handle this. There is no variable expansion!
# It is a pain but reasonable as it can be an attack vector.

_ver=5300 
# This way people can download the file from the AUR page directly. 
# Beats the hell out of me why they would want to do it, but this 
# stops the whining from the peanut gallery.

pkgdesc="The java-based bittorrent kitchen-sink servlet."
provides=("azureus")
arch=('i686' 'x86_64')
url="http://azureus.sf.net/"
license=('GPL')
depends=('java-runtime' 'desktop-file-utils')
makedepends=('unzip')
install="$pkgname.install"
options=(!strip)

optdepends=(
  'xulrunner192: for vuze channels GUI. Long compile ahead.'
  'webkitgtk2: for vuze channels GUI instead of xulrunner192. Crash prone.'
  'vuze-plugin-mldht: Talk DHT to uTorrent, Transmission, etc. Highly recommended.'
  'vuze-plugin-i2p: Use the i2p dark-net.'
  )

noextract=("Vuze_${_ver}${_extra}.jar") 

source=("http://downloads.sourceforge.net/azureus/vuze/Vuze_${_ver}/Vuze_${_ver}_linux.tar.bz2")

sha256sums=('c6c3a38581a17e216d6191573a180325fe5c537003f2fd58925d08d16f570f75')

package() {
  cd "$srcdir/$pkgname"

  # Create target directories

  install -d "$pkgdir/usr/bin"
  install -d "$pkgdir/usr/lib/vuze"
  install -d "$pkgdir/usr/share/vuze"
  install -d "$pkgdir/usr/share/applications"
  install -d "$pkgdir/usr/share/gconf/schemas"
  install -d "$pkgdir/usr/share/pixmaps"
  install -d "$pkgdir/usr/share/licenses/vuze"

  # Install desktop entries

  install -m644 vuze.desktop -t "$pkgdir/usr/share/applications"

  # Add magnet mime-type to desktop file and fix exec invocation.

  sed -i -e 's#\(x-bittorrent\)#\1;x-scheme-handler/magnet;#' \
      -e 's#^\(Exec=vuze \)%f#\1%U#' \
               "$pkgdir/usr/share/applications/vuze.desktop"

  install -pm644 vuze.png -t "$pkgdir/usr/share/pixmaps"
  install -pm644 vuze.torrent.png -t "$pkgdir/usr/share/pixmaps"
  install -pm644 vuze.schemas -t "$pkgdir/usr/share/gconf/schemas"

  # install SWT

  if [[ $CARCH == i686 ]] ; then
    install -pm644 swt/swt32.jar "$pkgdir/usr/lib/vuze/swt.jar"
  elif [[ $CARCH == x86_64 ]] ; then
    install -pm644 swt/swt64.jar "$pkgdir/usr/lib/vuze/swt.jar"
  fi

  # install vuze

  install -m755 vuze -t "$pkgdir/usr/bin"

  # Fix internal directory name

  sed -i 's|#PROGRAM_DIR="/home/username/apps/azureus"|PROGRAM_DIR="/usr/lib/vuze"|' "$pkgdir/usr/bin/vuze"

  # Install main jar.

  install -pm644 Azureus2.jar -t "$pkgdir/usr/lib/vuze"

  # Install system-wide plugins

  cp -a "$srcdir/$pkgname/plugins" "$pkgdir/usr/lib/vuze"

  # install the license

  install -pm644 TOS.txt -t "$pkgdir/usr/share/licenses/vuze"
}

