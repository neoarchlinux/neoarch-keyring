pkgname=neoarch-keyring
pkgver=0.1
pkgrel=1
pkgdesc="NeoArch Linux PGP keyring"
arch=('x86_64')
license=('GPL-3.0')
url="https://github.com/neoarchlinux/neoarch-keyring"
source=("$pkgname-$pkgver.tar.gz::$url/archive/refs/tags/v$pkgver.tar.gz")
sha256sums=('SKIP')

build() {
    cd "$pkgname-$pkgver"

    export GNUPGHOME="$srcdir/gnupg"
    mkdir -p "$GNUPGHOME"
    chmod 700 "$GNUPGHOME"

    for key in keyring/*/*.asc; do
        gpg --batch --import "$key"
    done

    cat keyring/*/*.asc > "$srcdir/neoarch.gpg"

    gpg --batch --with-colons --fingerprint \
        | awk -F: '$1=="fpr"{print $10}' \
        > "$srcdir/neoarch-trusted"

    : > "$srcdir/neoarch-revoked"
}

package() {
    install -d "$pkgdir/usr/share/pacman/keyrings"

    install -m644 "$srcdir/neoarch.gpg" \
        "$pkgdir/usr/share/pacman/keyrings/neoarch.gpg"

    install -m644 "$srcdir/neoarch-trusted" \
        "$pkgdir/usr/share/pacman/keyrings/neoarch-trusted"

    install -m644 "$srcdir/neoarch-revoked" \
        "$pkgdir/usr/share/pacman/keyrings/neoarch-revoked"
}