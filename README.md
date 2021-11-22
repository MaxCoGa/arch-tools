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
pacstrap /mnt base base-devel linux linux-firmware vim nano
````

Generate a fstab file
````shell
genfstab -U /mnt >> /mnt/etc/fstab
````

Enter mounted disk
````shel
arch-chroot /mnt
````

Enter hostname. Change name for an hostname
````shel
echo name > /etc/hostname
````

Set up root password
````shel
passwd
````

Set up Grub Grub bootloader
````shel
pacman -S grub efibootmgr networkmanager
````
````shel
systemctl enable NetworkManager
````

````shel
mkdir /boot/efi
````

mount efi partition
````shel
mount /dev/nvme0n1p1 /boot/efi
````

install grub
````shel
grub-install --target=x86_64-efi --bootloader-id=GRUB --efi-directory=/boot/efi
````
````shell
grub-mkconfig -o /boot/grub/grub.cfg
````


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