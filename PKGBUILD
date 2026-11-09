# Maintainer: Kay <s0990006@gmail.com>
pkgname=nautilus-image-compressor-app
pkgver=0.1.0
pkgrel=1
pkgdesc="Standalone app to compress images — built with Rust and GTK4"
arch=('x86_64' 'aarch64')
url="https://github.com/Kayxue/nautilus-image-compressor-app"
license=('MIT')
depends=(
    'gtk4'
    'libadwaita'  # relm4 "libadwaita" feature
    'dav1d'       # image "avif-native" feature (dav1d-sys)
    'libx11'      # gdk4-x11 / xlib
    'wayland'     # gdk4-wayland
)
makedepends=(
    'rust'
    'cargo'
    'clang'       # required by bindgen (dav1d-sys)
)
options=('!lto')
source=("$pkgname-$pkgver.tar.gz::$url/archive/v$pkgver.tar.gz")
sha256sums=('SKIP')  # Replace with actual checksum once a tagged release exists

prepare() {
    cd "$srcdir/$pkgname-$pkgver"

    # Pre-fetch Cargo dependencies into the offline cache so the build step
    # does not need network access (required for Arch packaging guidelines).
    export CARGO_HOME="$srcdir/cargo-home"
    cargo fetch --locked --target "$(rustc -vV | sed -n 's|host: ||p')"
}

build() {
    cd "$srcdir/$pkgname-$pkgver"

    export CARGO_HOME="$srcdir/cargo-home"
    export RUSTFLAGS="-C opt-level=2 -C link-arg=-Wl,-z,relro,-z,now"

    cargo build --frozen --release
}

check() {
    cd "$srcdir/$pkgname-$pkgver"

    export CARGO_HOME="$srcdir/cargo-home"
    cargo test --frozen --release
}

package() {
    cd "$srcdir/$pkgname-$pkgver"

    # Install the compiled binary
    install -Dm755 "target/release/nautilus_image_compressor" \
        "$pkgdir/usr/bin/nautilus-image-compressor"

    # Install the license
    install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

    # Install the README as documentation
    install -Dm644 README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
}
