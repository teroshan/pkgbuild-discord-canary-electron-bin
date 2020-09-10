# Maintained by johnnyapol (arch@johnnyapol.me)
# Based off the discord community repo PKGBUILD by Filipe Laíns (FFY00) <lains@archlinux.org>

pkgname=discord_arch_electron
_pkgname=discord
pkgver=0.0.12
pkgrel=2
pkgdesc="Discord (popular voice + video app) using the system provided electron for increased security and performance"
arch=('x86_64')
provides=('discord')
conflicts=('discord')
url='https://discordapp.com'
license=('custom')
depends=('electron')
optdepends=('libpulse: Pulseaudio support'
            'xdg-utils: Open files')
source=("https://dl.discordapp.net/apps/linux/$pkgver/$_pkgname-$pkgver.tar.gz"
        'LICENSE.html::https://discordapp.com/terms'
        'OSS-LICENSES.html::https://discordapp.com/licenses')
sha512sums=('c5009e022cac0b76d39cc125a98b9dd3d7a5827dd7d733c5578237b99b746aeccc1cd253aafa99e2a237bd82ef71ee42011f864059aa5ee62812488dbd82f511'
             SKIP
             SKIP)

prepare() {
  cd Discord

  sed -i "s|Exec=.*|Exec=/usr/bin/$_pkgname|" $_pkgname.desktop
  echo 'Path=/usr/bin' >> $_pkgname.desktop
}

package() {
  # Install the app
  install -d "$pkgdir"/opt/$_pkgname

  # Copy Relevanat data
  cp -r Discord/resources  "$pkgdir"/opt/$_pkgname/
  cp    Discord/discord.png "$pkgdir"/opt/$_pkgname/
  cp 	Discord/discord.desktop "$pkgdir"/opt/$_pkgname/

  # Create starter script for discord
  echo "#!/bin/sh" >> "$pkgdir"/opt/$_pkgname/$_pkgname
  echo "electron /opt/$_pkgname/resources/app.asar \$@" >> "$pkgdir"/opt/$_pkgname/$_pkgname

  # Set Permissions, symlinks
  chmod 755 "$pkgdir"/opt/$_pkgname/$_pkgname

  install -d "$pkgdir"/usr/{bin,share/{pixmaps,applications}}
  install -d "$pkgdir"/usr/lib/electron
  install -d "$pkgdir"/usr/lib/electron/resources

  ln -s /opt/$_pkgname/$_pkgname "$pkgdir"/usr/bin/$_pkgname
  ln -sf /opt/$_pkgname/discord.png "$pkgdir"/usr/share/pixmaps/$_pkgname.png
  ln -sf /opt/$_pkgname/$_pkgname.desktop "$pkgdir"/usr/share/applications/$_pkgname.desktop

  # HACKS FOR SYSTEM ELECTRON
  ln -s /opt/$_pkgname/resources/build_info.json "$pkgdir"/usr/lib/electron/resources/
  ln -s /opt/$_pkgname/discord.png        "$pkgdir"/usr/lib/electron

  # Licenses
  install -Dm 644 LICENSE.html "$pkgdir"/usr/share/licenses/$pkgname/LICENSE.html
  install -Dm 644 OSS-LICENSES.html "$pkgdir"/usr/share/licenses/$pkgname/OSS-LICENSES.html
}
