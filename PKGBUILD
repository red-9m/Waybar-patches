# Maintainer: Alexis Rouillard <contact@arouillard.fr>

pkgname=waybar-git
pkgver=r2134.5da45ec
pkgrel=1
pkgdesc='Highly customizable Wayland bar for Sway and Wlroots based compositors (GIT)'
arch=('x86_64')
url='https://github.com/Alexays/Waybar/'
license=('MIT')
provides=('waybar')
conflicts=('waybar')
depends=(
    'gtkmm3'
    'libjsoncpp.so'
    'libinput'
    'libsigc++'
    'fmt'
    'wayland'
    'chrono-date'
    'libspdlog.so'
    'gtk-layer-shell'
    'libpulse'
    'libnl'
    'libappindicator-gtk3'
    'libdbusmenu-gtk3'
    'libmpdclient'
)
makedepends=(
    'git'
    'cmake'
    'meson'
    'scdoc' # For generating manpages
    'wayland-protocols'
)
optdepends=(
    'otf-font-awesome: Icons in the default configuration'
)

source=("${pkgname}::git+https://github.com/Alexays/Waybar"
	"mod_custom_ret_type.patch"::"https://github.com/red-9m/Waybar-patches/raw/main/mod_custom_ret_type.patch"
	"mod_net_fix_signal.patch"::"https://github.com/red-9m/Waybar-patches/raw/main/mod_net_fix_signal.patch"
	)

sha1sums=('SKIP' 'SKIP' 'SKIP')

pkgver() {
    cd "${srcdir}/${pkgname}"
    printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
    cd "${srcdir}/${pkgname}"
    local src
    for src in "${source[@]}"; do
      src="${src%%::*}"
      src="${src##*/}"
      [[ "$src" =~ .*(patch|diff)$ ]] || continue
      echo -e "  ${BLUE}-> ${ALL_OFF}${BOLD}Applying patch: ${ALL_OFF}$src"
      patch -Np1 < "../$src"
    done
}

build() {
    cd "${srcdir}/${pkgname}"
    rm -rf "${srcdir}/build"
    meson --prefix=/usr "${srcdir}/build"
    ninja -C "${srcdir}/build"
}

package() {
    DESTDIR="$pkgdir" ninja -C "${srcdir}/build" install
}
