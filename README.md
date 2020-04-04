# Arch
Vanilla arch installation

* Check UEFI `efivar -l`
* Connect to Internet `wifi-menu`
* Enable NTP `timedatectl set-ntp true`
* Disk partition: 512M EFI System, the rest on Linux Filesystem `cfdisk /dev/nvme0n1`
* LUKS encrypt `cryptsetup luksFormat /dev/nvme0n1p2`
* Decrypt to LVM `cryptsetup open /dev/nvme0n1p2 cryptlvm`
* Create physical volume `pvcreate /dev/mapper/cryptlvm`
* Create volume group `vgcreate MyVolGroup /dev/mapper/cryptlvm`
* Create logical volumes
```bash
lvcreate -L 8G MyVolGroup -n swap
lvcreate -L 32G MyVolGroup -n root
lvcreate -l 100%FREE MyVolGroup -n home
```
* Format filesystems
```bash
mkfs.ext4 /dev/MyVolGroup/root
mkfs.ext4 /dev/MyVolGroup/home
mkswap /dev/MyVolGroup/swap
```
* Mount filesystems
```bash
mount /dev/MyVolGroup/root /mnt
mkdir /mnt/home
mount /dev/MyVolGroup/home /mnt/home
swapon /dev/MyVolGroup/swap
```
* Boot partition
```bash
mkfs.fat -F32 /dev/nvme0n1p1
mkdir /mnt/boot
mount /dev/sdb1 /mnt/boot
```
* Install kernel and firmware `pacstrap /mnt base base-devel linux linux-firmware`
* Generate fstab `genfstab -U /mnt >> /mnt/etc/fstab`
* chroot `arch-chroot /mnt`
* Set timezone `ln -sf /usr/share/zoneinfo/Australia/Melbourne /etc/localtime`
* Adjust time `hwclock --systohc`
* Install Vim `pacman -S vim`
* Edit file `vim /etc/locale.gen` and uncomment `en_US.UTF-8 UTF-8`
* Generate locale `locale-gen`
* Create file `vim /etc/locale.conf` and insert `LANG=en_US.UTF-8`
* `echo “arch” > /etc/hostname`
* Install lvm2 package `pacman -S lvm2`
* Add encrypt and lvm2 `vim /etc/mkinitcpio.conf`
* Creating a new initramfs `mkinitcpio -P`
* Set root password `passwd`
* Install systemd-boot loader `bootctl install`
* Create file `vim /boot/loader/entries/arch.conf` with following content
```bash
title Arch Linux
linux /vmlinuz-linux
initrd /initramfs-linux.img
options cryptdevice=UUID={UUID}:cryptlvm root=/dev/MyVolGroup/root quiet rw
```
Run inside Vim `:read ! blkid /dev/nvm -s UUID -o value /dev/nvme0n1p2` to get UUID
* Install NetworkManager before reboot 
```bash
pacman -S networkmanager
systemctl enable NetworkManager
```
* Reboot
```bash
exit
umount -R /mnt
reboot
```
