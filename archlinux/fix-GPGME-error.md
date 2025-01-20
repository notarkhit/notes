# fix GPGME ERROR in pacman


to fix the pacman error where the database is out of sync:

```bash
sudo rm /var/lib/pacman/sync/*
sudo rm /var/lib/pacman/sync/* -rf
reboot
```

> after rebooting, just update the system.  

```bash
 sudo pacman -Syu
```
