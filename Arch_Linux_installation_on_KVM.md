# 🚀 Arch Installation on kvm Step Guide 🚀

### check internet connection

```
ping archlinux.org
```

### Update the system clock \[Update the system clock\]

```
timedatectl set-ntp true
```

### Partition the Disk

- Identify device use `lsblk`
- partition disk use `cfdisk`

create 1 partition as swap (min 512mb) create 1 partition as / , choose
bootable, and write

### Format the partitions

- for the root partitions :
    
    `mkfs.ext4 /dev/your_root_partition (i.e sda1, vda1, etc)`
    
- for swap partition :
    
    `mkswap /dev/your_swap_partition ( i.e sda2, vda2, etc )`
    
    `swapon /dev/your_swap_partition`
    

### Mount the file systems

```
    mount /dev/your_root_partition  /mnt
```

* * *

## Installation

**install text editor :**

```
sudo pacman -S vim
```

edit mirror server in `/etc/pacman.d/mirrorlist` to get better connection
for server indonesia we can add this mirror

```
## Indonesia mirror list 
   ## Arch Linux repository mirrorlist
   ## Filtered by mirror score from mirror status page
   ## Generated on 2021-02-19
   ##

   ## Indonesia
   #Server = http://vpsmurah.jagoanhosting.com/archlinux/$repo/os/$arch
   ## Indonesia
   #Server = http://mirror.papua.go.id/archlinux/$repo/os/$arch
   ## Indonesia
   #Server = https://mirror.papua.go.id/archlinux/$repo/os/$arch
   ## Indonesia
   #Server = https://vpsmurah.jagoanhosting.com/archlinux/$repo/os/$arch
   ## Indonesia
   #Server = http://mirror.faizuladib.com/archlinux/$repo/os/$arch
   ## Indonesia
   #Server = https://mirror.gi.co.id/archlinux/$repo/os/$arch
   ## Indonesia
   #Server = http://mirror.gi.co.id/archlinux/$repo/os/$arch
   ## Indonesia
   #Server = http://mirror.labkom.id/archlinux/$repo/os/$arch
```

### Install essential packages

```
pacstrap /mnt base linux linux-firmware
```

* * *

## Configure the system

**Fstab** Generate an fstab file :

```
genfstab -U /mnt >> /mnt/etc/fstab
```

**Chroot** Change root into the new system:

```
arch-chroot /mnt
```

**Time zone** Set time zone :

```
ln -sf /usr/share/zoneinfo/YourRegion/YourCity  /etc/localtime
```

Run *hwclock* to generate `/etc/adjtime`

```
hwclock --systohc
```

### **Localization**

- Edit `/etc/locale.gen` and uncomment `en\_US.UTF-8 UTF-8` and other
    needed locales, then generate the locales by running :
    
    ```
    locale-gen 
    ```
- Create the **locale.conf** file and
    set the LANG variable :
    `LANG=en_US.UTF-8`
    

### **Network configuration**

- Create the hostname file `vim /etc/hostname`:
    
    ```
    yourHostName
    ```
- add entries to hosts file in /etc/hosts:
    

```
127.0.0.1   localhost
::1         localhost
127.0.1.1   yourHostName.localdomain    yourHostName
```

- **install** `dhcpcd`

```
$ sudo pacman -S dhcpcd

```
- **enable & start service** `dhcpcd`

```
$ sudo systemctl enable dhcpd
$ sudo systemctl start dhcpd


```
### **Set Root Password**

```
passwd
```
### **Users and Groups**

- Create your username
	```
    useradd -m  yourusername
    ```
- Set Password for your username:
	```
    passwd yourusername
    ```
- add user to sudoers:
    - first install sudo:
        
        ```
        pacman -S sudo
        ```
    - add your user to sudoers:
        
        ```
        usermod -aG wheel,audio,video,optical,storage yourUsername
        ```
    - edit sudoers file :
        
        ```
        visudo
        ```
    - edit and uncomment line `%wheel ALL=(ALL) NOPASSWD: ALL` to get a root privilleges
        

### **Install Boot Loader**

```
# sudo pacman -S grub 

# grub-install /dev/xda

# grub-mkconfig -o /boot/grub/grub.cfg 
```


`exit` to exit a chroot

`shutdown` for shutdown arch in kvm

shutdown the kvm and turn-on back again the kvm

**For install KDE plasma :**

```
sudo pacman -S xorg plasma konsole 
```

**For install and using Xmonad :**
```
$ sudo pacman -S xorg xmonad xmonad-contrib xmobar dmenu
