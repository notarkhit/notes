# fix GPGME ERROR in pacman

> this can also happen on a new archlinux [[installation]]

to fix the pacman error where the database is out of sync:

```shell
sudo rm /var/lib/pacman/sync/*
sudo rm /var/lib/pacman/sync/* -rf
reboot
```

> after rebooting, just update the system.  

```shell
 sudo pacman -Syu
```

>  Do this when getting [[[Database lock error]]].