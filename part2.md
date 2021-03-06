* Add new user `useradd -m -G wheel nam`
* Set password `passwd nam`
* Connect to internet `nmcli device wifi list` then `nmcli device wifi connect {SSID} password {password}`
* Install some packages:
```
sudo pacman -S git
sudo pacman -S nvidia
sudo pacman -S xf86-video-intel
sudo pacman -S xorg-xinit
sudo pacman -S libxft # FreeType-based font drawing library for X
sudo pacman -S xorg-xdpyinfo # Display information utility for X
sudo pacman -S xbindkeys
sudo pacman -S xorg-xinput
sudo pacman -S xorg-xbacklight
sudo pacman -S alsa-utils # Sound
sudo pacman -S openssh # ssh-keygen
```
* Update kernel parameter `sudo vim /boot/loader/entries/arch.conf` and add `options mem_sleep_default=deep`
* Install `optimus-manager` for graphics switching between NVIDIA and Intel
```
optimus-manager --switch hybrid   # Use hybrid graphics (Requires xorg-server >= 1.20.6-1)
optimus-manager --set-startup hybrid
```
* Install build source file management tool `sudo pacman -S asp`
* Re-install VIM `asp checkout vim` 
