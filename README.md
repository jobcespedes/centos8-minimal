[![Build Status](https://travis-ci.org/jobcespedes/centos8-minimal.svg?branch=master)](https://travis-ci.org/jobcespedes/centos8-minimal) [![Buy me a coffee](https://img.shields.io/badge/$-BuyMeACoffee-blue.svg)](https://www.buymeacoffee.com/jobcespedes)

# build
```bash
# variables
export VERSION=8
export OS_DISTRO=centos
export OS_RELEASE="${OS_DISTRO}${VERSION}"
export TYPE=minimal
export CTR_NAME=${OS_RELEASE}-${TYPE}
export KS_FILE="${CTR_NAME}.ks"
export ROOTFS="${CTR_NAME}.tar.xz"
# create container
docker run -ti --privileged -e VERSION -e OS_DISTRO -e OS_RELEASE -e TYPE -e CTR_NAME -e KS_FILE -e ROOTFS -v "$PWD:/repo:z" --name livemedia-${OS_RELEASE} ${OS_DISTRO}:${VERSION} bash

# install
dnf install -y anaconda anaconda-tui lorax

# build inside container
livemedia-creator --logfile="/tmp/${CTR_NAME}.log" --make-tar --ks="/repo/${KS_FILE}" --image-name="${ROOTFS}" --no-virt

mv /var/tmp/${ROOTFS} /repo/${ROOTFS}

# clean anaconda installation
anaconda-cleanup
```
