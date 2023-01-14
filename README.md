# arch-ntfs-img-boot

This repo provides two initcpio hooks (`mountntfs`, `loopfile`) for booting Arch Linux from an image file in NTFS filesystem.

## Usage

1. Prepare for a working Arch Linux OS root in an image file.
    1. It is mounted by `losetup /dev/loop0 arch.img && mount /dev/loop0 /mnt`
    1. `/boot` should be in a partition outside the image file. Otherwise, the GRUB bootloader may fail to find it. (TODO: use GRUB loopback to support `/boot` in file)

1. (Chroot to the OS in file)

1. Copy the files in `initcpio` into `/etc/initcpio/`.

1. Add initcpio hooks `mountntfs` `loopfile`.
    1. Edit `/etc/mkinitcpio.conf`, in `HOOKS`, add `mountntfs loopfile` after `block`.
    1. Rebuild initcpio (`mkinitcpio -P`).

1. Add kernel cmdline parameters.
    1. (GRUB bootloader for example) 
    1. Edit `/etc/default/grub`
    1. Add `mountntfs=<NTFS_DEV>:/ntfs loopfile=/ntfs/arch.img` in cmdline.  
       (This mounts `<NTFS_DEV>` and maps the `arch.img` file as `/dev/loop0`.)
    1. Run `grub-mkconfig -o /boot/grub/grub.cfg`
