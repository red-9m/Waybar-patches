# Maintainer: Alexis Rouillard <contact@arouillard.fr>

pkgname=waybar-git
pkgver=r3428.cb2d54a2
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
    'libsigc++'
    'fmt'
    'jack' 'libjack.so'
    'wayland'
    'libdate-tz.so'
    'libspdlog.so'
    'gtk-layer-shell'
    'libupower-glib.so'
    'upower'
    'libevdev'
    'libinput'
    'libpulse'
    'libnl'
    'libappindicator-gtk3'
    'libdbusmenu-gtk3'
    'libmpdclient'
    'libsndio.so'
    'libxkbcommon'
    'libwireplumber'
    'playerctl'
)
makedepends=(
    'git'
    'cmake'
    'catch2'
    'meson'
    'scdoc' # For generating manpages
    'wayland-protocols'
)
backup=(
    etc/xdg/waybar/config
    etc/xdg/waybar/style.css
)
optdepends=(
    'otf-font-awesome: Icons in the default configuration'
)

source=("${pkgname}::git+https://github.com/Alexays/Waybar"
        "mod_custom_ret_type.patch"::"https://github.com/red-9m/Waybar-patches/raw/main/mod_custom_ret_type.patch"
        "mod_net_fix_signal.patch"::"https://github.com/red-9m/Waybar-patches/raw/main/mod_net_fix_signal.patch"
        "mod_wlr_taskbar_btn_hide.patch"::"https://github.com/red-9m/Waybar-patches/raw/main/mod_wlr_taskbar_btn_hide.patch"
        )

sha1sums=('SKIP' 'SKIP' 'SKIP' 'SKIP')

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
    CXX=clang++ CC=clang CC_LD=lld CXX_LD=lld CFLAGS="-march=native -O2 -flto" CXXFLAGS="-march=native -O2 -flto" LDFLAGS="-flto -march=native" meson --prefix=/usr \
          --buildtype=release \
          --auto-features=enabled \
          -Dmpris=disabled \
          -Dlibnl=enabled \
          -Dlibudev=disabled \
          -Dlibevdev=disabled \
          -Dupower_glib=disabled \
          -Ddbusmenu-gtk=disabled \
          -Dmpd=disabled \
          -Dmpd=disabled \
          -Dsndio=disabled \
          -Dlogind=disabled \
          -Dexperimental=false \
          -Db_lto=true \
          -Dcava=disabled \
          -Dtests=disabled \
          "${srcdir}/build"
    ninja -C "${srcdir}/build"
}

package() {
    DESTDIR="$pkgdir" ninja -C "${srcdir}/build" install
    install -Dm644 "${srcdir}/${pkgname}/LICENSE" -t "$pkgdir/usr/share/licenses/$pkgname/"
}
