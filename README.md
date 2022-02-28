# gentoo-rpi-64-bit
Building a gentoo 64-bit system for Raspberry Pi 4. I'm using a laptop with Ubuntu 20.04 as host machine. You need following packages installed:
```
sudo apt-get install build-essential gawk gcc g++ gfortran git texinfo bison libncurses-dev flex libssl-dev gcc-aarch64-linux-gnu g++-aarch64-linux-gnu
```

# Get the sources
```
cd 
mkdir rpi-64
cd rpi-64
git clone --depth=1 https://github.com/raspberrypi/firmware -b stable 
git clone --depth=1 https://github.com/raspberrypi/linux -b rpi-5.15.y
wget https://bouncer.gentoo.org/fetch/root/all/releases/arm/autobuilds/20220222T223655Z/stage3-armv7a-openrc-20220222T223655Z.tar.xz
wget https://mirror.leaseweb.com/gentoo/snapshots/portage-latest.tar.bz2
```
# Preparing the SD card
as decribed here [here](https://wiki.gentoo.org/wiki/Raspberry_Pi_3_64_bit_Install) and [here](https://wiki.gentoo.org/wiki/Raspberry_Pi/Quick_Install_Guide) (gentoo wiki).

# Installation
## Mounting the partitions
```
sudo mkdir /mnt/gentoo
sudo mount /dev/mmcblk0p3 /mnt/gentoo
sudo mkdir /mnt/gentoo/boot/
sudo mount /dev/mmcblk0p1 /mnt/gentoo/boot
```

## Extracting files
```
sudo tar xpfv stage3-armv7a-openrc-20220222T223655Z.tar.xz --xattrs-include='*.*' --numeric-owner -C /mnt/gentoo/
sudo tar xvpf portage-latest.tar.bz2 --strip-components=1 -C /mnt/gentoo/usr
```
## Compiling and installing kernel, modules and device tree
```
cd linux
ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- make distclean
ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- make bcm2711_defconfig
ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- make -j4 Image modules dtbs
sudo ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- make modules_install INSTALL_MOD_PATH=/mnt/gentoo
cd ..

sudo cp -vr firmware/boot/* /mnt/gentoo/boot/
sudo cp -vr firmware/modules /mnt/gentoo/lib
sudo cp -vr linux/arch/arm64/boot/dts/overlays/*.dtbo /mnt/gentoo/boot/overlays
sudo cp -v linux/arch/arm64/boot/dts/broadcom/bcm2711-rpi-4-b.dtb /mnt/gentoo/boot/
sudo cp -v linux/arch/arm64/boot/Image /mnt/gentoo/boot/kernel8.img
```
# Configurtion

## ToDo fstab
## ToDo cmdline.txt
## ToDo config.txt
## ToDo inittab 

List the timezones:
```
ls /mnt/gentoo/usr/share/zoneinfo
```

and choose one:
```
sudo cp /mnt/gentoo/usr/share/zoneinfo/Europe/Berlin /mnt/gentoo/etc/localtime
sudo echo "Europe/Berlin" > /mnt/gentoo/etc/timezone
```

clrear root password
```
sudo sed -i 's/^root:.*/root::::::::/' /mnt/gentoo/etc/shadow
```

set keymap
```
sudo sed -i 's/^keymap="us"/keymap="de"/' /mnt/gentoo/etc/conf.d/keymaps
```

# Unmount SD
```
sudo umount /mnt/gentoo/boot
sudo umount /mnt/gentoo
```

# References
https://wiki.gentoo.org/wiki/Raspberry_Pi_3_64_bit_Install

https://wiki.gentoo.org/wiki/Raspberry_Pi4_64_Bit_Install

https://github.com/sakaki-/gentoo-on-rpi-64bit

https://wiki.gentoo.org/wiki/Raspberry_Pi/Quick_Install_Guide

https://deardevices.com/2019/04/18/how-to-crosscompile-raspi/



