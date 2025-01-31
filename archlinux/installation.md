# Arch linux install guide

> This is a guide for my own reference for a more detailed guide go to the [Archlinux installation guide](https://wiki.archlinux.org/title/Installation_guide)

This document is a guide for installing Arch Linux using the live system booted from an installation medium made from an official installation image.

Before installing, it would be advised to view the [FAQ](https://wiki.archlinux.org/title/Frequently_asked_questions). For conventions used in this document, see [Help:Reading](https://wiki.archlinux.org/title/Help:Reading). In particular, code examples may contain placeholders (formatted in italics) that must be replaced manually.

## 1. Pre installation

## connect to the internet

```shell

iwctl

device list

device name set-property Powered on
adapter adapter adapter set-property Powered on

station name scan
station name get-networks
station name connect SSID

exit

```

## Partition the disks

> partition the disks using gptfdisk 

## Format the partitions 

```shell

mkfs.fat -F 32 /dev/esp

mkfs.ext4 /dev/root-partition

mkswap /dev/swap-partition

```

## Mount the file systems

```shell

mount /dev/root-partition

mount --mkdir /dev/esp /mnt/boot

swapon /dev/swap-partition

```



