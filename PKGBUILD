# Maintainer: Arisu  <alice at gaudon dot pro>
# Contributor: Sylwek  <skmtrn at gmail dot com>
# Contributor: Poscat <poscat at mail dot poscat dot moe>

pkgname=caddy2
_pkgname=caddy
pkgver=2.1.1
_tag=v2.1.1
_distcommit='a509155e3cff18af793f6af5f930a71c89e05df8'
pkgrel=3
pkgdesc="Fast web server with automatic HTTPS"
arch=('x86_64')
url="https://caddyserver.com"
license=('Apache')
depends=('glibc')
makedepends=('go' 'git')
provides=('caddy')
conflicts=('caddy')
backup=('etc/caddy/Caddyfile'
        'etc/caddy/caddy.conf')
source=("git+https://github.com/caddyserver/caddy#tag=${_tag}?signed"
        "index-${_distcommit}.html::https://raw.githubusercontent.com/caddyserver/dist/${_distcommit}/welcome/index.html"
        caddy.service
        caddy-api.service
        caddy.tmpfiles
        caddy.sysusers
        caddy.conf
        WarningCaddyfile)
sha512sums=('SKIP'
            '2abccd41f770daebf61285dc017249f20c707877ea3c870f4a2375bbbd2bf481a8652d1fd3c7afd7d6b5c54838e9d8474a33e2c9790ef67dcf9d79c4e52953b4'
            '69a619f2be6df77ac516621ad7b27dd6adde0b40cf4680c3afb25e94ac7c1cd6afb42a650d47a4fb096503dec85988af08e85a617b5290cf3a3d9b50b2e2ec46'
            'eeb352e023331a3e3d88e47cc52f8786864abeb66bccc864d2557722afd54bdfcee9781590c3cf204923c8c0fa37029ba417a6a65ccd5569f8cbc214eb3ce642'
            '41d6b82ab99ca729d5b48d77d4557f3c5368b63847054216018547ea40e0cc302bc36668e9a57ba63e8983205febffee18eae1c306a665365a13da54010cd415'
            'a9b3e4af421a4be0193d9a452cdf6b66b8f03fbf8bce3de4454a2ee70556c156c458b12a141fefb3e15a2acbb0f3acedc09cbccbecf82aa8916b9b6d799ec066'
            '399c177475e299bf5736dba1a9d045477072594390b73c7dd2e13e12785abe029d48e480aee98216f7a8735addd353ce4a07b56ed14364f641b138e4fca0ebcd'
            'e195d235a8893d27277a88554a8786e7aec2ca6c83855e051adbdfe5463409463cd159aac2db09786270259be9280eafa0a0ed5dfd2e04d587c8136856ea4d4c')
validpgpkeys=(
  '29D0817A67156E4F25DC24782A349DD577D586A5' # Matthew Holt <mholt@users.noreply.github.com>
)

pkgver() {
  cd ${_pkgname}
  git describe --tags --match 'v*' | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  sed 's|/var/www/html|/srv/http|g' -i "${srcdir}/index-${_distcommit}.html"
  sed 's|/etc/caddy/Caddyfile|/etc/caddy/caddy.conf|g' -i "${srcdir}/index-${_distcommit}.html"
}

build() {
  cd "${_pkgname}/cmd/caddy/"
  export CGO_LDFLAGS="${LDFLAGS}"
  export CGO_CPPFLAGS="${CPPFLAGS}"
  export CGO_CFLAGS="${CFLAGS}"
  export CGO_CXXFLAGS="${CXXFLAGS}"
  export GOFLAGS="-buildmode=pie -trimpath"
  go build .
}


check() {
  cd "${_pkgname}"
  go test ./...
}

package() {
  cd "${_pkgname}"
  install -Dm 755 cmd/caddy/caddy -t "${pkgdir}/usr/bin"
  install -Dm 644 "${srcdir}/caddy.service" -t "${pkgdir}/usr/lib/systemd/system"
  install -Dm 644 "${srcdir}/caddy-api.service" -t "${pkgdir}/usr/lib/systemd/system"
  install -Dm 644 "${srcdir}/caddy.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/caddy.conf"
  install -Dm 644 "${srcdir}/caddy.sysusers" "${pkgdir}/usr/lib/sysusers.d/caddy.conf"
  install -Dm 644 "${srcdir}/caddy.conf" "${pkgdir}/etc/caddy/caddy.conf"
  install -d "${pkgdir}/etc/caddy/conf.d"
  install -Dm 644 "${srcdir}/index-${_distcommit}.html" "${pkgdir}/usr/share/caddy/index.html"
  install -Dm 644 "${srcdir}/WarningCaddyfile" "${pkgdir}/etc/caddy/Caddyfile"
}
