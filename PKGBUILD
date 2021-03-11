# Maintainer    : Vincent Grande <shoober420@gmail.com>
# Contributor   : Adrian Perez de Castro <aperez@igalia.com>
# Contributor   : Eric Vidal <eric@obarun.org>
# Contributor   : Jean-Michel T.Dydak <jean-michel@obarun.org>
# Contributor   : Brett Cornwall <ainola@archlinux.org>
# Contributor   : Omar Pakker

pkgname=wlroots-eglstreams-nosystemd-git
pkgver=0.12.0.r377.gca0073de
pkgrel=1
pkgdesc='Modular Wayland compositor library'
url="https://github.com/danvd/wlroots-eglstreams"
arch=('x86_64')
license=('MIT')
source=("git+https://github.com/danvd/wlroots-eglstreams")
sha512sums=('SKIP')

depends=(
    'libinput'
    'libxkbcommon'
    'opengl-driver'
    'pixman'
    'xcb-util-errors'
    'xcb-util-image'
    'xcb-util-wm'
    'xcb-util-renderutil'
    'xorg-xwayland'
)
makedepends=(
    'meson'
    'ninja'
    'wayland-protocols'
)
provides=('libwlroots.so' 'wlroots' 'wlroots-git')
conflicts=('wlroots' 'wlroots-git')

prepare() {
    mv wlroots-eglstreams wlroots
}

pkgver () {
	cd wlroots
	(
		set -o pipefail
		git describe --long 2>/dev/null | sed 's/\([^-]*-g\)/r\1/;s/-/./g' ||
		printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
	)
}

_path=(
    --prefix=/usr
)

_flags=(
    --buildtype=plain
    -Dlogind=enabled
    -Dlogind-provider=elogind
    -Dxcb-errors=enabled
    -Dxcb-icccm=enabled
    -Dxcb-xkb=enabled
    -Dxwayland=enabled
    -Dx11-backend=enabled
    -Dwerror=false
)

build() {
    meson wlroots build "${_path[@]}" "${_flags[@]}"
    ninja $NINJAFLAGS -C build
}

package() {
    DESTDIR="$pkgdir" ninja $NINJAFLAGS -C build install
    install -Dm644 "wlroots/LICENSE" -t "$pkgdir/usr/share/licenses/wlroots/"
}
