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
4. mkfs.fat -F 32 /dev/nvme0n1p5
5. mkfs.btrfs /dev/nvme0n1p6
6. mount /dev/nvme0n1p6 /mnt
7. btrfs su cr /mnt/@
8. btrfs su cr /mnt/@home
9. unount /mnt
10. mount -o noatime,discard=async,compress=zstd,subvol=@ /dev/nvme0n1p6 /mnt
11. mkdir /mnt/home
12. mount -o noatime,discard=async,compress=zstd,subvol=@home /dev/nvme0n1p6 /mnt/home
13. mkdir -p /mnt/boot/efi
14. mount /dev/nvme0n1p4 /mnt/boot/efi

## GDM
1. xbps-install nvidia
2. nvidia-drm.modeset=1 to kernel parameters in /etc/default/grub. Then run update-grub
3. /etc/dracut.conf.d/nvidia.conf - add_drivers+=" nvidia nvidia_modeset nvidia_uvm nvidia_drm "
4. dracut -f
5. xbps-install xorg-minimal gnome

## Audio
1. xbps-install pulseaudio pulseaudio-utils pulsemixer alsa-plugins-pulseaudio
