# boot into live ubuntu

### partition structure

    $ lsblk -o name,uudi,size
    nvme0n1p1                                           /boot/efi   {apple boot manager}
    nvme0n1p4   8CAC2830-F7B0-44A5-B661-F75AD7C2DF8B    /boot       {ubuntu boot partition}
    nvme0n1p5   8b1d70a6-3e2a-4207-88e4-a1aea0305152    /           {ubuntu root partition}

# Install grub (from live os)

    $ sudo su

    # partitions
    $ mount /dev/nvme0n1p5 /mnt
    $ mount /dev/nvme0n1p4 /mnt/boot
    $ mount /dev/nvme0n1p1 /mnt/boot/efi
    $ mount -o bind /dev /mnt/dev
    $ mount -o bind /sys /mnt/sys
    $ mount -o bind /proc /mnt/proc

    $ df -h

    $ chroot /mnt # required for live os

    $ grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB --no-nvram --removable
    $ grub-mkconfig -o /boot/grub/grub.cfg

# update grub

    # modify grub config
    $ nano /etc/default/grub
    $ update-grub
