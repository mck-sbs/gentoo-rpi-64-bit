# gentoo-rpi-64-bit
Building a gentoo 64-bit system for Raspberry Pi 4

# Preparing the host
I'm using a linux machine with Ubuntu 20.04. You need to install the following packages:
```
sudo apt-get install build-essential gawk gcc g++ gfortran git texinfo bison libncurses-dev flex libssl-dev gcc-aarch64-linux-gnu g++-aarch64-linux-gnu

```
Create a folder, here rpi-64
```
cd 
mkdir rpi-64

```

# Get the sources
```
cd rpi-64
git clone --depth=1 https://github.com/raspberrypi/firmware -b stable 
git clone --depth=1 https://github.com/raspberrypi/linux -b rpi-5.15.y
wget https://bouncer.gentoo.org/fetch/root/all/releases/arm/autobuilds/20220222T223655Z/stage3-armv7a-openrc-20220222T223655Z.tar.xz
wget https://mirror.leaseweb.com/gentoo/snapshots/portage-latest.tar.bz2

```
