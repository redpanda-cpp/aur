_pkgname=RedPanda-CPP
pkgname=${_pkgname,,}
pkgver=3.2
pkgrel=2
pkgdesc='A fast, lightweight, open source, and cross platform C/C++/GNU Assembly IDE'
arch=('i686' 'pentium4' 'x86_64' 'arm' 'armv6h' 'armv7h' 'aarch64' 'riscv64' 'loong64')
url="https://github.com/royqh1979/$_pkgname"
license=('GPL3')
depends=(qt6-base qt6-svg gcc gdb astyle)
makedepends=(qt6-tools imagemagick librsvg)
optdepends=(
    'clang: C/C++ compiler (alternative)'
)
optdepends_x86_64=(
    'mingw-w64-gcc: Windows C/C++ compiler'
    'wine: run Windows executable'
    'aarch64-linux-gnu-gcc: AArch64 C/C++ compiler'
    'qemu-user-static: run AArch64 executable'
    'qemu-user-static-binfmt: run AArch64 executable'
)
source=(
    "$_pkgname-$pkgver.tar.gz::https://github.com/royqh1979/$_pkgname/archive/refs/tags/v$pkgver.tar.gz"
    'compiler_hint.lua'
    '0001-fix-crash-when-debug.patch'
    '0002-fix-qt-6.9-build.patch'
)
sha256sums=('dc2267d793a553373a78c8a4624e908fab9bc1624c3abd12da5a1278d58219f7'
            '6f887af50757c2ec7d57806b78119e0271c424db2119fbc62d111c2122c06b0c'
            '632d966f60999ba121fe37c297ef3f02128bb53f845d7630e0f3839df232bd4d'
            '6d55bf146ebce9b62b713f3a90e5f698c336daa4561cfba826bea5e95184725c')

prepare() {
    cd "$srcdir/$_pkgname-$pkgver"
    sed -i '/CONFIG += ENABLE_LUA_ADDON/ { s/^#\s*// }' "RedPandaIDE/RedPandaIDE.pro"
    patch -Np1 <"$srcdir/0001-fix-crash-when-debug.patch"
    patch -Np1 <"$srcdir/0002-fix-qt-6.9-build.patch"
}

build() {
    mkdir redpanda-build
    cd redpanda-build
    qmake6 \
        PREFIX='/usr' \
        LIBEXECDIR='/usr/lib' \
        "$srcdir/$_pkgname-$pkgver/Red_Panda_CPP.pro"
    make
}

package() {
    local _libexecdir="$pkgdir/usr/lib/RedPandaCPP"
    install -Dm644 "compiler_hint.lua" "$_libexecdir/compiler_hint.lua"

    cd redpanda-build
    make INSTALL_ROOT="$pkgdir" install

    for size in 16 22 24 32 36 48 64 72 96 128 192 256 512; do
        mkdir -p "$pkgdir/usr/share/icons/hicolor/${size}x${size}/apps"
        magick convert \
            -background none \
            "$pkgdir/usr/share/icons/hicolor/scalable/apps/redpandaide.svg" \
            -resize ${size}x${size} \
            "$pkgdir/usr/share/icons/hicolor/${size}x${size}/apps/redpandaide.png"
    done
}
