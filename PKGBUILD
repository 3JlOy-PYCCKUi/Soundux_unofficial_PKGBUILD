# Maintainer: Nico <d3sox at protonmail dot com>
pkgname=soundux-git
_pkgname=soundux
pkgver=r1297.6e9351a
pkgrel=1
epoch=1
pkgdesc="A cross-platform soundboard - unstable development version"
arch=('any')
url="https://github.com/Soundux/Soundux"
license=('GPL3')
depends=('pulseaudio' 'webkit2gtk' 'libwnck3' 'libappindicator-gtk3' 'lsb-release')
optdepends=('youtube-dl: downloader integration' 'ffmpeg: downloader integration' 'pipewire: pipewire backend')
makedepends=('git' 'cmake' 'ninja' 'pipewire')
conflicts=('soundux')
provides=('soundux')
source=("${_pkgname}::git+https://github.com/Soundux/Soundux.git"
        "${_pkgname}-json::git+https://github.com/nlohmann/json"
        "${_pkgname}-miniaudio::git+https://github.com/mackron/miniaudio"
        "${_pkgname}-fancypp::git+https://github.com/Curve/fancypp"
        "${_pkgname}-nativefiledialog-extended::git+https://github.com/btzy/nativefiledialog-extended"
        "${_pkgname}-soundux-ui::git+https://github.com/Soundux/soundux-ui/"
        "${_pkgname}-webviewpp::git+https://github.com/Soundux/webviewpp"
        "${_pkgname}-tiny-process-library::git+https://gitlab.com/eidheim/tiny-process-library.git"
        "${_pkgname}-cpp-httplib::git+https://github.com/yhirose/cpp-httplib"
        "${_pkgname}-traypp::git+https://github.com/Soundux/traypp"
        "${_pkgname}-shared-modules::git+https://github.com/flathub/shared-modules"
        "${_pkgname}-semver::git+https://github.com/Neargye/semver"
        "${_pkgname}-lockpp::git+https://github.com/Soundux/lockpp"
        "${_pkgname}-backward-cpp::git+https://github.com/bombela/backward-cpp"
        "${_pkgname}-guardpp::git+https://github.com/Soundux/guardpp"
)
sha256sums=('SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP')

pkgver() {
  cd "${srcdir}/${_pkgname}"

  # Get the version number.
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}
prepare() {
  cd "${srcdir}/${_pkgname}"
  git submodule init

  ## target git to local mirrors
  git config submodule.lib/json.url "${srcdir}/${_pkgname}-json"
  git config submodule.lib/miniaudio.url "${srcdir}/${_pkgname}-miniaudio"
  git config submodule.lib/fancypp.url "${srcdir}/${_pkgname}-fancypp"
  git config submodule.lib/nativefiledialog-extended.url "${srcdir}/${_pkgname}-nativefiledialog-extended"
  git config submodule.src/ui/impl/webview/lib/soundux-ui.url "${srcdir}/${_pkgname}-soundux-ui"
  git config submodule.src/ui/impl/webview/lib/webviewpp.url "${srcdir}/${_pkgname}-webviewpp"
  git config submodule.lib/tiny-process-library.url "${srcdir}/${_pkgname}-tiny-process-library"
  git config submodule.lib/cpp-httplib.url "${srcdir}/${_pkgname}-cpp-httplib"
  git config submodule.lib/traypp.url "${srcdir}/${_pkgname}-traypp"
  git config submodule.deployment/flatpak/shared-modules.url "${srcdir}/${_pkgname}-shared-modules"
  git config submodule.lib/semver.url "${srcdir}/${_pkgname}-semver"
  git config submodule.lib/lockpp.url "${srcdir}/${_pkgname}-lockpp"
  git config submodule.lib/backward-cpp.url "${srcdir}/${_pkgname}-backward-cpp"
  git config submodule.lib/guardpp.url "${srcdir}/${_pkgname}-guardpp"
  git submodule update  
}
build() {
  ## reuse json submodule once again
  cd "${srcdir}/${_pkgname}/src/ui/impl/webview/lib/webviewpp/"
  git config submodule.lib/json.url "${srcdir}/${_pkgname}-json"
  cd "${srcdir}/${_pkgname}"
  git submodule update --init --recursive
  mkdir -p build
  cd build
  cmake -GNinja -DCMAKE_BUILD_TYPE=Release ..
  ninja
}

package() {
  cd "${srcdir}/${_pkgname}/build"
  DESTDIR="$pkgdir/" ninja install
  # install binary symlink
  mkdir -p "${pkgdir}/usr/bin/"
  ln -sf /opt/soundux/soundux "${pkgdir}/usr/bin/soundux"
  
  # install doc
  install -Dm 644 -t "${pkgdir}/usr/share/doc/${pkgname}" "${srcdir}/${_pkgname}/README.md"
  # install license
  install -Dm 644 -t "${pkgdir}/usr/share/licenses/${pkgname}" "${srcdir}/${_pkgname}/LICENSE"
}
