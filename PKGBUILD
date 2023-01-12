# Maintainer: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>

pkgname=nginx-quic-mod-cache_purge
pkgver=2.5.2
pkgrel=3

_modname="${pkgname#nginx-quic-mod-}"
if [[ $CC=="clang" ]];then
    _cc_opt="-flto"
    _ld_opt="-flto -fuse-ld=lld"
fi

pkgdesc='Nginx module with ability to purge content from FastCGI, proxy, SCGI and uWSGI caches'
arch=('x86_64')
depends=('nginx')
makedepends=('nginx-quic-src')
url="https://github.com/nginx-modules/ngx_cache_purge"
license=('CUSTOM')

source=(
	https://github.com/nginx-modules/ngx_cache_purge/archive/$pkgver/ngx_cache_purge-$pkgver.tar.gz
)

validpgpkeys=(
	'B0F4253373F8F6F510D42178520A9993A1C052F8' # Maxim Dounin <mdounin@mdounin.ru>
)

sha256sums=('552ff1b9a8bcf77b21093b0e2e59a86852870ffda8c97af8ca9422ccd90ccd5f')

prepare() {
	mkdir -p build
	cd build
	ln -sf /usr/src/nginx/auto
	ln -sf /usr/src/nginx/src
}

build() {
	cd build
	/usr/src/nginx/configure --with-compat --add-dynamic-module=../ngx_cache_purge-$pkgver --with-cc-opt="$_cc_opt" --with-ld-opt="$_ld_opt"
	make modules
}

package() {
	install -Dm644 "$srcdir"/ngx_cache_purge-$pkgver/LICENSE \
	               "$pkgdir"/usr/share/licenses/$pkgname/LICENSE

	cd build/objs
	for mod in *.so; do
		install -Dm755 $mod "$pkgdir"/usr/lib/nginx/modules/$mod
	done
}
