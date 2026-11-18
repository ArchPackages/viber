# Maintainer: Miroslav Pavleski <miroslav@pavleski.net>
# Contributor: Jason Go <jasongo@jasongo.net>
# Contributor: Sandor Nagy <sandor[dot]nagy[at]kdemail[dot]net>
# Contributor: Mladen Pejaković <pejakm[at]autistici[dot]org>
# Contributor: Morealaz <morealaz@gmail.com>
# Contributor: Lev Lybin <lev.lybin@gmail.com>
# Contributor: Özgür Sarıer <ozgursarier1011601115[at]gmail[dot]com>

pkgname=viber
pkgver=29.4.0.7
pkgrel=2
pkgdesc="Free and secure calls and messages to anyone, anywhere, on any device and network, in any country!"
arch=('x86_64')
url='https://www.viber.com'
license=('LicenseRef-Viber-TOS')
# Viber bundles Qt 6, ICU, OpenSSL, curl and FFmpeg under /opt/viber/lib.
# The dependencies below are the system libraries that the bundled ELF
# objects still link against.
depends=(
  'alsa-lib'
  'brotli'
  'bzip2'
  'dbus'
  'expat'
  'fontconfig'
  'freetype2'
  'gcc-libs'
  'glib2'
  'glibc'
  'krb5'
  'libdrm'
  'libglvnd'
  'libice'
  'libpng'
  'libpulse'
  'libsm'
  'libva'
  'libx11'
  'libxcb'
  'libxcomposite'
  'libxdamage'
  'libxext'
  'libxfixes'
  'libxkbcommon'
  'libxkbcommon-x11'
  'libxkbfile'
  'libxrandr'
  'libxss'
  'libxtst'
  'mesa'
  'nspr'
  'nss'
  'systemd-libs'
  'wayland'
  'xcb-util-image'
  'xcb-util-keysyms'
  'xcb-util-renderutil'
  'xcb-util-wm'
  'zlib'
  'zstd'
)
optdepends=(
  'gtk3: GTK3 platform theme and native file dialogs'
  'xdg-desktop-portal: Open/save dialogs through the desktop portal'
  'xdg-desktop-portal-gtk: GTK file chooser backend for the portal'
  'xdg-utils: Open external links and attachments'
  'qt6-base: Provides qtpaths for xdg-mime file associations'
  'firefox: Open external links'
  'chromium: Open external links'
  'epiphany: Open external links'
  'pavucontrol: Manage audio devices and volume'
  'pipewire-pulse: PulseAudio server provided by PipeWire'
  'nvidia-utils: VDPAU backend for hardware video decoding'
  'gnome-shell-extension-appindicator: Tray icon support in GNOME'
)
conflicts=('viber')
options=('!debug' '!strip')
umask 022
source=(
  "viber-$pkgver.deb::https://download.cdn.viber.com/cdn/desktop/Linux/viber.deb"
  'viber.sh.in'
  'https://www.viber.com/app/uploads/Viber-Terms-of-Service-EN-March-2026.pdf'
)
b2sums=('2cb11c91f1bb15ea2708ca458b39f896876f3d8224590a6907a72f82edd5ffa3d4c51afe64383511d604f085caab5fa5bc0404cf42e7b6e4d828060358911db3'
        '4573489bb58102dab38ead69feb8ee2ab8d51f1d4a423a4b44f638877c0fc594f77e8384e4a85426375757e0f34135db6621df230fb0bdbb4bc5c365670b3788'
        '7a935c23873efaf903fa686c74a5a9ade31c29c8dee54f4d8f98742ffd3624ff203bd681b7ae525b2118e74680edf471b9ac14ac6cd2b2f07b13dc5960600c92')

prepare() {
  bsdtar -xf data.tar.xz -C ./

  # The deb ships 0664/0775 modes; normalise them for a system package.
  find opt usr -type d -exec chmod 755 {} +
  find opt usr -type f -perm /111 -exec chmod 755 {} +
  find opt usr -type f ! -perm /111 -exec chmod 644 {} +

  # Use the wrapper in /usr/bin and drop the deb-specific working directory.
  sed -i -e 's|Exec=/opt/viber/Viber|Exec=/usr/bin/viber|' \
         -e '/^Path=/d' \
         ./usr/share/applications/viber.desktop
}

package() {
  # Install binary wrapper
  install -Dm755 viber.sh.in "$pkgdir/usr/bin/viber"

  # Copy all deb files
  cp -dr --no-preserve=ownership {opt,usr} "$pkgdir"

  # The desktop file references the lowercase icon name; icon lookup is
  # case-sensitive on Linux.
  cp "$pkgdir/usr/share/icons/hicolor/scalable/apps/Viber.svg" \
     "$pkgdir/usr/share/icons/hicolor/scalable/apps/viber.svg"

  # Copy updated license
  install -Dm644 -t "$pkgdir/usr/share/licenses/viber/" \
    Viber-Terms-of-Service-EN-March-2026.pdf

  # cp honours the caller's umask; make sure the prebuilt runtime is readable
  # and executable for every user regardless of how makepkg was invoked.
  find "$pkgdir"/opt "$pkgdir"/usr -type d -exec chmod 755 {} +
  find "$pkgdir"/opt "$pkgdir"/usr -type f -perm /111 -exec chmod 755 {} +
  find "$pkgdir"/opt "$pkgdir"/usr -type f ! -perm /111 -exec chmod 644 {} +
}
