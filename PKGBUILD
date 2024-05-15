# Maintainer: Ivan Gabaldon <aur[at]inetol.net>
# Contributor: S Stewart <tda@null.net>
# Contributor: Cranky Supertoon <crankysupertoon@gmail.com>

pkgname=gdlauncher-bin
pkgver=2.0.9
pkgrel=1
pkgdesc='A simple, yet powerful Minecraft custom launcher with a strong focus on the user experience (binary release)'
arch=('x86_64')
url=https://gdlauncher.com
license=('BUSL-1.1')
provides=("${pkgname//-bin}")
conflicts=("${pkgname//-bin}")
source=("$pkgname-$pkgver.AppImage::https://cdn-raw.gdl.gg/launcher/GDLauncher__${pkgver}__linux__x64.AppImage"
        "LICENSE::https://raw.githubusercontent.com/gorilla-devs/GDLauncher-Carbon/develop/LICENSE")
b2sums=('c204a57f936b0aacf3d63f5df47b77e33c29ee7ad19d9a92a14024eee08187fe701356f3ded6445f09da9ed2f69cb1ff32f5f03373fcd87b5ec886bae3f55738'
        '93aed8a6736b73bc8ce08847d9dc895c0e14310125811ea43fbfe9977faa186c4d8f8ffcf88a2a17ab8ae34ab82e1ffbe84e1c590456d4bef46e552b5f80cee7')

prepare() {
    chmod +x "$pkgname-$pkgver.AppImage" && "./$pkgname-$pkgver.AppImage" --appimage-extract

    # Convert
    cd squashfs-root/

    mv "@gddesktop" "${pkgname//-bin}"

    mv "@gddesktop.desktop" "${pkgname//-bin}.desktop"
    sed -i -e "s|Exec=.*|Exec=/usr/bin/${pkgname//-bin} --dummy-flag %U|" -e "s|Icon=.*|Icon=${pkgname//-bin}|" -e '/X-AppImage-Version=.*/d' "${pkgname//-bin}.desktop"

    mv usr/share/icons/hicolor/0x0/apps/@gddesktop.png "${pkgname//-bin}.png"

    cat ../LICENSE > LICENSE

    rm -rf {usr/,"@gd"*,.DirIcon,AppRun}

    find . -type d -exec chmod 755 {} \;
    find . -type f -exec chmod 644 {} \;
    chmod 755 {chrome-sandbox,chrome_crashpad_handler,"${pkgname//-bin}",*.so,*.so.*}
    chmod 755 resources/{"app.asar.unpacked/node_modules/@sentry/cli-linux-x64/bin/sentry-cli",binaries/core_module,app.asar}
}

package() {
    depends=('alsa-lib'
             'at-spi2-core'
             'cairo'
             'dbus'
             'expat'
             'gcc-libs'
             'glib2'
             'glibc'
             'gtk3'
             'hicolor-icon-theme'
             'libcups'
             'libdrm'
             'libx11'
             'libxcomposite'
             'libxdamage'
             'libxext'
             'libxfixes'
             'libxkbcommon'
             'libxrandr'
             'libxcb'
             'mesa'
             'nspr'
             'nss'
             'pango')

    install -d "$pkgdir/opt/${pkgname//-bin}/"
    cp -a squashfs-root/. "$pkgdir/opt/${pkgname//-bin}/"

    install -d "$pkgdir/usr/bin/"
    ln -s "/opt/${pkgname//-bin}/${pkgname//-bin}" "$pkgdir/usr/bin/${pkgname//-bin}"

    install -d "$pkgdir/usr/share/applications/"
    ln -s "/opt/${pkgname//-bin}/${pkgname//-bin}.desktop" "$pkgdir/usr/share/applications/${pkgname//-bin}.desktop"

    install -d "$pkgdir/usr/share/icons/hicolor/2048x2048/apps/"
    ln -s "/opt/${pkgname//-bin}/${pkgname//-bin}.png" "$pkgdir/usr/share/icons/hicolor/2048x2048/apps/${pkgname//-bin}.png"

    install -Dm644 LICENSE -t "$pkgdir/usr/share/licenses/${pkgname//-bin}/"
}
