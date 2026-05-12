pkgname=gnome-shell-extension-media-controls
pkgver=2.4.5
pkgrel=1
pkgdesc="A media indicator for the GNOME shell"
arch=('any')
url="https://github.com/sakithb/media-controls"
license=('MIT')
depends=('gnome-shell' 'glib2')
makedepends=('pnpm' 'nodejs')
provides=("${pkgname}")
options=(!strip !emptydirs)

_uuid="mediacontrols@cliffniff.github.com"
source=("${pkgname}-${pkgver}.tar.gz::https://github.com/sakithb/media-controls/archive/refs/tags/v${pkgver}.tar.gz")
sha256sums=('SKIP')

build() {
    cd "media-controls-$pkgver"

    pnpm install
    pnpm run build
}

package() {
    cd "media-controls-$pkgver"

    _destdir="$pkgdir/usr/share/gnome-shell/extensions/$_uuid"
    install -dm755 "$_destdir"

    _zipfile=$(find . -maxdepth 1 -name "*.zip" -print -quit)

    if [[ -n "$_zipfile" ]]; then
        bsdtar -xf "$_zipfile" -C "$_destdir/"
    else
        if [[ -d "dist" ]]; then
            cp -a dist/* "$_destdir/"
        elif [[ -d "extension" ]]; then
            cp -a extension/* "$_destdir/"
        else
            cp -a . "$_destdir/"
            rm -rf "$_destdir"/{node_modules,src,tests,.git,.github,*.tar.gz,*.zip}
        fi

        for item in metadata.json schemas stylesheet.css; do
            if [[ -e "$item" ]] && [[ ! -e "$_destdir/$item" ]]; then
                cp -a "$item" "$_destdir/"
            fi
        done
    fi

    if [[ -d "$_destdir/schemas" ]]; then
        glib-compile-schemas "$_destdir/schemas/"
    fi

    if [[ -f "$_destdir/metadata.json" ]]; then
        chmod 644 "$_destdir/metadata.json"
    fi
}
