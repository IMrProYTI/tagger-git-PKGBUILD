# Maintainer: Your Name <you@example.com>
# Upstream: https://github.com/NickvisionApps/Tagger

pkgname=tagger
pkgver=2024.6.0
pkgrel=2
pkgdesc='Easy-to-use music tag (metadata) editor'
arch=('x86_64')
url='https://github.com/NickvisionApps/Tagger'
license=('MIT')
depends=(
  'chromaprint'
  'dotnet-runtime-8.0'
  'glib2'
  'gtk4'
  'libadwaita'
  'webp-pixbuf-loader'
  'adwaita-icon-theme'
)
makedepends=(
  'blueprint-compiler'
  'dotnet-sdk-8.0'
  'gettext'
)
source=(
  "https://github.com/NickvisionApps/Tagger/archive/${pkgver}-1.tar.gz"
)
sha256sums=(
  'c0565ce8be05e72f8ef05ed8de5c26794d59864e87d475a17ce73ab59bfbcfce'
)

build() {
  cd "${srcdir}/Tagger-${pkgver}-1"

  export DOTNET_CLI_TELEMETRY_OPTOUT=1
  export DOTNET_NOLOGO=1
  export NUGET_PACKAGES="${srcdir}/nuget-packages"

  dotnet restore \
    NickvisionTagger.GNOME/NickvisionTagger.GNOME.csproj

  dotnet publish \
    NickvisionTagger.GNOME/NickvisionTagger.GNOME.csproj \
    --configuration Release \
    --no-restore \
    --output "${srcdir}/publish"
}

package() {
  cd "${srcdir}/Tagger-${pkgver}-1"

  # Install the published application.
  install -dm755 "${pkgdir}/usr/lib/tagger"
  cp -a "${srcdir}/publish/." "${pkgdir}/usr/lib/tagger/"

  # Launcher.
  install -dm755 "${pkgdir}/usr/bin"
  ln -s /usr/lib/tagger/NickvisionTagger.GNOME \
    "${pkgdir}/usr/bin/tagger"

  # Desktop integration.
  install -Dm644 /dev/stdin \
    "${pkgdir}/usr/share/applications/org.nickvision.tagger.desktop" <<'EOF'
[Desktop Entry]
Name=Tagger
Comment=Tag your music
Exec=tagger %F
Icon=org.nickvision.tagger
Terminal=false
Type=Application
Categories=AudioVideo;Audio;AudioVideoEditing;
MimeType=audio/mpeg;audio/ogg;audio/flac;audio/x-flac;audio/x-wav;audio/x-ms-wma;
StartupNotify=true
EOF

  # Application icon.
  install -dm755 \
    "${pkgdir}/usr/share/icons/hicolor/scalable/apps"

  if [[ -f "${srcdir}/publish/org.nickvision.tagger.svg" ]]; then
    install -Dm644 \
      "${srcdir}/publish/org.nickvision.tagger.svg" \
      "${pkgdir}/usr/share/icons/hicolor/scalable/apps/org.nickvision.tagger.svg"
  fi

  # License.
  install -Dm644 \
    LICENSE \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
