# arch-tools
installation, update, desktop environment

## Download
https://archlinux.org/download/

## Installation

### Wifi setup
Open iwctl
````shell
iwctl
````
Get the device name (e.g. wlan0)
````shell
device list
````
Get your network name
Change wlan0 to your device name
````shell
station wlan0 scan
station wlan0 get-networks
````
Connect to your network using your device name.
If your network name contain space(s), use this format "network name"
````shell
station wlan0 connect networkname
````
Exit iwctl
````shell
exit
````
Verify connectity
````shell
ping gnu.org
````

### Steps

````shell
timedatectl set-ntp true
````

List all your disks and partitions(under device)
````shell
fdisk -l
````
Find your disk name, e.g. /dev/nvme0n1. You will need to use this device name to create the first two partitions<br>
Make sure to not use your usb key disk name.<br>

Using your disk name. It will open an app.
````shell
fdisk /dev/nvme0n1
````
From the fdisk utility press p and enter to list all the partition on this disk.
e.g. /dev/nvme0n1p1
````shell
/dev/nvme0n1p1
/dev/nvme0n1p2
/dev/nvme0n1p3
````
Delete all the existing partition from this disk.
If there is no partition skip this part. 
1. Press d and enter.
1. Press enter.
1. Restart until there is no partiton left

Create an EFI partition.(UEFI system)
1. Press n and enter.
1. For the partition number Press enter or Press 1 and then enter.
1. First Sector, press enter.
1. Last Sector, type 128M and press enter.
1. Press t and enter.
1. Selected Partition, Press 1 and enter.
1. Partition type, Press 1 and enter.
> You should see  Changed type of partition "Linux filesystem" to "EFI system".
Press w and enter.
> EFI partition name e.g. /dev/nvme0n1p1

Create a root partition using cfidsk.
````shell
cfdisk /dev/nvme0n1
````
1. Select free space with the up and down arrow.
1. Choose new with the right and left arrow and press enter.
1. Type enter to use all the remaining space.
1. Use the right and left arrow to select write and press enter.
1. Then select quit and press enter.
> root partition name e.g. /dev/nvme0n1p2

Check if you have efi
````shell
ls /sys/firmware/efi/efivars
````

Create filesystem for the EFI partition
````shell
mkfs.vfat /dev/nvme0n1p1
````
Create filesystem for the root partition
````shell
mkfs.ext4 /dev/nvme0n1p2
````


Sync pacman
````shell
pacman -Syy
````

Mount to your root partition
````shell
mount /dev/nvme0n1p2 /mnt
````

Install necessary packages using pacstrap. vim and nano are both included here
````shell
pacstrap /mnt base base-devel linux linux-firmware linux-headers vim nano
````

Generate a fstab file
````shell
genfstab -U /mnt >> /mnt/etc/fstab
````

Enter mounted disk
````shell
arch-chroot /mnt
````

Enter hostname. Change name for an hostname
````shell
echo name > /etc/hostname
````

Set up root password
````shell
passwd
````

Set up Grub Grub bootloader
````shell
pacman -S grub efibootmgr networkmanager
````
````shell
systemctl enable NetworkManager
````

````shell
mkdir /boot/efi
````

mount efi partition
````shell
mount /dev/nvme0n1p1 /boot/efi
````

install grub
````shell
grub-install --target=x86_64-efi --bootloader-id=GRUB --efi-directory=/boot/efi
OR
grub-install /dev/sda
````
````shell
grub-mkconfig -o /boot/grub/grub.cfg
````

update packages - warning
````shell
pacman -Syu
````

````shell
pacman -S sudo
````

Create a user
````shell
useradd -m username
````
Set the password the new username
````shell
passwd username
````

````shell
visudo /etc/sudoers
````
Add username ALL=(ALL) ALL below root ALL=(ALL) ALL and save.

### Desktop Environment
````shell
pacman -Syuu
````

install x server
````shell
pacman -S xorg
````

````shell
pacman -S plasma
````

````shell
pacman -S plasma-wayland-session
````

Minimal
````shell
pacman -S konsole sddm plasma-nm plasma-pa dolphin kdeplasma-addons kde-gtk-config
````

Optionnals
````shell
pacman -S kde-applications
````
NVIDIA
````shell
pacman -S nvidia nvidia-settings nvidia-utils
````

````shell
systemctl enable sddm.service
````

````shell
systemctl enable NetworkManager.service
````

NVIDIA
````shell
systemctl enable NetworkManager.service
````

### IMPORTANT
Exit arch-root, root should become red
````shell
exit
````

````shell
umount -R /mnt
````

````shell
reboot
````
> Reboot from the disk!


## Packages
````shell
sudo pacman -S git neofetch
````

Install snap 
````shell
git clone https://aur.archlinux.org/snapd.git
cd snapd
makepkg -si
sudo systemctl enable --now snapd.socket
sudo ln -s /var/lib/snapd/snap /snap
````

Install Brave
````shell
sudo snap install brave
````

Install Discord
````shell
sudo snap install discord
````
