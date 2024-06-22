# Maintainer: Ivan Gabaldon <aur[at]inetol.net>
# Contributor: S Stewart <tda@null.net>
# Contributor: Cranky Supertoon <crankysupertoon@gmail.com>

pkgname=gdlauncher-bin
pkgver=2.0.20
pkgrel=1
pkgdesc='A simple, yet powerful Minecraft custom launcher with a strong focus on the user experience (binary release)'
arch=('x86_64')
url=https://gdlauncher.com
license=('BUSL-1.1')
provides=("${pkgname//-bin}")
conflicts=("${pkgname//-bin}")
source=("$pkgname-$pkgver.AppImage::https://cdn-raw.gdl.gg/launcher/GDLauncher__${pkgver}__linux__x64.AppImage"
        "LICENSE::https://raw.githubusercontent.com/gorilla-devs/GDLauncher-Carbon/develop/LICENSE")
b2sums=('773af34aeeb2f7a36ee4356661529b218d10ded1e37ffc9907f274e06d9e99446f1417b0becc3d1ef140728824896e291cbb83f63f5c35db685c97f55c059755'
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
    chmod 755 resources/{binaries/core_module,app.asar}
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
