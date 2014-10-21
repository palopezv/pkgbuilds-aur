# Original maintainer: td123 <gostrc AT gmail DOT com>
# Previous maintainer: phoenixlzx < phoenixlzx AT phoenixsec DOT org >

# This file is provided to you under the terms of the BSD 2-clause
# license. <http://opensource.org/licenses/BSD-2-Clause>

pkgname=vuze
pkgver=5.4.0.0
pkgrel=5
_ver=${pkgver//./} 
_extra="" # sometimes Azureus devs add letters at the end.
pkgdesc="The java-based bittorrent kitchen-sink servlet."
provides=("azureus")
arch=('i686' 'x86_64')
url="http://azureus.sourceforge.net/"
license=('GPL')
depends=('java-runtime' 'desktop-file-utils')
makedepends=('unzip')
install="$pkgname.install"
options=(!strip)
optdepends=(
  'xulrunner192:      for vuze channels GUI. Long compile from AUR.'
  'webkitgtk2:        for vuze channels GUI instead of xulrunner192. Very crash prone.'
  'vuze-plugin-mldht: Talk DHT to uTorrent, Transmission, etc. Highly recommended.'
  'vuze-i2phelper:   I2P support. Install with plugin manager.'
  )
noextract=("Vuze_${_ver}${_extra}.jar") 
source=("http://downloads.sourceforge.net/azureus/vuze/Vuze_${_ver}/Vuze_${_ver}_linux.tar.bz2")
sha256sums=('c09a7926f9bcd1ebb22f9aba09c48980bf43ecc331ca0aa3b665356c18282c3d')

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
  install -m644 vuze.desktop      -t "$pkgdir/usr/share/applications"

  # Install icons
  install -pm644 vuze.png         -t "$pkgdir/usr/share/pixmaps"
  install -pm644 vuze.torrent.png -t "$pkgdir/usr/share/pixmaps"
  install -pm644 vuze.schemas     -t "$pkgdir/usr/share/gconf/schemas"

  # Add magnet mime-type to desktop file and fix exec invocation.
  sed -i \
   -e 's#\(x-bittorrent\)#\1;x-scheme-handler/magnet;#' \
   -e 's#^\(Exec=vuze \)%f#\1%U#' \
     "$pkgdir/usr/share/applications/vuze.desktop"
  
  # install SWT selecting build architecture.
  if [[ $CARCH == i686 ]] ; then
    install -pm644 swt/swt32.jar "$pkgdir/usr/lib/vuze/swt.jar"
  elif [[ $CARCH == x86_64 ]] ; then
    install -pm644 swt/swt64.jar "$pkgdir/usr/lib/vuze/swt.jar"
  fi

  # install vuze
  install -m755 vuze -t "$pkgdir/usr/bin"

  # Fix startup script: adjust internal directory name and make it work 
  # with the new java-environment-common multiversions setup.
  sed -i \
    -e 's|#PROGRAM_DIR="/home/username/apps/azureus"|PROGRAM_DIR="/usr/lib/vuze"|' \
    -e 's|JAVA_PROGRAM_DIR=""|JAVA_PROGRAM_DIR="$JAVA_HOME/bin/"|' \
    "$pkgdir/usr/bin/vuze"

  # Install main jar.
  install -pm644 Azureus2.jar -t "$pkgdir/usr/lib/vuze"

  # Install system-wide plugins
  cp -a "$srcdir/$pkgname/plugins" "$pkgdir/usr/lib/vuze"

  # install license
  install -pm644 TOS.txt -t "$pkgdir/usr/share/licenses/vuze"
}

