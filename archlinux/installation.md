# Arch linux install guide

> This is a guide for my own reference for a more detailed guide go to the [Archlinux installation guide](https://wiki.archlinux.org/title/Installation_guide)

This document is a guide for installing Arch Linux using the live system booted from an installation medium made from an official installation image.

Before installing, it would be advised to view the [FAQ](https://wiki.archlinux.org/title/Frequently_asked_questions). For conventions used in this document, see [Help:Reading](https://wiki.archlinux.org/title/Help:Reading). In particular, code examples may contain placeholders (formatted in italics) that must be replaced manually.

## 1. Pre installation

### connect to the internet

```bash

iwctl

device list

device name set-property Powered on
adapter adapter adapter set-property Powered on

station name scan
station name get-networks
station name connect SSID

exit

```

### Partition the disks

> partition the disks using gptfdisk 

### Format the partitions 

```bash

mkfs.fat -F 32 /dev/esp

mkfs.ext4 /dev/root-partition

mkswap /dev/swap-partition

```

### Mount the file systems

```bash

mount /dev/root-partition

mount --mkdir /dev/esp /mnt/boot

swapon /dev/swap-partition

```

### Update mirrorlist

```bash

sudo pacman -Sy
sudo pacman -S pacman-contrib

cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup

rankmirrors -n 6 /etc/pacman.d/mirrorlist.backup > /etc/pacman.d/mirrorlist

```

## Installation

### Install essential packages

```bash

pacstrap -K /mnt base linux linux-firmware base-devel

```
## Configure the system

### Fstab

```bash
genfstab -U -p /mnt >> /mnt/etc/fstab
```

### Chroot
change root into the new system

```bash
arch-chroot /mnt 
```
### generate locale

```bash
vim /etc/locale.gen
```
uncomment en_US.UTF-8 UTF-8

```bash
locale-gen
echo LANG=en_US.UTF-8 > /etc/locale.conf
export LANG=en_US.UTF-8
```

### set timezones

```bash
ln -s /usr/share/zoneinfo/Asia/Kolkata > /etc/localtime

hwclock --systohc --utc

```
### set hostname

```bash
echo "hostname" > /etc/hostname
```

### enable fstrim for ssd

```bash
systemctl enable fstrim.timer
```

### enable 32 bin suppoet

> uncommnet multilib from /etc/pacman.conf then synchronize database

### SET ROOT PASSWORD and add user

```bash
# set root password
passwd

#add user
useradd -m -g users -G wheel,storage,power -s /bin/bash username
passwd usename
```

### edit sudoers file

> uncomment wheel

### install bootloader

verify boot mode

```bash
mount -t efivarfs efivarfs
```
