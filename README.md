# piprox-arch
Install notes for installing arch linux on pi proxmox


fdisk -l
fdisk /dev/sda
n,p,+512M
t,ef
n,p,w
mkfs.fat -F32 /dev/sda1
mkfs.ext4 /dev/sda2
pacman -Syy
mount /dev/sda2 /mnt
pacstrap /mnt base linux linux-firmware vim nano
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt

timedatectl list-timezones
timedatectl set-timezone America/New_York

nano /etc/locale.gen
# uncomment en_US.UTF-8 UTF-8

locale-gen
echo LANG=en_US.UTF-8 > /etc/locale.conf
export LANG=en_US.UTF-8

echo myarch > /etc/hostname

touch /etc/hosts
127.0.0.1	localhost
::1		localhost
127.0.1.1	myarch

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



