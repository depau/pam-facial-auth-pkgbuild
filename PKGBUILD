pkgname=pam-facial-auth-git
pkgver=r21.1184b9c
pkgrel=1
pkgdesc="PAM facial authentication for Linux"
arch=(i686 x86_64)
url="https://github.com/devinaconley/pam-facial-auth"
license=('MIT')
depends=('opencv' 'hdf5' 'pam')
makedepends=('git' 'cmake')
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
replaces=()
backup=("etc/pam.d/${pkgname%-git}")
#install=
source=("$pkgname::git+https://github.com/devinaconley/pam-facial-auth.git" 'fix-build.patch')
noextract=()
md5sums=('SKIP' 'de97e305432420c88a8965b1a7141b36')

pkgver() {
        cd "$pkgname"
        printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {     
        cd "$srcdir/$_distdir/$pkgname"
        mkdir -p build
        patch -Np1 < ../fix-build.patch
}

build() {
        cd "$srcdir/$_distdir/$pkgname/build"
        cmake ..
        make
}

package() {
        cd "$srcdir/$_distdir/$pkgname/build"
        mkdir -p $pkgdir/usr/bin
        mkdir -p $pkgdir/usr/lib/security
        mkdir -p $pkgdir/etc/pam.d
        mkdir -p $pkgdir/etc/pam-facial-auth

        install -Dm755 run_test $pkgdir/usr/bin/pam_facial_auth_run_test
        install -Dm755 run_training $pkgdir/usr/bin/pam_facial_auth_run_training
        install -Dm755 facialauth.so $pkgdir/usr/lib/security/facialauth.so

        install -Dm644 /dev/stdin "$pkgdir/etc/pam.d/${pkgname%-git}" <<END
#-------pam_test config---------#
auth sufficient facialauth.so
account sufficient facialauth.so
END


}

