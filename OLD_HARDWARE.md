cfdisk /dev/sda
mkfs.ext4 /dev/sda1
mkfs.ext4 /dev/sda2
mount /dev/sda2 /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
lsblk
pacstrap /mnt base base-devel linux linux-firmware vim
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt /bin/bash
pacman -S networkmanager grub
systemctl enable NetworkManager
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
passwd
vim /etc/locale.gen
/en_US (then uncomment en_US.UTF-8 + ISO)
locale-gen
vim /etc/locale.conf (LANG=en-US.UTF-8)
vim /etc/hostname (type in the name of you PC)
ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime
exit
umount -R /mnt
reboot
