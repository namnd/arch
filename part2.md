* Add new user `useradd -m -G wheel nam`
* Set password `passwd nam`
* Connect to internet `nmcli device wifi list` then `nmcli device wifi connect {SSID} password {password}`
* Install some packages:
```
sudo pacman -S git
sudo pacman -S nvidia
sudo pacman -S xorg-xinit
```
* Update kernel parameter `sudo vim /boot/loader/entries/arch.conf` and add `options mem_sleep_default=deep`
