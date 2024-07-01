# voidlinux

## GDM
1. xbps-install nvidia
2. nvidia-drm.modeset=1 to kernel parameters in /etc/default/grub. Then run update-grub
3. /etc/dracut.conf.d/nvidia.conf - add_drivers+=" nvidia nvidia_modeset nvidia_uvm nvidia_drm "
4. dracut -f
5. xbps-install xorg-minimal gnome
