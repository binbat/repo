ARCH=amd64
VERSION=0.68.1
RELEASE=1

default: src build package

src:
	wget https://github.com/fatedier/frp/releases/download/v${VERSION}/frp_${VERSION}_linux_${ARCH}.tar.gz
	mkdir -p src
	tar -xf frp_${VERSION}_linux_${ARCH}.tar.gz -C src --strip-components=1

build: src
	NFPM_ARCH=${ARCH} \
	NFPM_VERSION=${VERSION} \
	NFPM_RELEASE=${RELEASE} \
	envsubst < ./frps.yaml > src/frps.yaml
	NFPM_ARCH=${ARCH} \
	NFPM_VERSION=${VERSION} \
	NFPM_RELEASE=${RELEASE} \
	envsubst < ./frpc.yaml > src/frpc.yaml

package: src build
	nfpm pkg --packager deb --config src/frps.yaml
	nfpm pkg --packager deb --config src/frpc.yaml

clean:
	rm -r src
	rm frp_${VERSION}_linux_${ARCH}.tar.gz
	rm frps_${VERSION}-${RELEASE}_${ARCH}.deb
	rm frpc_${VERSION}-${RELEASE}_${ARCH}.deb

