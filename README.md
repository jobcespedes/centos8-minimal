[![Docker Repository on Quay](https://quay.io/repository/krestomatio/centos8-minimal/status "Docker Repository on Quay")](https://quay.io/repository/krestomatio/centos8-minimal)

This is a Centos 8 minimal container image like Fedora-minimal or UBI. TESTING only

# How rootfs is generated from this repo
```bash
# variables
export TYPE=minimal
export VERSION=8
export OS_DISTRO=centos
export KS_FILE=centos8-minimal.ks
export ROOTFS=centos8-minimal.tar.xz

export OS_RELEASE="${OS_DISTRO}${VERSION}"
export CTR_NAME=${OS_RELEASE}-${TYPE}

# create and enter container
docker run -ti --privileged -e VERSION -e CTR_NAME \
    -e KS_FILE -e ROOTFS -v "$PWD:/repo:z" \
    --name livemedia-${OS_RELEASE} \
    ${OS_DISTRO}:${VERSION} bash

#### inside container ####
# install tools
dnf install -y anaconda anaconda-tui lorax

# create rootfs inside container
livemedia-creator --logfile="/tmp/${CTR_NAME}.log" \
    --make-tar --ks="/repo/${KS_FILE}" --no-virt \
    --image-name="${ROOTFS}"

mv /var/tmp/${ROOTFS} /repo/${ROOTFS}

# clean anaconda installation
# anaconda-cleanup
exit
#### container out ####
```

# Build image
```bash
docker build .
```
