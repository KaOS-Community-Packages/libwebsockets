license=('MIT')
arch=('x86_64')
pkgname=libwebsockets
pkgver=4.2.0
pkgrel=1
pkgdesc="C library for websocket clients and servers"
depends=('glibc' 'openssl' 'libuv' 'libev' 'zlib' 'glib2' 'libcap')
makedepends=('cmake')
url="https://libwebsockets.org"
source=("$pkgname-$pkgver.tar.gz::https://github.com/warmcat/libwebsockets/archive/v$pkgver.tar.gz")
sha512sums=('e1fb5b204a030ded8dfe2a75c66ec8d1a2e6a67e82c7709fe3c4277e0ccb5fb40c18db04e73c640d07ef4516aa266ae8b102f802b2a41b80980260cb6921f369')

prepare() {
  cd "$pkgname-$pkgver"
  # -Werror, not even once
  sed '/Werror/d' -i CMakeLists.txt
}

build() {
cd "$pkgname-$pkgver"
  # NOTE: set LWS_BUILD_HASH explicitely as it can otherwise only be set using
  # git
  cmake -D CMAKE_INSTALL_PREFIX=/usr \
        -D CMAKE_BUILD_TYPE='Release' \
        -D LWS_BUILD_HASH="no_hash" \
        -D LWS_IPV6=ON \
        -D LWS_LINK_TESTAPPS_DYNAMIC=ON \
        -D LWS_WITH_ACME=ON \
        -D LWS_WITH_DISKCACHE=ON \
        -D LWS_WITH_EXTERNAL_POLL=ON \
        -D LWS_WITH_FTS=ON \
        -D LWS_WITH_GLIB=ON \
        -D LWS_WITH_HTTP2=ON \
        -D LWS_WITH_HTTP_PROXY=ON \
        -D LWS_WITH_LIBEV=ON \
        -D LWS_WITH_LIBEVENT=OFF \
        -D LWS_WITH_LIBUV=ON \
        -D LWS_WITH_LWSAC=ON \
        -D LWS_WITH_RANGES=ON \
        -D LWS_WITH_SOCKS5=ON \
        -D LWS_WITH_STATIC=OFF \
        -D LWS_WITH_THREADPOOL=ON \
        -D LWS_WITH_ZIP_FOPS=ON \
        -D LWS_WITHOUT_BUILTIN_GETIFADDRS=ON \
        -D LWS_WITHOUT_BUILTIN_SHA1=ON \
        -D LWS_WITHOUT_CLIENT=OFF \
        -D LWS_WITHOUT_SERVER=OFF \
        -D LWS_WITHOUT_TESTAPPS=ON \
        -D LWS_WITHOUT_TEST_CLIENT=ON \
        -D LWS_WITHOUT_TEST_PING=ON \
        -D LWS_WITHOUT_TEST_SERVER=OFF \
        -D LWS_WITHOUT_TEST_SERVER_EXTPOLL=ON \
        -D LWS_UNIX_SOCK=ON \
        -Wno-dev \
        -B build \
        -S .
  make VERBOSE=1 -C build
}

package() {
  cd "$pkgname-$pkgver"
  make DESTDIR="${pkgdir}" install -C build
  install -vDm 644 LICENSE -t "${pkgdir}/usr/share/licenses/${pkgname}"
}
