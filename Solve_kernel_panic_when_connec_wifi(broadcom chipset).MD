## Solution if having trouble "KERNEL PANIC" when you connect to internet through wifi (BROADCOM CHIPSET) ##



Install broadcom-wl

```
sudo pacman -S broadcom-wl
```

create file .conf in /etc/modprobe.d

```
blacklist b43
blacklist b43legacy
blacklist ssb
blacklist bcm43xx
blacklist brcm80211
blacklist brcmfmac
blacklist brcmsmac
blacklist bcma
```

For chipset bcm4313

if use LTS kernel , install linux-lts-headers

```
pacman -S linux-lts-headers
```

then, install broadcom-wl-dkms in Aur

```
yay -S broadcom-wl-dkms
```
