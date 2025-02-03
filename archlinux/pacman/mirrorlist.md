# Sorting mirrors

> ranking existing mirrorlist

```bash
#
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup

```

To prepare mirrorlist.backup for ranking with rankmirrors, the following actions can be carried out:

Edit mirrorlist.backup and uncomment the servers to be tested

If the servers in the file are grouped by country, one can extract all the servers of a specific country by using:

```bash
$ awk '/^## Country Name$/{f=1; next}f==0{next}/^$/{exit}{print substr($0, 1);}' /etc/pacman.d/mirrorlist.backup
``` 

To uncomment every mirror, run the following sed line: 

```bash
#
sed -i 's/^#Server/Server/' /etc/pacman.d/mirrorlist.backup
```

Finally, rank the mirrors, here with the operand -n 6 to only output the 6 fastest mirrors: 

```bash
#
rankmirrors -n 6 /etc/pacman.d/mirrorlist.backup > /etc/pacman.d/mirrorlist
```

## Fetching and ranking a live mirror list

In order to start with a shortlist of up-to-date mirrors based in some countries and feed it to rankmirrors one can fetch the list from the Pacman Mirrorlist Generator. The command below pulls the up-to-date mirrors in either France or the United Kingdom which support the https protocol, it uncomments the servers in the list and then ranks them and outputs the 5 fastest. 

```bash
$ curl -s "https://archlinux.org/mirrorlist/?country=FR&country=GB&protocol=https&use_mirror_status=on" | sed -e 's/^#Server/Server/' -e '/^#/d' | rankmirrors -n 5 -
```

## This method can also be used on a new archlinux [[installation]] 