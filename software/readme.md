## Software
### Steam
EDIT: /etc/pacman.conf <br>

Change: <br>
/# [multilib] <br>
/# Include = /etc/pacman.d/mirrorlist <br>
[multilib] <br>
Include = /etc/pacman.d/mirrorlist <br>

sudo pacman -Syu <br>
sudo pacman -S ttf-liberation <br>
sudo pacman -S steam <br>




Open .AppImage:<br>
sudo pacman -S fuse2 <br>
chmod a+x NAME.AppImage <br>





Minecraft-Launcher: <br>
sudo pacman -S jre-openjdk <br>
sudo pacman -S jdk-openjdk <br>
mkdir games <br>
cd games <br>
git clone https://aur.archlinux.org/minecraft-launcher.git  <br>
makepkg -si <br>
