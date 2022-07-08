# piprox-arch

[![Discord](https://img.shields.io/discord/316245914987528193?logo=discord)](https://discord.com/invite/v8dAnFV) [![Youtube](https://img.shields.io/badge/YouTube-FF0000?style=flat-square&logo=youtube&logoColor=white)](https://www.youtube.com/channel/UCrjKdwxaQMSV_NDywgKXVmw) [![Twitter URL](https://img.shields.io/twitter/follow/novaspirittech?style=flat-square&logo=twitter)](https://twitter.com/novaspirittech)

Install notes for installing arch linux on pi proxmox

video - [Installing Arch Linux on Pi Proxmox](https://youtu.be/GvwD9pXYJi4}

## Instructions

Formatting the drive.

### Listing all the disk

~~~~bash
fdisk -l
~~~~

### Selecting the disk

~~~~bash
fdisk /dev/sda
~~~~

### partioning the EFI disk

~~~~
n,p,+512M
t,ef
~~~~

### partitioning the Main disk

~~~~
n,p,w
~~~~

### Formating the EFI disk to Fat32

~~~~bash
mkfs.fat -F32 /dev/sda1
~~~~

### Formating the Main disk to EXT4

~~~~bash
mkfs.ext4 /dev/sda2
~~~~

pacman -Syy

mount /dev/sda2 /mnt

pacstrap /mnt base linux linux-firmware vim nano

genfstab -U /mnt >> /mnt/etc/fstab

arch-chroot /mnt

timedatectl list-timezones

timedatectl set-timezone America/New_York

nano /etc/locale.gen

uncomment en_US.UTF-8 UTF-8

locale-gen

echo LANG=en_US.UTF-8 > /etc/locale.conf

export LANG=en_US.UTF-8

echo myarch > /etc/hostname

touch /etc/hosts

```127.0.0.1	localhost
::1		localhost
127.0.1.1	myarch
```

passwd

pacman -S grub efibootmgr

mkdir /boot/efi

mount /dev/sda1 /boot/efi

grub-install --target=aarch64-efi --bootloader-id=GRUB --efi-directory=/boot/efi

grub-mkconfig -o /boot/grub/grub.cfg

pacman -S sudo

useradd -m username

passwd username

usermod -aG wheel,audio,video,storage username

visudo

uncomment %wheel ALL=(ALL:ALL) ALL

pacman -S xorg networkmanager

pacman -S gnome / xfce4 xfce4-googdies lightdm

systemctl enable gdm.service lightdm.service

systemctl enable NetworkManager.service

exit

umount /mnt

umount -l /mnt

shutdown now



