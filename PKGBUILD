# Maintainer: Felix Schindler <aur at felixschindler dot net>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Antonio Rojas <arojas@archlinux.org>
# Contributor: Imanol Celaya <ornitorrincos@archlinux-es.org>
# Contributor: Lukas Jirkovsky <l.jirkovsky@gmail.com>
# Contributor: Dan Vratil <progdan@progdansoft.com>
# Contributor: thotypous <matiasΘarchlinux-br·org>
# Contributor: delor <bartekpiech gmail com>

pkgname=qtcreator-clang8
_pkgname=qtcreator
pkgver=4.13.1
_clangver=8.0.1
pkgrel=1
pkgdesc='Lightweight, cross-platform integrated development environment'
arch=(x86_64)
url='https://www.qt.io'
license=(LGPL)
depends=(qt5-tools qt5-quickcontrols qt5-quickcontrols2 qt5-webengine clang8=$_clangver qbs clazy syntax-highlighting yaml-cpp desktop-file-utils)
makedepends=(llvm8 python)
options=(docs)
conflicts=(qtcreator)
provides=("qtcreator=${pkgver}.clang8")
replaces=(qtcreator)
optdepends=('qt5-doc: integrated Qt documentation'
            'qt5-examples: welcome page examples'
            'qt5-translations: for other languages'
            'gdb: debugger'
            'cmake: cmake project support'
            'x11-ssh-askpass: ssh support'
            'git: git support'
            'mercurial: mercurial support'
            'bzr: bazaar support'
            'valgrind: analyze support'
            'perf: performer analyzer')
source=("https://download.qt.io/official_releases/qtcreator/${pkgver%.*}/$pkgver/qt-creator-opensource-src-$pkgver.tar.xz")
sha256sums=('7a401bee0c5518e498ca7fb2d82c6713200def7928a706614dbf8f4d9a112bf9')

prepare() {
  mkdir -p build

  cd qt-creator-opensource-src-$pkgver
  # fix hardcoded libexec path
  sed -e 's|libexec\/qtcreator|lib\/qtcreator|g' -i qtcreator.pri
  sed -e 's|libexec|lib|g' -i src/tools/tools.pro
  # use system qbs
  rm -r src/shared/qbs
}

build() {
  cd build

  qmake LLVM_INSTALL_DIR=/usr QBS_INSTALL_DIR=/usr \
    KSYNTAXHIGHLIGHTING_LIB_DIR=/usr/lib KSYNTAXHIGHLIGHTING_INCLUDE_DIR=/usr/include/KF5/KSyntaxHighlighting \
    CONFIG+=journald QMAKE_CFLAGS_ISYSTEM=-I \
    DEFINES+=QBS_ENABLE_PROJECT_FILE_UPDATES \
    "$srcdir"/qt-creator-opensource-src-$pkgver/qtcreator.pro
  make
  #make docs
}

package() {
  cd build

  make INSTALL_ROOT="$pkgdir/usr/" install
  #make INSTALL_ROOT="$pkgdir/usr/" install_docs

  install -Dm644 "$srcdir"/qt-creator-opensource-src-$pkgver/LICENSE.GPL3-EXCEPT "$pkgdir"/usr/share/licenses/$_pkgname/LICENSE.GPL3-EXCEPT
}
