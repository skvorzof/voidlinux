# voidlinux

## Wifi
1. wpa_cli
2. add_network
3. set_network 0 ssid "gate"
4. set_network 0 key_mgmt WPA-PSK
5. set_network 0 pairwise CCMP
6. set_network 0 psk "password"
7. enable_network 0

## Partition + btrfs
1. lsblk
2. cfdisk /dev/nvme0n1
3. part /boot/efi - 256M, part / - all
4. mkfs.vfat /dev/nvme0n1p5
5. mkfs.btrfs /dev/nvme0n1p6
6. mount /dev/nvme0n1p6 /mnt
7. btrfs su cr /mnt/@
8. btrfs su cr /mnt/@home
9. unount /mnt
10. mount -o noatime,discard=async,compress=zstd,space_cache=v2,subvol=@ /dev/nvme0n1p6 /mnt
11. mkdir /mnt/home
12. mount -o noatime,discard=async,compress=zstd,space_cache=v2,subvol=@home /dev/nvme0n1p6 /mnt/home
13. mkdir -p /mnt/boot/efi
14. mount /dev/nvme0n1p4 /mnt/boot/efi

## Base install
1. REPO=https://repo-default.voidlinux.org/current
2. ARCH=x86_64
3. mkdir -p /mnt/var/db/xbps/keys
4. cp /var/db/xbps/keys/* /mnt/var/db/xbps/keys/
5. XBPS_ARCH=$ARCH xbps-install -S -r /mnt -R "$REPO" base-system nano
6. xchroot /mnt /bin/bash
7. echo void > /etc/hostname
8. nano /etc/default/libc-locales
9. xbps-reconfigure -f glibc-locales
10. passwd
11. cp /proc/mounts /etc/fstab
12. edit tmpfs /tmp
13. xbps-install grub-x86_64-efi NetworkManager
14. grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id="Void"
15. xbps-reconfigure -fa
16. exit
17. umount -R /mnt
18. shutdown -r now

## Post install
1. ln -s /etc/sv/dbus /var/service/
2. ln -s /etc/sv/NetworkManager /var/service/
3. ln -s /etc/sv/bluetoothd /var/service
4. nmtui

## GDM
1. xbps-install void-repo-nonfree
2. xbps-install -Su
3. xbps-install nvidia
4. nvidia-drm.modeset=1 to kernel parameters in /etc/default/grub. Then run update-grub
5. /etc/dracut.conf.d/nvidia.conf - add_drivers+=" nvidia nvidia_modeset nvidia_uvm nvidia_drm "
6. dracut -f
7. xbps-install xorg-minimal gnome xdg-desktop-portal xdg-desktop-portal-gtk xdg-user-dirs xdg-user-dirs-gtk xdg-utils gnome-browser-connector
8. shutdown -r now
9. touch /etc/sv/gdm/down
10. ln -s /etc/sv/gdm /var/service/
11. sv once gdm
12. xbps-install noto-fonts-emoji noto-fonts-ttf noto-fonts-ttf-extra
13. ln -s /usr/share/fontconfig/conf.avail/70-no-bitmaps.conf /etc/fonts/conf.d/
14. xbps-reconfigure -f fontconfig

## Wayland
1. mkdir /etc/udev/rules.d/
2. ln -s /dev/null /etc/udev/rules.d/61-gdm.rules

## Network
1. xbps-install gvfs-smb

## Audio
1. xbps-install pulseaudio pulseaudio-utils pulsemixer alsa-plugins-pulseaudio

## Backup
1. xbps-install timeshift

## App
### Librewolf
1. echo 'repository=https://github.com/index-0/librewolf-void/releases/latest/download/' > /etc/xbps.d/20-librewolf.conf
2. xbps-install -Su librewolf
