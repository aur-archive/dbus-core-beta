# $Id: PKGBUILD 156105 2012-04-14 07:39:59Z andyrtr $
# Maintainer: Jan de Groot <jgc@archlinux.org>
# Contributor: Link Dupont <link@subpop.net>
#
#	Custom AUR Package Maintainer: Diogo B. <db_eee.at.hotmail.dot.com>

pkgname=dbus-core-beta
pkgver=1.6.0
pkgrel=1
pkgdesc="Freedesktop.org message bus system."
url="http://www.freedesktop.org/Software/dbus"
arch=(i686 x86_64)
license=('GPL' 'custom')
depends=('expat>=2.0.1' 'coreutils' 'filesystem' 'shadow') # shadow for install scriptlet FS#29341
makedepends=('libx11')
conflicts=('dbus<1.2.3-2' 'dbus-core')
provides=('dbus-core=1.5.12')
optdepends=('systemd: native systemd support')
options=(!libtool)
install=dbus.install
source=(http://dbus.freedesktop.org/releases/dbus/dbus-${pkgver}.tar.gz
        dbus)

md5sums=('16dcae2dd0c76e398381601ac9acdec4'
         '08f93dd19cffd1b45ab05c1fd4efb560')
build() {
  cd "${srcdir}/dbus-${pkgver}"
  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var \
      --libexecdir=/usr/lib/dbus-1.0 --with-dbus-user=81 \
      --with-system-pid-file=/var/run/dbus.pid \
      --enable-inotify --disable-dnotify \
      --disable-verbose-mode --disable-static \
      --disable-tests --disable-asserts \
      --with-systemdsystemunitdir=/usr/lib/systemd/system
  make
}

package(){
  cd "${srcdir}/dbus-${pkgver}"
  make DESTDIR="${pkgdir}" install

  rm -f "${pkgdir}/usr/bin/dbus-launch"
  rm -f "${pkgdir}/usr/share/man/man1/dbus-launch.1"
  rm -rf "${pkgdir}/var/run"

  install -m755 -d "${pkgdir}/etc/rc.d"
  install -m755 "${srcdir}/dbus" "${pkgdir}/etc/rc.d/"

  #Fix configuration file
  sed -i -e 's|<user>81</user>|<user>dbus</user>|' "${pkgdir}/etc/dbus-1/system.conf"

  install -d -m755 "${pkgdir}/usr/share/licenses/dbus-core"
  install -m644 COPYING "${pkgdir}/usr/share/licenses/dbus-core/"
}
