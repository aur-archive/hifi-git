# Maintainer: Bennett Goble <nivardus@gmail.com>

pkgname=hifi-git
pkgver=r22568.f1f0878
pkgrel=1
pkgdesc="Open, decentralized virtual worlds using sensors to control avatars and dynamically assigned devices as servers."
arch=('x86_64' 'i686')
url="https://github.com/highfidelity/hifi"
license=('Apache')

depends=('intel-tbb'
         'qt5-base'
         'qt5-multimedia'
         'qt5-script'
         'qt5-tools'
         'qt5-svg'
         'openssl'
         'glm'
         'zlib'
         'freeglut'
         'bullet',
         'sdl2',
         'libsoxr',
         'qxmpp-qt5')

makedepends=('git' 
             'cmake')

source=('git://github.com/highfidelity/hifi.git'
        'git://github.com/highfidelity/gverb.git'
        'interface.desktop'
        'interface.png')

sha256sums=('SKIP'
            'SKIP'
            '24c2760ab1f1a96b2de0c3d756ab79a60a6ca610eee310c9e02ec3498a2c6881'
            'ea4df47bc852ce7a3558f830383320b6bd2847b6c8d5ef77def58430905bf013')

_gitname="hifi"

pkgver() {
  cd "$srcdir/$_gitname"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  ln -s "$srcdir/gverb/include" "$srcdir/$_gitname/interface/external/gverb/include"
  ln -s "$srcdir/gverb/src" "$srcdir/$_gitname/interface/external/gverb/src"
  ln -s "$srcdir/gverb/include" "$srcdir/$_gitname/libraries/audio-client/external/gverb/include"
  ln -s "$srcdir/gverb/src" "$srcdir/$_gitname/libraries/audio-client/external/gverb/src"
  cd $srcdir && mkdir build && cd build
  cmake -DCMAKE_INSTALL_PREFIX:PATH="$pkgdir/opt/interface" -DCMAKE_BUILD_TYPE:STRING=Release "../$_gitname"
  make
}

package() {
  install -Dm644 interface.png "$pkgdir/usr/share/pixmaps/interface.png"
  install -Dm644 interface.desktop "$pkgdir/usr/share/applications/interface.desktop"

  mkdir "$pkgdir/usr/bin"
  cd "$srcdir/build"

  # TODO: create patched CMAKE with make install target

  install -Dm755 assignment-client/assignment-client "$pkgdir/opt/hifi/assignment-client"
  ln -s "$pkgdir/opt/hifi/assignment-client" "$pkgdir/usr/bin/assignment-client"

  install -Dm755 domain-server/domain-server "$pkgdir/opt/hifi/domain-server/domain-server"
  cp -R domain-server/resources "$pkgdir/opt/hifi/domain-server/"
  ln -s "$pkgdir/opt/hifi/domain-server/domain-server" "$pkgdir/usr/bin/domain-server"

  install -Dm755 ice-server/ice-server "$pkgdir/opt/hifi/ice-server"
  ln -s "$pkgdir/opt/hifi/ice-server" "$pkgdir/usr/bin/ice-server"

  install -Dm755 interface/interface "$pkgdir/opt/hifi/interface/interface"
  cp -R interface/resources "$pkgdir/opt/hifi/interface/"
  ln -s "$pkgdir/opt/hifi/interface/interface" "$pkgdir/usr/bin/interface"

  install -Dm755 tools/bitstream2json/bitstream2json "$pkgdir/opt/hifi/tools/bitstream2json"
  ln -s "$pkgdir/opt/hifi/tools/bitstream2json" "$pkgdir/usr/bin/bitstream2json"

  install -Dm755 tools/json2bitstream/json2bitstream "$pkgdir/opt/hifi/tools/json2bitstream"
  ln -s "$pkgdir/opt/hifi/tools/json2bitstream" "$pkgdir/usr/bin/json2bitstream"

  install -Dm755 tools/mtc/mtc "$pkgdir/opt/hifi/tools/mtc"
  ln -s "$pkgdir/opt/hifi/tools/mtc" "$pkgdir/usr/bin/mtc"

  install -Dm755 tools/scribe/scribe "$pkgdir/opt/hifi/tools/scribe"
  ln -s "$pkgdir/opt/hifi/tools/scribe" "$pkgdir/usr/bin/scribe"
}
