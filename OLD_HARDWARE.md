cfdisk /dev/sda <br>
mkfs.ext4 /dev/sda1 <br>
mkfs.ext4 /dev/sda2<br>
mount /dev/sda2 /mnt<br>
mkdir /mnt/boot<br>
mount /dev/sda1 /mnt/boot<br>
lsblk<br>
pacstrap /mnt base base-devel linux linux-firmware vim<br>
genfstab -U /mnt >> /mnt/etc/fstab<br>
arch-chroot /mnt /bin/bash<br>
pacman -S networkmanager grub<br>
systemctl enable NetworkManager<br>
grub-install /dev/sda<br>
grub-mkconfig -o /boot/grub/grub.cfg<br>
passwd<br>
vim /etc/locale.gen<br>
/en_US (then uncomment en_US.UTF-8 + ISO)<br>
locale-gen<br>
vim /etc/locale.conf (LANG=en-US.UTF-8)<br>
vim /etc/hostname (type in the name of you PC)<br>
ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime<br>
exit<br>
umount -R /mnt<br>
reboot<br>
