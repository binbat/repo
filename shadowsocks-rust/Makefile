ARCH=amd64
VERSION=1.24.0
RELEASE=1

HASH=343f90ff9b9830f25dfb204157c43288a37dbf7d

ifeq ($(ARCH),amd64)
    TARGET := x86_64-unknown-linux-gnu
else ifeq ($(ARCH),arm64)
    TARGET := aarch64-unknown-linux-gnu
else
    $(error ❌ Unsupported ARCH=$(ARCH). Only Supported Arch: amd64 arm64)
endif

default: src build package

src:
	wget https://github.com/shadowsocks/shadowsocks-rust/releases/download/v${VERSION}/shadowsocks-v${VERSION}.${TARGET}.tar.xz
	mkdir -p src
	tar -xf shadowsocks-v${VERSION}.${TARGET}.tar.xz -C src
	wget -P src https://raw.githubusercontent.com/shadowsocks/shadowsocks-rust/${HASH}/debian/config.json
	wget -P src https://raw.githubusercontent.com/shadowsocks/shadowsocks-rust/${HASH}/debian/shadowsocks-rust-local%40.service
	wget -P src https://raw.githubusercontent.com/shadowsocks/shadowsocks-rust/${HASH}/debian/shadowsocks-rust-server%40.service

build: src
	NFPM_ARCH=${ARCH} \
	NFPM_VERSION=${VERSION} \
	NFPM_RELEASE=${RELEASE} \
	envsubst < ./shadowsocks-rust.yaml > src/shadowsocks-rust.yaml

package: src build
	nfpm pkg --packager deb --config src/shadowsocks-rust.yaml

clean:
	rm -r src
	rm shadowsocks-v${VERSION}.${TARGET}.tar.xz
	rm shadowsocks-rust_${VERSION}-${RELEASE}_${ARCH}.deb

