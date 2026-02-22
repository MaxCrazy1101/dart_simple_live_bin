# Maintainer: Your Name <your.email@example.com>
pkgname=simple-live-app-bin
_pkgname=${pkgname%-bin}
pkgver=1.11.4
pkgrel=1
pkgdesc="简简单单的看直播 (Flutter client for Linux)"
arch=('x86_64')
url="https://github.com/xiaoyaocz/dart_simple_live"
license=('GPL3')
depends=('libayatana-appindicator' 'webkit2gtk-4.1' 'xdg-user-dirs')
provides=("${_pkgname}")
conflicts=("${_pkgname}")
_tag=dev_v1.11.4

source=("https://github.com/MaxCrazy1101/dart_simple_live_bin/releases/download/${_tag}/${_pkgname}-linux.tar.gz")
sha256sums=('INSERT_SHA256_HERE')

package() {
  install -d "${pkgdir}/opt/${_pkgname}"
  install -d "${pkgdir}/usr/bin"
  install -d "${pkgdir}/usr/share/applications"
  install -d "${pkgdir}/usr/share/icons/hicolor/512x512/apps"
  
  # 将下载的压缩包内产物复制到 /opt
  cp -a "${srcdir}/data" "${pkgdir}/opt/${_pkgname}/"
  cp -a "${srcdir}/lib" "${pkgdir}/opt/${_pkgname}/"
  install -Dm755 "${srcdir}/simple_live_app" "${pkgdir}/opt/${_pkgname}/${_pkgname}"
  
  # 创建软链接到 /usr/bin 以便全局调用
  ln -sf "/opt/${_pkgname}/${_pkgname}" "${pkgdir}/usr/bin/${_pkgname}"

  # 1. 尝试像 kazumi 一样从 flutter_assets 中提取图标 (注意：需确认原仓库是否有这个图片)
  # 如果上游图标路径不同，请修改此处的相对路径
  local _icon_src="${srcdir}/data/flutter_assets/assets/images/logo.png"
  if [ -f "$_icon_src" ]; then
      install -Dm644 "$_icon_src" "${pkgdir}/usr/share/icons/hicolor/512x512/apps/${_pkgname}.png"
  fi

  # 2. 动态生成 .desktop 桌面快捷方式文件，不需要额外下载
  cat > "${pkgdir}/usr/share/applications/${_pkgname}.desktop" << EOF
[Desktop Entry]
Version=1.0
Name=Simple Live
Comment=Watch live streams easily.
Comment[zh_CN]=简简单单的看直播
Exec=${_pkgname} %U
StartupWMClass=simple_live_app
Icon=${_pkgname}
Terminal=false
Type=Application
Categories=AudioVideo;Player;Network;
StartupNotify=true
EOF
}