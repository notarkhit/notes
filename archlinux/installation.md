# Arch linux installation guide

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

#### Pertitioning schme

| Partition | Partition           | Size                      | File System          |
|-----------|---------------------|---------------------------|----------------------|
| /boot     | /dev/esp            | 1 GiB                     | EFI System Partition |
| swap      | /dev/swap-partition | 16 GiB                    | Linux Swap           |
| / (root)  | /dev/root-partition | Remainder of the device   | Linux File System    |

### Format the partitions 

```bash

mkfs.fat -F 32 /dev/esp  
mkfs.ext4 /dev/root-partition  
mkswap /dev/swap-partition  

```

### Mount the file systems

```bash

mount /dev/root-partition /mnt  

mount --mkdir /dev/esp /mnt/boot  

swapon /dev/swap-partition  

```

### Update [[mirrorlist]]

```bash

sudo pacman -Sy
sudo pacman -S pacman-contrib

cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup

rankmirrors -n 6 /etc/pacman.d/mirrorlist.backup > /etc/pacman.d/mirrorlist

```

## 2. Installation

### Install essential packages

```bash

pacstrap -K /mnt base linux linux-firmware base-devel

```
## 3. Configure the system

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
ln -s /usr/share/zoneinfo/Asia/Kolkata  /etc/localtime

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

### enable 32 bin support

> uncomment multilib from /etc/pacman.conf then synchronize database

### SET ROOT PASSWORD and add user

```bash
# set root password
passwd

#add user
useradd -m -g users -G wheel,storage,power -s /bin/bash username
passwd usename
```

### edit sudoers file

> uncomment #%wheel ALL=(ALL) ALL

### install bootloader

> for installing rEFInd instead go to [[rEFInd]]

verify boot mode

```bash
mount -t efivarfs efivarfs /sys/firmware/efi/efivars/
ls /sys/firmware/efi/efivars/
```

install bootloader

```bash
vim /boot/loader/entries/arch.conf
```

> add the following to arch.conf

title Arch  
linux /vmlinux-linux  
initrd /initramfs-linux.img  

>add root partuuid  to arch.conf

```bash
echo "options root=PARTUUID=$(blkid -s PARTUUID -o value /dev/root-partition) rw" >> /boot/loader/entries/arch.conf
```

### configure internet

enable dhcpcd

```bash
 sudo pacman -S dhcpcd
 sudo systemctl enable dhcpcd@wlan0.service
```

enable network manager

```bash
sudo pacman -S networkmanager
sudo systemctl enable networkmanager.service
```

### Install and configure NVIDIA drivers

```bash
sudo pacman -S linux-headers

sudo pacman -S nvidia-dkms libglvnd nvidia-utils opencl-nvidia lib32-libglvn lib-32-nvidia-utils lib32-opencl-nvidia nvidia-settings
```

### Add mkinitcpio modules

> edit mkinitcpio.conf 

```bash
vim /etc/mkinitcpio.conf
```
add the following modules in this specific order to mkinitcpio.conf

```ini
MODULES = (nvidia nvidia_modeset nvidia_uvm nvidia_drm )
```

### Edit /boot/loader/entries/arch.conf
add this afer rw in options

```
options = root=PARTUUID={root partition uuid} rw nvidia-drm.modeset=1
```

### pacman hooks for nvidia

```bash
mkdir /etc/pacman.d/hooks/
vim /etc/pacman.d/hooks/nvidia.hook
```

> add the following update hooks to /etc/pacman.d/hooks/nvidia.hook

```ini
[Trigger]
Operation=Install
Operation=Upgrade
Operation=Remove
Type=Package
# Uncomment the installed NVIDIA package
Target=nvidia
#Target=nvidia-open
#Target=nvidia-lts
# If running a different kernel, modify below to match
Target=linux

[Action]
Description=Updating NVIDIA module in initcpio
Depends=mkinitcpio
When=PostTransaction
NeedsTargets
Exec=/bin/sh -c 'while read -r trg; do case $trg in linux*) exit 0; esac; done; /usr/bin/mkinitcpio -P'
```

## 4. Reboot

```bash
exit
umount -R /mnt
reboot
```

