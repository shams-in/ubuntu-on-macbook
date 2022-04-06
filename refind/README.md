# install from macos

## disable SIP (system integrity protection)

1. Restart your computer in Recovery mode Command (⌘)-R.
2. Launch Terminal from the Utilities menu.
3. Run the command `$ csrutil disable`.
4. Restart your computer.

## disable secure boot

https://support.apple.com/en-us/HT208198

1. Restart your computer in Recovery mode Command (⌘)-R.
2. Utilities > Startup Security Utility.
3. Set 'Secure boot' > 'No Security'
4. Set 'Allowed boot media' > 'Allow booting from external or removable media'

## 1. create partition

    200MB -> MS-DOS FAT
    label: REFIND

## 2. move zip to downloads dir

    $ cp refind-bin-0.13.2.zip ~/Downloads

## 3. execute below commands

```sh
IDENTIFIER=$(diskutil info REFIND | grep "Device Identifier" | cut -d: -f2 | xargs)
cd ~/Downloads
unzip refind-bin*
rm refind-bin*.zip
cd refind-bin*
xattr -rd com.apple.quarantine .
sed -i '' "s/sed -i 's/sed -i '' 's/g" refind-install
diskutil unmount $IDENTIFIER
sudo ./refind-install --usedefault /dev/$IDENTIFIER
diskutil unmount $IDENTIFIER
diskutil mount $IDENTIFIER
sudo rmdir /tmp/refind_install
rm -r ~/Downloads/refind-bin*
```

## 4. change the label for `rEFInd` from `EFI Boot` in mac startup manager

    $ bless --folder /Volumes/REFIND/EFI/BOOT --label rEFInd

## 5. copy drivers

    'EFI/BOOT/drivers_x64' -> '/Volumes/REFIND/EFI/BOOT/drivers_x64'

## 6. copy theme

    'EFI/BOOT/theme' -> '/Volumes/REFIND/EFI/BOOT/theme'

## 7. copy config

    'EFI/BOOT/refind.conf' -> '/Volumes/REFIND/EFI/BOOT/refind.conf'

## 8. manual boot menu for mac os

[IMP] add dirvers to refind to support mac file system

    `/usr/standalone/i386/apfs.efi` --> `EFI/BOOT/drivers_x64/apfs.efi`

```sh
    $ diskutil list

    #    /dev/disk1 (synthesized):
    #:                       TYPE NAME                    SIZE       IDENTIFIER
    #    0:      APFS Container Scheme -                      +567.1 GB   disk1
    #                                  Physical Store disk0s2
    #    1:                APFS Volume Macintosh HD            24.0 GB    disk1s1
    #    2:              APFS Snapshot com.apple.os.update-... 24.0 GB    disk1s1s1
    #    3:                APFS Volume Macintosh HD - Data     399.0 GB   disk1s2       <-- root dir
    #    4:                APFS Volume Preboot                 331.3 MB   disk1s3
    #    5:                APFS Volume Recovery                1.1 GB     disk1s4

    $ diskutil info disk1s2 | grep 'Partition UUID'
    # Disk / Partition UUID:          A2C2AA97-9D7B-43BF-A24A-894AFB3D9533    <-- add this to loeader

    # final config
    dont_scan_files +,"Preboot:/A2C2AA97-9D7B-43BF-A24A-894AFB3D9533/System/Library/CoreServices/boot.efi"
    menuentry "MacOSX" {
        icon /EFI/BOOT/theme/icons/os_mac.png
        volume "Preboot"
        loader /A2C2AA97-9D7B-43BF-A24A-894AFB3D9533/System/Library/CoreServices/boot.efi
        ostype "MacOS"
    }

```

## 9. maunal boot into ubuntu (bypass Grub)

    from `rEFInd -> Grub -> Ubuntu` to `rEFInd -> Ubuntu`

[IMP] add drivers to rEFInd to support ext4 filesystem

    EFI/BOOT/drivers_x64/ext4_x64.efi

[Note] below commands are executed on live ubuntu

```sh

# Ubuntu partition structure
$ lsblk -o name,uuid,partuuid,size
nvme0n1p1                                                                                   /boot/efi   {apple boot manager}
nvme0n1p4   xxx                                     8CAC2830-F7B0-44A5-B661-F75AD7C2DF8B    /boot       {ubuntu boot partition}     # volume in menuentry
nvme0n1p5   8b1d70a6-3e2a-4207-88e4-a1aea0305152                                            /           {ubuntu root partition}     # root in menuentry

# 1. execute below commands as root
$ sudo su

# 2. mount partitions
$ mount /dev/nvme0n1p5 /mnt
$ mount /dev/nvme0n1p4 /mnt/boot
$ mount /dev/nvme0n1p1 /mnt/boot/efi

# 3. (NEW) don't show boot menus from efi dir
# hide default boot loaders
# a94a9b51-3da8-40f0-8125-aa52b35c7d1a -> partuuid of 'EFI' partition
dont_scan_files +,"a94a9b51-3da8-40f0-8125-aa52b35c7d1a:/EFI/ubuntu/grubx64.efi"
dont_scan_files +,"a94a9b51-3da8-40f0-8125-aa52b35c7d1a:/EFI/BOOT/BOOTX64.EFI"

# 3. (OLD) delete `boot` and `ubuntu` dir from apple boot
$ cd /mnt/boot/efi/EFI
$ rm -rf BOOT ubuntu

# remove efi mount from fstab
$ nano /mnt/etc/fstab
# comment out /boot/efi mount point

[IMP]
    volume -- PARTITION UUID (PARTUUID)
    root=uuid -- UUID

# menuentry
dont_scan_files +,"8CAC2830-F7B0-44A5-B661-F75AD7C2DF8B:/"
menuentry "Ubuntu" {
    icon /EFI/BOOT/theme/icons/os_ubuntu.png
    volume "8CAC2830-F7B0-44A5-B661-F75AD7C2DF8B"
    loader vmlinuz-5.17.1-t2
    initrd initrd.img-5.17.1-t2
    options "root=UUID=8b1d70a6-3e2a-4207-88e4-a1aea0305152 ro efi=noruntime intel_iommu=on iommu=pt pcie_ports=compat nomodeset quiet splash vt.handoff=7"
    ostype "Linux"
}

# file tree
/EFI
    /BOOT
    /ubuntu
/boot
    /vmlinuz-5.17.1-t2
    /initrd.img-5.17.1-t2
/REFIND
    /EFI
        /BOOT
            /theme
                /icons
                    /os_ubuntu.png
```
