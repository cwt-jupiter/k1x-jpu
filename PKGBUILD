# Maintainer: Chaiwat Suttipongsakul <cwt@bashell.com>

pkgname=k1x-jpu
pkgdesc="JPEG Process Unit API for SpacemiT K1-x"
pkgver=2.0.2
pkgrel=2
arch=('riscv64')
url="https://gitee.com/bianbu-linux/k1x-jpu"
license=('BSD-3-Clause')
_tag=v${pkgver}
_srcname=${pkgname}-${_tag}
makedepends=(cmake gcc)
options=('!debug')
source=("${_srcname}.tar.gz::${url}/repository/archive/${_tag}.tar.gz"
        '01-fix-implicit-gettimeofday.patch'
	'udev-jpu.rules')

b2sums=('48ae003f5e4a3f9d19376df4b8274f5542b1185cf1f74343845fd931e13b618e5c2bea8ab441738302f7b4f87e172495b2680ad789dc9bc9d03762768b728505'
        '80947104face14b485c74212fc7d783b399808be1ec3dc7039445af2443172b6141029e9d6c5fd742035d51478814d094fd865ec8c5ea8dd6e1d2051d433629a'
        'f3f956921bd432d4d44cd8467d878ff2de2df225da059897032cf1c3d75639b8b35f1e38735f9d31e8d309a64a39828e203423599831ef4262fd6c239e606a2a')

prepare() {
    cd $_srcname

    local src
    for src in $(ls ../*.patch); do
      echo "Applying patch $src..."
      patch -Np1 < "../$src"
    done

    cmake -B build -DCMAKE_POLICY_VERSION_MINIMUM=3.5 -DCMAKE_INSTALL_PREFIX=${pkgdir}/usr
}

build() {
    cd $_srcname
    cmake --build build
}

package() {
    cd $_srcname
    cmake --install build
    install -Dm644 ../udev-jpu.rules ${pkgdir}/etc/udev/rules.d/99-${pkgname}.rules
    install -Dm644 LICENSE ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
}

post_install() {
    udevadm control --reload-rules
    udevadm trigger
}
