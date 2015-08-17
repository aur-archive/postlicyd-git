# Maintainer: Nikolay Amiantov <nikoamia@gmail.com>

# pkgbase=pfixtools-git
# pkgname=(pfix-srsd-git postlicyd-git)
pkgname=postlicyd-git
pkgver=0.8.r106.g759538c
pkgrel=1
# pkgdesc="Collection of postfix-related tools."
pkgdesc="This is a postfix policy daemon. It includes greylisting, rbl lookups, counters, SMTP session tracking, in a way that allow very fine grained schemes."
# makedepends=(libsrs2 libev pcre unbound libsrs2 tokyocabinet
#     xmlto docbook-xsl asciidoc)
depends=(libev pcre unbound libsrs2 gperf tokyocabinet)
makedepends=(xmlto docbook-xsl asciidoc)
provides=(postlicyd)
conflicts=(postlicyd)

arch=(i686 x86_64)
url="https://github.com/Fruneau/pfixtools"
license=(BSD)
source=('git+https://github.com/Fruneau/pfixtools.git'
    'git+https://github.com/Fruneau/libcommon.git'
    'use-usr-bin.patch')
sha1sums=('SKIP'
    'SKIP'
    '9fa564ee0659bdcbe1e40908c8738e02bd8caffe')

pkgver() {
  cd "$srcdir/pfixtools"

  git describe --long | sed -r 's/^pfixtools-//;s/([^-]*-g)/r\1/;s/-/./g'
}

prepare() {
  cd "$srcdir/pfixtools"

  git submodule init
  git config submodule.common.url "$srcdir/libcommon"
  git submodule update

  cd common
  patch -p1 -i "$srcdir/use-usr-bin.patch"
}

build() {
  cd "$srcdir/pfixtools"

  cd common
  make
  cd ..

  #make prefix=/usr all doc
  cd postlicyd
  make prefix=/usr all doc
}

# check() {
#   cd "$srcdir/pfixtools/tests"
# 
#   for i in ./tst-*; do
#     [ -x "$i" ] && "$i"
#   done
# }

# package_pfix-srsd-git() {
#   pkgdesc="This daemon brings SRS to postfix using its tcp_table or socketmap_table mechanisms."
#   depends=(libsrs2 libev)
#   provides=(pfix-srsd)
#   conflicts=(pfix-srsd)

#   cd "$srcdir/pfixtools/pfix-srsd"
  
#   mkdir -p "$pkgdir/usr/bin"
#   mkdir -p "$pkgdir/usr/share/doc/pfixtools"
#   make prefix=/usr DESTDIR="$pkgdir" install install-doc
#   install -D -m644 ../LICENSE "$pkgdir/usr/share/licenses/$pkgname"
# }

# package_postlicyd-git() {
package() {
  cd "$srcdir/pfixtools/postlicyd"
  mkdir -p "$pkgdir/usr/bin"
  mkdir -p "$pkgdir/usr/share/doc/pfixtools"
  make prefix=/usr DESTDIR="$pkgdir" install install-doc

  cd "$srcdir/pfixtools"
  make prefix=/usr DESTDIR="$pkgdir" install-postlicyd-conf \
      install-postlicyd-tools
  install -D -m644 LICENSE "$pkgdir/usr/share/licenses/$pkgname"
}
